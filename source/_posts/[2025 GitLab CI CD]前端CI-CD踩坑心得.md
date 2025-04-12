---
title: 前端 CI-CD 踩坑與 Cloudflare Zero Trust Access 使用心得
date: 2025-04-12 20:00:00
updated: 2025-04-12 20:00:00
categories: 心得
tags: 
- 2025
- remote_server
- CI/CD
- GitLab
- DevOps
---

前言：
> 這篇是專門對於 GitLab CI/CD 而寫的 Writeup，如果是 GitHub 的，僅供參考核心邏輯即可。
> 筆者第一次寫 CI/CD，如有誤或改進之處，可以跟我說 ><

---

## 目前專案與 remote server 的情境

* 專案
  * Nuxt3
* Server
  * Ubuntu 23.04 x86_64
  * 主機沒有直接對外的公網 IP，是透過 Cloudflare Tunnel 建立一個對外可訪問的通道

---

## Pipeline 結構

Pipeline 依序分為以下階段：

1. **dependencies**：安裝依賴套件。
2. **lint**：靜態分析與型別檢查。
3. **build**：建置 Nuxt 專案。
4. **deploy**：部署至指定開發環境(自己的 remote server)。

---

## 階段細節

### 1. dependencies

- **作用**：安裝與緩存專案依賴套件
- **指令**：

```bash
pnpm install
pnpm run prepare
```
- **Artifacts**：`.nuxt`

---

### 2. lint

- **作用**：確保程式碼符合規範且無型別錯誤
- **指令**：

```bash
pnpm lint
pnpm vue-tsc --noEmit
```

---

### 3. build_develop（僅限 develop 分支）

* Environment 可以在 `側邊欄 > Operate > Environments > New environment`

- **目的**：將 Nuxt 專案編譯為可部署的靜態檔案。
- **指令**：
```bash
pnpm build
```
- **Artifacts**：`.output/`
- **環境設定**：
```yaml
environment:
  name: develop
only:
  - develop
```

---

### 4. deploy_develop（僅限 develop 分支）

- **目的**：將建置好的檔案透過 Cloudflare Tunnel 與 SSH Proxy 安全部署至開發主機。

#### 部署環境前置步驟

- 安裝必備工具：
  - `openssh-client`
  - `cloudflared`
- 建立 SSH 金鑰

#### SSH 連線測試

透過 Cloudflare Tunnel 建立 SSH Proxy 並測試連線：
```bash
ssh -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
--service-token-id $CF_ACCESS_CLIENT_ID \
--service-token-secret $CF_ACCESS_CLIENT_SECRET" \
-o StrictHostKeyChecking=no \
$DEPLOY_USER@$DEPLOY_HOST "echo ✅ SSH via Cloudflare Access succeeded"
```

#### 部署步驟

- 將 artifacts 上傳至指定伺服器目錄：

```bash
scp -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
--service-token-id $CF_ACCESS_CLIENT_ID \
--service-token-secret $CF_ACCESS_CLIENT_SECRET" \
-o StrictHostKeyChecking=no \
-r .output/* $DEPLOY_USER@$DEPLOY_HOST:/var/www/nuxt-develop/
```

- 登入遠端主機並執行部署腳本：

```bash
ssh -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
--service-token-id $CF_ACCESS_CLIENT_ID \
--service-token-secret $CF_ACCESS_CLIENT_SECRET" \
-o StrictHostKeyChecking=no \
$DEPLOY_USER@$DEPLOY_HOST "bash -c \"
  cd /var/www/nuxt-develop &&
  pm2 delete nuxt-develop || true &&
  pm2 start ecosystem.config.js &&
  pm2 save\""
```

---

## 使用到的 GitLab CI 變數

* 確保以下變數已安全儲存在 GitLab CI 中：
  * `側邊欄 > Settings > CI/CD > Variables`
* 設定 Variable 時記得開啟 Mask & Protected

| 變數名稱                    | 說明                             | 範例                      |
|-----------------------------|----------------------------------|---------------------------|
| `DEPLOY_HOST`               | 部署伺服器域名                   | `example.com`             |
| `DEPLOY_USER`               | 部署伺服器登入使用者             | `serverusername`              |
| `CF_ACCESS_CLIENT_ID`       | Cloudflare Access 的服務 Token ID | `12345678.access`            |
| `CF_ACCESS_CLIENT_SECRET`   | Cloudflare Access 的服務密鑰     | `abcdef1234567890`        |
| `SSH_PRIVATE_KEY`           | SSH 私鑰                         | 儲存在 GitLab Secure 內   |

---

## 一些 Cloudflare Zero Trust Access 小科普

### 為什麼 SSH 和 SCP 需要 CF_ACCESS_CLIENT_ID 和 CF_ACCESS_CLIENT_SECRET？

#### 1. **Cloudflare Zero Trust 的身份驗證機制**

我的 `.gitlab-ci.yml` 中，`ssh` 和 `scp` 命令使用了 `cloudflared` 作為代理（`ProxyCommand`），Cloudflare 提供此工具以建立與 Cloudflare 網路的安全連線。

為了確保「只有授權的用戶或系統」可以存取我的 server，Cloudflare Zero Trust 要求進行身份驗證。

這時後就需要 `CF_ACCESS_CLIENT_ID` 和 `CF_ACCESS_CLIENT_SECRET` 這一對 Service Token，用來允許我的 CI/CD 流水線以程式化的方式存取受保護的資源，而不需要手動登錄身份提供者（Identity Provider, IdP）。

如果沒有這對 Service Token，就會在 job 執行中看到以下這個「要你點連結以登入」的互動式登入方法，這明顯與我們自動化背道而馳。

```bash
Please open the following URL and log in with your Cloudflare account:
https://aaa/cdn-cgi/access/cli?aud=xxx&edge_token_transfer=true&redirect_url=yyy&send_org_token=true&token=zzz
Leave cloudflared running to download the token automatically.
```

具體來說：
- **CF_ACCESS_CLIENT_ID**：就是一個公開的識別碼，用來標識 Service Token。
- **CF_ACCESS_CLIENT_SECRET**：就是一個秘密金鑰，與 `Client ID` 配對，用來驗證請求的合法性。

在我的腳本中，`cloudflared access ssh` 命令會使用這對 token 來與 Cloudflare 進行身份驗證：
```bash
ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
--service-token-id $CF_ACCESS_CLIENT_ID \
--service-token-secret $CF_ACCESS_CLIENT_SECRET
```
當 `cloudflared` 執行時，它會將這對 Token 發送到 Cloudflare，Cloudflare 會驗證 token 的有效性。如果驗證通過，Cloudflare 會生成一個臨時的 JSON Web Token（JWT），允許我的 `ssh` 或 `scp` 命令通過 Cloudflare 的網路連接到目標伺服器。

#### 2. **取代傳統 SSH 鑰匙的短暫憑證**
傳統的 SSH 連線通常依賴長期存在的 SSH 鑰匙（例如 `~/.ssh/id_rsa`）。然而，Cloudflare Zero Trust 引入了一個更安全的機制：使用短暫的 SSH 憑證（short-lived certificates）。這些憑證是由 Cloudflare 的憑證機構（CA）簽發的，並且有效期很短（通常幾小時），從而降低了長期憑證被盜用的風險。

在我的部署流程中，`CF_ACCESS_CLIENT_ID` 和 `CF_ACCESS_CLIENT_SECRET` 被用來向 Cloudflare 請求這些短暫憑證。Cloudflare 會根據我的身份（由 Service Token 驗證）和存取策略（Access Policy）來決定是否簽發憑證。如果策略允許，Cloudflare 會簽發一個短暫憑證，`cloudflared` 會使用這個憑證來完成與伺服器的 SSH 連線。

這種方法的好處是：
- **無需管理長期 SSH 鑰匙**：我不需要在伺服器上維護多個用戶的公鑰。
- **更高的安全性**：短暫憑證過期後就無效，即使被盜用，影響範圍也有限。

#### 3. **支援自動化系統**
我的 GitLab CI/CD 流水線是一個自動化系統，無法像人類用戶一樣通過瀏覽器登錄身份提供者（例如 Google 或 Okta）來進行身份驗證。`CF_ACCESS_CLIENT_ID` 和 `CF_ACCESS_CLIENT_SECRET` 提供了一種程式化的身份驗證方式，允許我的流水線以「服務身份」（Service Auth）的方式存取受保護的資源。

如果我沒有使用 Service Token，Cloudflare 會要求我的流水線通過身份提供者進行互動式登錄，這在 CI/CD 環境中是不切實際的。因此，Service Token 是自動化部署的關鍵。

這裡呼應上面第一點說的：
> 這個「要你點連結以登入」的互動式登入方法，這明顯與我們自動化背道而馳。

### Cloudflare Application 與 Policy 的關係

![alt text](/img/gitlabcicd-1.png)

上圖是 Cloudflare Zero Trust 的側邊欄

#### 1. **Cloudflare Application**
- **Cloudflare Application**
  - 在 Cloudflare Zero Trust 中，`Application` 是一個受保護的資源（例如我的 SSH Server）。

- **Application Config**：
  - 這裡需要指定目標伺服器的主機名 (例如 `xxx.subarya.me`)

- **Service Token 與 Application 的關係**：
  - 在 New Application 時，通常會配置存取策略（Access Policy），指定哪些用戶或服務可以存取這個應用。
  - 我的 Service Token（`CF_ACCESS_CLIENT_ID` 和 `CF_ACCESS_CLIENT_SECRET`）是與這個 Application 綁定的。當 `cloudflared` 使用這對 Token 進行身份驗證時，Cloudflare 會檢查該 Token 是否被授權存取這個特定的應用。
  - 如果 Token 有效，Cloudflare 會生成一個 JWT，允許我的流水線存取應用（即我的 SSH 伺服器）。

#### 2. **Cloudflare Policy**
Cloudflare Zero Trust 的存取策略（Access Policy）定義了誰（或什麼）可以存取我的應用，以及在什麼條件下可以存取。

- **Policy Config**：
  - 在 Cloudflare Zero Trust 儀表板中，我可以為我的 Application 創建一個 Policy，例如：
    - **動作（Action）**：設為 `Service Auth`，表示允許使用 Service Token (那一對 Client ID 和 Secret) 進行身份驗證。
    - **條件（Conditions）**：指定允許的 Service Token（例如我的 `CF_ACCESS_CLIENT_ID`）。
    - **其他條件**：我可以添加額外的條件，例如限制存取的 IP 範圍、設備姿態（Device Posture）等。
  - 例如，我可以配置一個 Policy，只允許特定的 Service Token 存取我的伺服器，而其他未授權的 Token 或用戶會被拒絕。

- **Policy 執行**：
  - 當我的流水線使用 `CF_ACCESS_CLIENT_ID` 和 `CF_ACCESS_CLIENT_SECRET` 進行身份驗證時，Cloudflare 會檢查這些 Token 「是否符合 Application 的 Access Policy」。
  - 如果符合，Cloudflare 會允許連線，並簽發短暫憑證給 `cloudflared`，用於與伺服器建立 SSH 連線。
  - 如果不符合（例如 Token 已過期或被撤銷），Cloudflare 會拒絕連線，並記錄一個「存取被拒」（Access Denied）事件。

---

## 使用與流程截圖

### 建立 Token

1. 進入 Cloudflare Zero Trust Access 的 服務驗證
2. 點選「建立服務 Token」

![alt text](/img/gitlabcicd-4.png)

3. 填寫完名稱和持續時間

![alt text](/img/gitlabcicd-5.png)

4. 第三點作完後點「產生 Token」並「自己存好用戶端 ID 和 密碼」後如下圖

![alt text](/img/gitlabcicd-6.png)

### 建立 Policy

1. 進入 Cloudflare Zero Trust Access 的 原則
2. 點選「新增原則」

![alt text](/img/gitlabcicd-7.png)

3. 輸入與選擇：
   1. 填寫原則名稱
   2. 動作選 Service Auth
   3. 下面新增規則處的「選取器」選擇「Service Token」
   4. 值 選擇 上面建立 Token 時取的名稱
   5. 確認新增規則處 3, 4 點都是使用「包含 OR」
   6. 右下角儲存

![alt text](/img/gitlabcicd-8.png)

4. 如果你的 Server 本身進入方法就是用 Tunnel 再看以下的解釋
   * 你會發現規則建立後，如果先去把 Application 串起來，你會發現你在本地端 ssh 進 remote server 時被擋在外面了
   * 原因是你在原本自己的裝置上應該在 `~/.ssh/config` 中有設定過...
   * ![alt text](/img/gitlabcicd-9.png)
   * 恭喜、你沒有那一對 Service Token
   * 因此你需要再建立一個 Policy，這次動作選 Allow (不是 Service Auth)
   * 選取器我是選 Email，然後填入你的 Email 即可 (選什麼看你本地端的需求是什麼)
   * ![alt text](/img/gitlabcicd-10.png)
   * 儲存後你就會得到下圖...

![alt text](/img/gitlabcicd-11.png)

### 建立 Application

1. 進入 Cloudflare Zero Trust Access 的 應用程式

![alt text](/img/gitlabcicd-2.png)

2. 點選 `加入應用程式 > 自我裝載的選取`

![alt text](/img/gitlabcicd-3.png)

3. 填完名稱與持續時間後
4. 點選「新增公用主機名稱」，然後填寫網域和子網域 (這裡看你要加入的是什麼，我是公用主機，這裡反而要去看你原本 Tunnel 或是其他是怎麼設定的)
5. 點選下面 Access 原則 的「選取現有規則」

![alt text](/img/gitlabcicd-12.png)

6. 選擇剛才建立的那兩條 Policy

![alt text](/img/gitlabcicd-13.png)

7. 一直右下角下一步到儲存

![alt text](/img/gitlabcicd-14.png)

### 結束工作

恭喜你完成「建立 Service Token」、「建立 Policy」、「建立 Application」啦！

接著把 Service Token 放入你的 GitLab CI/CD Variables 裡吧

---

## 後續此 CI/CD 有待擴充

- 建立其他分支流程（如 `staging`, `production`）
- 導入測試（如整合測試、E2E 測試）

---

## 附上我 Develop 版本的 gitlab-ci.yml

```yaml
stages:
  - dependencies
  - lint
  - build
  - deploy

image: node:22

variables:
  USER_NAME: ntnu_tow
  REPOSITORY_NAME: nuxt-3-ntnutow-dev

before_script:
  - npm install --location=global pnpm@9.15.0
  - pnpm config set store-dir .pnpm-store

default:
  cache: &cache
    key:
      files:
        - pnpm-lock.yaml
    paths:
      - .pnpm-store
      - ./node_modules
    policy: pull

install dependencies:
  stage: dependencies
  interruptible: true
  when: always
  cache:
    <<: *cache
    policy: push
  script:
    - pnpm install
    - pnpm run prepare
  artifacts:
    paths:
      - .nuxt
    expire_in: 1 hour

# Lint job 使用 node image
lint:
  stage: lint
  interruptible: true
  script:
    - echo "📝 Skipping tests (not implemented yet)"
    - echo "🔍 Running lint..."
    - pnpm lint
    - echo "🔍 Running TypeScript type check..."
    - pnpm vue-tsc --noEmit

build_develop:
  stage: build
  image: node:22
  interruptible: true
  cache:
    <<: *cache
    policy: pull
  dependencies:
    - install dependencies
  script:
    - echo "🏗️ Building Nuxt 3 project..."
    - pnpm build
  artifacts:
    paths:
      - .output/
    expire_in: 1 hour
  environment:
    name: develop
  only:
    - develop

deploy_develop:
  stage: deploy
  image: alpine:latest
  dependencies:
    - build_develop
  before_script:
    - apk add --no-cache openssh-client bash curl
    - command -v ssh-agent >/dev/null || ( apk add --update openssh )
    - curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -o /usr/local/bin/cloudflared
    - chmod +x /usr/local/bin/cloudflared
    - cloudflared --version
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa
    - chmod 600 ~/.ssh/id_rsa
    - eval "$(ssh-agent -s)"
    - ssh-add ~/.ssh/id_rsa
    - |
      ssh -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
      --service-token-id $CF_ACCESS_CLIENT_ID \
      --service-token-secret $CF_ACCESS_CLIENT_SECRET" \
      -o StrictHostKeyChecking=no \
      $DEPLOY_USER@$DEPLOY_HOST "echo ✅ SSH via Cloudflare Access succeeded"
  script:
    - echo "🚀 Deploying to ta.subarya.me (develop)..."
    - |
      scp -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
      --service-token-id $CF_ACCESS_CLIENT_ID \
      --service-token-secret $CF_ACCESS_CLIENT_SECRET" \
      -o StrictHostKeyChecking=no \
      -r .output/* $DEPLOY_USER@$DEPLOY_HOST:/var/www/nuxt-develop/
    - |
      ssh -o "ProxyCommand=/usr/local/bin/cloudflared access ssh --hostname $DEPLOY_HOST \
      --service-token-id $CF_ACCESS_CLIENT_ID \
      --service-token-secret $CF_ACCESS_CLIENT_SECRET" \
      -o StrictHostKeyChecking=no \
      $DEPLOY_USER@$DEPLOY_HOST "bash -c \"
        cd /var/www/nuxt-develop &&
        pm2 delete nuxt-develop || true &&
        pm2 start ecosystem.config.js &&
        pm2 save\""
  environment:
    name: develop
    url: $ENV_URL
  only:
    - develop
```