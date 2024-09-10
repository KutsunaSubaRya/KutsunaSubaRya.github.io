---
title: 踩 CORS 坑心得 - 從公司的新專案中遇到 CORS 到一步一步解決的心得分享
date: 2024-09-10 16:40:00
updated: 2024-09-10 16:40:00
categories: 心得
tags: 
- 2024
- CORS
- 實習
---

# Line 專案 CORS 踩坑紀錄

## 目的
這是我在公司中新開專案時踩到 CORS 的坑，希望對未來健忘的我或是看到這篇文章的你有所幫助。

## 前文引導
這是我們 CTO 希望有人還不理解什麼是 CORS 的人先去詳看的 Blog，總共有六個章節，寫的還蠻詳細的 (雖然我只有很粗淺略地看過，但我猜裡面的內容足以應付大多數的問題)

1. [CORS 完全手冊（一）：為什麼會發生 CORS 錯誤？](https://blog.huli.tw/2021/02/19/cors-guide-1/)
2. [CORS 完全手冊（二）：如何解決 CORS 問題？](https://blog.huli.tw/2021/02/19/cors-guide-2/)
3. [CORS 完全手冊（三）：CORS 詳解](https://blog.huli.tw/2021/02/19/cors-guide-3/)
4. [CORS 完全手冊（四）：一起看規範](https://blog.huli.tw/2021/02/19/cors-guide-4/)
5. [CORS 完全手冊（五）：跨來源的安全性問題](https://blog.huli.tw/2021/02/19/cors-guide-5/)
6. [CORS 完全手冊（六）：總結、後記與遺珠](https://blog.huli.tw/2021/02/19/cors-guide-6/)

那，就開始介紹這次專案中從我(前端)的視角出發的 CORS 之旅吧。

## CORS 情境
* 前端需注意
    1. 報 CORS 的 status code
        * 如果是 redirect: status code<30x>，有很大機率是 http redirect (重導向) 至 `https`。解法就是有 `https` 就打 `https`。(這裡指的是 api endpoint 的 URL scheme)
    2. 前去 `Dev Tool > Network > 欲檢索的封包 > Headers > Request Headers` 確認 referer 是否為空或是有誤。
    3. 前去 `Dev Tool > Network > 欲檢索的封包 > Headers > General` 確認 Referrer Policy 是否設定正確。
        * 正常來說目前皆是使用 `strict-origin-when-cross-origin`，這個也是正常網頁開發會優先採用的 Referer Policy。在目前此專案的情況 (前端: `http`、後端: `https`，且是 cross origin)，因為是 cross origin，我們會根據 `strict-origin` 策略進行，這專案的情況會是「升級」(`http` -> `https`)，而非「降級 downgrade」(`https` -> `http`)，因此「肯定會夾帶 Referer」。
        * 補充：沒記錯的話 Chrome 85 之前是採用 `no-referer-when-downgrade`，Chrome 85 後才是 `strict-origin-when-cross-origin`。
    4. 如果發封包前有做 preflight, 檢查其有沒有通過 (不知道為什麼造成 preflight 請搜尋相關關鍵詞)，若沒有通過請回到第一點檢查 status code。
        * 呈上，若該 api 同時包含「redirect」和「觸發 preflight 預檢，如自定義 header」，肯定會 preflight not pass，原因可以搜尋「preflight disallowed redirect cors」關鍵詞。
    5. 再次確認 target (target api endpoint 或是叫做 Request URL) 有沒有打對 ，比如說 https://<xxx.xx>/api/asdf 打成 https://<xxx.xx>/ 之類的。

* 觸發 CORS 時關鍵的 Status Code
    1. `405` (最為常見)
        * 大機率是 preflight 預檢沒過(基本上會是 HTTP Method OPTIONS)，可能要向後端確認是否有 OPTIONS。
    2. `307` (目前前端只有我這次事件遇到)
        * `https` 的請求改成 `http`。簡言之，後端有 `https` 就打 `https`，不要打 http。

* 回報給後端時必要附件(含截圖)
    1. Dev tool > Network > 封包
        1. Status Code
        2. General
        3. Request Headers
    2. Console
        1. 透過理解 Error Message 並將其歸約成精簡的問題方向，以確保與後端溝通問題時可以明確點出問題。(其實這個部分前端也要理解錯誤訊息在說什麼，比如說 Error Message 有寫道「`Access-Control-Allow-Origin` 報錯」還是「Preflight 報錯」之類的，這些都是可以歸納成「後端需要 allow 什麼」還是「請後端幫忙確認 HTTP Method OPTIONS」)

## 回顧當時串接 SSO API 時遇到 CORS 的情況

1. 我是前端，後端提供給我「login 機」的 API 串接
    * 過程中一直踩 CORS，最後發現 login 機是後端內部使用的，我們前端(外部)無法使用。
    * 於是改成提供我一支可以打的 api，後端內部將該 api 串接 login 機的 api。
    * 結果：可行。
2. 呈上，過程中遇到問題如下
    * 前端的問題與解方
        1. Request Headers 的 Referer 為空
            * 確認 Nuxt 的 fetch 是否有使用正確，如果使用方法有誤，可能會導致 refer 是空的情形，這裡筆者我無法 reprod 這個事件，但我很確定的我的 Referer Policy 是 `strict-origin-when-cross-origin` 且有在 `nuxt.config.ts` 中設定，但是發出封包後結果 Referer 是帶空值。最後我將 `$fetch` 的方法根據 [Nuxt 3 Doc](https://nuxt.com/docs/api/utils/dollarfetch) 重新架構後，就可以打出 Referer 了 (這裡推測是筆者我在亂寫 `$fetch` 導致 referer 沒有成功帶進 Request Headers)。
            * 最後發現根本不是 Referer Policy 出問題，而是上述的問題導致「沒有 referer」。
            * 補充 1：如果遇到 referer 為空會是不確定有沒有打通，可以先確認自己的封包是不是有先過 Cloudflare (之類的 [SaaS](https://www.cloudflare.com/zh-tw/learning/cloud/what-is-saas/)) 那一關，如果有可以向後端確認 Cloudflare 是否有觸發後端設置的機制。
            * 補充 2：如何定 Referer Policy (在 `nuxt.config.ts`)

        2. Request URL path 是否正確
            * 我這裡的狀況是一開始有 CORS 問題時 URL path 是對的，但中途塗塗改改的過程中我貌似有動到 path 的部分，導致有一段時間問題其實是出在前端這裡。
    * 後端的問題與解方
        1. 前端透過 API tool (如 Postman) 打 API 後發現噴 SQL 錯
            * 肯定請求後端協助
        2. Preflight not pass
            * 請後端確認 OPTIONS 是否有放行
        3. Console Error 顯示 `Access-Control-Allow-Origin` 報錯
            * 跟後端確認他們放行的 Method

以上是遇到的情況，也有可能是大多數人初次建立專案時會遇到的情況，以下我們要提到一個比較稀有的情況，也是這次專案 CORS 的大坑。

## 此次專案 CORS 大坑

### 直接用結論去描述

將後端 Middleware CORS 「手刻的部分」改成「現成的套件」，就沒有 CORS 問題了。這裡經過測試後發現的現象是 Middleware CORS 在簡單請求下可以過，但在非簡單請求(Preflight Request) 會報 CORS。檢測方法為分別製造簡單與非簡單請求，發現現象為簡單請求不會 CORS，但非簡單請求會被阻擋。

因此這裡提醒說如遇到相關事件時也不妨向後端確認 Middleware CORS 是否是使用現成套件 XD。
