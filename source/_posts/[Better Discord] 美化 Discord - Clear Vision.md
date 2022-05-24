---
title: Better Discord 美化 Discord - ClearVision
date: 2022-05-23 10:39:00
updated: 2022-05-23 10:39:00
categories: Better Discord
tags: 
- Better Discord
- 2022
---

先附上成品:

![](https://i.imgur.com/0tWRzM5.jpg)

## Getting Started

### Windows:

前往 Better Discord 官網下載 exe 檔案 : [官網連結](https://betterdiscord.app/)

### Arch Linux:

[AUR Arch Linux - betterdiscordctl](https://aur.archlinux.org/packages/betterdiscordctl)
```bash=
$ yay -S betterdiscordctl
```
install betterdiscordctl
```bash=
$ betterdiscordctl install
```
下載 PluginRepo : [PluginRepo 連結](https://betterdiscord.app/plugin/PluginRepo)
下載後將此 Plugin 放進 BetterDiscord 資料夾
```bash=
$ .config
$ BetterDiscord
$ mv ~/Downloads/*.plugin.* .
```

## 挑選 Theme

我十分推薦 [ClearVision - Theme](https://betterdiscord.app/theme/ClearVision)

主要原因是:
* 版面簡單
* 有網頁版的 [Theme Editor](https://bdeditor.dev/theme/clearvision) 可以做預覽，不須用自己改 `.css` 檔

## ClearVision Theme Editor

這裡來介紹此 Theme Editor 可修改的內容以及注意事項。

> 以下有提到 Fixed attachment 要設為 False 的項目依定要注意，因為此主題的 User popout 和 User profile 的背景圖片是對整個 Discord 去做中心調整，而非對兩者的 container 做調整，因此這裡的 Fixed attachment 要把預設的 True 改為 False。 by - 採坑的作者 ( SubaRya )

### 最左邊直排的欄位

1. Colours: 
    * 可選擇整體的版面顏色
2. App background:
    * 整個 Discord 背景圖片
3. User popout:
    * 側邊用戶點擊開的小介面
    * **Fixed attachment: False**
4. User profile:
    * 側邊用戶點小介面點開後詳細的大介面
    * **Fixed attachment: False**
5. Home button icon:
    * 左上角 Discord Icon 自定義
6. Channel colours:
    * 頻道文字已讀、未讀、靜音的文字顏色
7. Status colours:
    * 用戶狀態顏色自定義
8. Custom fonts:
    * 主要字體 (推薦 [Roboto](https://fonts.google.com/specimen/Roboto?query=Roboto))
    * Code Block 的字體 (推薦 [Fira Code](https://fonts.google.com/specimen/Fira+Code?query=Fira))
9. Others:
    * 寬度調整
10. Addons:
    * 伺服器換到上方橫排
    * 伺服器並排數
    * 用戶狀態改成環繞頭像

## 將 Theme 加入 Plugin

兩個步驟如下:
1. 從 Theme Editor 下載剛剛調好的 `.css` 檔
2. 去 Discord 點擊 `使用者設定` -> `Better Discord 的 佈景主題` -> `開啟佈景主題資料夾` -> `將剛剛載的 .css 檔案放入此資料夾` -> `開關打開`

## 其他有趣的 Plugin 
* [CallTimeCounter](https://betterdiscord.app/plugin/CallTimeCounter): 計算你連進語音經過多久時間
* [ShowHiddenChannels](https://betterdiscord.app/plugin/ShowHiddenChannels): 顯示伺服器被隱藏(不是你身分組的)的語音或文字頻道，但你還是無權限連線進去。
* [Typing Users Avatars](https://betterdiscord.app/plugin/Typing%20Users%20Avatars): 多個人在打字時可以看到正在打字那幾個人的頭像
* [Double Click To Edit](https://betterdiscord.app/plugin/Double%20Click%20To%20Edit): 對你已發送的訊息點兩下即可編輯
* [WhoReacted](https://betterdiscord.app/plugin/WhoReacted): 按此表情符號的用戶頭貼會顯示
* [OnlineFriendCount](https://betterdiscord.app/plugin/OnlineFriendCount): 左上角 Discord Icon 下顯示你有幾位朋友在線上

### Plugin 放入相對應資料夾

1. 去 Discord 點擊 `使用者設定` -> `Better Discord 的 附加元件` -> `開啟附加元件資料夾` -> `將剛剛載的 .js 檔案放入此資料夾` 
2. 過程中需要更新舊更新、要 download 就 download 
3. 開關打開