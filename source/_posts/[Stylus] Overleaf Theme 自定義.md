---
title: 自定義 Overleaf
date: 2022-06-23 15:02:00
updated: 2022-06-23 15:02:00
categories: Plugin Template
tags:
- CSS
- Stylus
- 2022
---

參考自: [這裡](https://userstyles.org/styles/183490/dark-overleaf)

先放上成品:

![](https://i.imgur.com/KggqBu6.png)

![](https://i.imgur.com/tLtnmNU.png)

## 修改 Editor 

### Dark Mode

由於從 [Stylish](https://userstyles.org/) 上載下來的 CSS 是有瑕疵的，仔細觀察會發現左邊 Editor 的 Theme 沒有被修改成 Dark Mode。

利用 `filter invert` 負片(補色)效果將 Editor 變成 Dark Mode

```css
#editor {
    filter: invert(1);
}
```

### Editor 加上 Background Image

~~渴望油油嗎~~ OwO

此時將 background 加上後會發生背景圖片也被 invert 了 !!!

因此 invert 回來。

```css
.ace_scroller {
	background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url(https://i.imgur.com/6OR1oUa.jpg) no-repeat;
	background-size: cover;
    filter: invert(1);
}
```

### 將 Layer 層的 Text Invert 回來

你會發現增加 background 後加上 invert 會導致文字層也被補色，因此針對文字層做 invert。

```css
.ace_layer {
    filter: invert(1);
}
```

### 左側行號顏色改變

做到這了，你是否發現左邊行號顏色十分的淡?

那就針對他改顏色。

```css
.ace_gutter-cell {
    color: #FFFFFF;
    opacity: 0.9;
}
.ace_gutter-cell:hover {
    color: #f0ff00;
    opacity: 0.5;
}
```

## 附上完成的 Theme.css

針對一些顏色與透明度的細節調整就大功告成了 !!!

```css
@-moz-document url-prefix("https://www.overleaf.com/")
{
    .pdf {
        background-color: #151515;
    }
    .pdfjs-viewer {
        filter: invert(0.89);
    }
    .pdf-viewer .pdfjs-viewer canvas {
        box-shadow: 0 0 10px rgba(255,255,255,.5);
    }
    #left-menu {
        filter: invert(0.95);
    }
    #left-menu a {
        filter: invert(0.95);
        color: #138a07;
    }
    #left-menu a:hover {
        background-color: #138a07 !important;
    }
    #left-menu .form-controls label {
        color: #999 !important   
    }
    #left-menu .form-controls {
        color: #999 !important;
    }
    #left-menu .form-controls:hover {
        background-color: #138a07 !important;
        filter: invert(0.95);
    }
    #left-menu .form-controls:hover select:focus {
        filter: invert(1);
        outline-color: #EC75F8;
    }
    #left-menu .form-controls:hover select {
        filter: invert(0.95);
    }
    #left-menu .form-controls select:focus {
        filter: invert(0.05);
        outline-color: #EC75F8;
    }
    #left-menu .form-controls select option {
        background-color: #151515;
    }
    .site-footer * {
        background-color: #222 !important;
        border-top-color: #1A1A1A !important;
    }
    .project-list-table {
        background-color: #222;
    }
    .project-list-main {
        background-color: #151515;
    }
    .card {
        background-color: #222;
    }
    .project-list {
        filter: invert(1);
    }
    .project-list-table {
        filter: invert(1);
    }
    tr.project-list-table-row:hover {
        background-color: #333;
    }
    .content {
        background-color: #151515;
    }
    .loading-screen {
        background-color: #151515;
    }
    .project-list-table-name, .project-list-table-name-link {
        color: #E3F7FC;
    }
    .project-list-table-name-link:hover {
        color: #E3F7FC;
    }
    .project-list-table-name-link:focus {
        color: inherit;
        filter: brightness(0.8);
        outline-color: #E3F7FC;
    }
    .project-list-table-actions-cell div button {
        color: #E3F7FC !important;
    }
    .project-list-table-actions-cell div button:hover {
        filter: brightness(0.8);
    }
    element.style {
        font-size: 12px;
        font-family: "Lucida Console", "Source Code Pro", monospace;
        line-height: 1.6;
    }
    #editor {
        filter: invert(1);
    }
    .ace-editor-wrapper .ace-editor-body {
        height: 100%;
        width: 100%;
    }
    .ace_scroller {
        background: linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url(https://i.imgur.com/6OR1oUa.jpg) no-repeat;
        background-size: cover;
        filter: invert(1);
    }
    .ace_gutter-cell {
        color: #FFFFFF;
        opacity: 0.9;
    }
    .ace_gutter-cell:hover {
        color: #f0ff00;
        opacity: 0.5;
    }
    .ace_layer {
        filter: invert(1);
    }
    .ace_editor {
        position: relative;
        overflow: hidden;
        padding: 0;
        font: 12px/normal 'Monaco', 'Menlo', 'Ubuntu Mono', 'Consolas', 'source-code-pro', monospace;
        direction: ltr;
        text-align: left;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }
}
```