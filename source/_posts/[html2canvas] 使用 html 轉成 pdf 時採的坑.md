---
title: html2canvas - 使用 html 轉成 pdf 時採的坑
date: 2024-10-18 10:40:00
updated: 2024-10-18 10:40:00
categories: 心得
tags: 
- 2024
- pdf
- html2canvas
- html_to_pdf
- jspdf
- 實習
---

# html2canvas - 使用 html 轉成 pdf 時採的坑

## html 轉 pdf 詳解
以下兩篇十分推薦，基本上跟著思路與 codebase 就可以理解 DFS 拜訪 DOM 的原理和如何做一些細節的調整與處理。
* [jsPDF + html2canvas A4分页截断 完美解决方案（含代码 + 案例）](https://juejin.cn/post/7138370283739545613)
* [纯前端导出 pdf (Vue + Element Plus) 代码](https://gitee.com/jseven68/vue-pdf2)
## 問題發現 (html to canvas 時內容被往下 shift 一行)
經過一系列的排查後發現， html2canvas 和 Tailwind CSS 在 CSS 上會有部分衝突，進一步分析後只要在專案中使用 Tailwind CSS，Tailwind CSS 在 preflight 時會將 img 標籤的 display 屬性設置為 display: block，這會在 element 中引入一個換行。當然，在 頁面 render 時不會顯示出來。但當使用 html2canvas 將相同的 HTML 轉換為圖片時，這個換行就會會被渲染出來。

接著我同事跟我提出可以做 CSS reset 解決此問題。


## 解決方法
- Reference Link: [官方 Github issue 中有人提出的解法](https://github.com/niklasvh/html2canvas/issues/2775#issuecomment-1204988157)
  - 關鍵詞: `html2canvas Texts shifted down`
  - 研究後發現可以做到，在不影響全局 CSS 的情況下，可以使用以下方法在單個要產出 pdf 的 file 下做 img 的 reset CSS (preflight)。


```css
<style scoped> 
:global(img) { 
      display: inline-block !important; /* 覆寫 preflight 的 display */
} 
</style>
```

最後將此方法作為核心，改寫成「在 html2canvas 將 html 轉成 canvas 前」做 reset CSS。

```ts
async toCanvas(element, width) {
    ...
    
    // remove preflight img display format
    const style = document.createElement('style');
    document.head.appendChild(style);
    style.sheet?.insertRule('body > div:last-child img { display: inline-block; }');
    // canvas 元素
    let canvas = await html2canvas(element, {
        allowTaint: true, // 允許 render 內容有跨域圖片
        scale: this.scale || window.devicePixelRatio * 2,
        useCORS: true, // 允許跨域
    });
    // style remove
    style.remove();
    const canvasWidth = canvas.width;
    const canvasHeight = canvas.height;
    
    ...
}
```