---
title: 扁平化設計理論篇
date: 2022-08-25 10:25:00
updated: 2022-09-19 22:05:00
categories: Writeup
tags: 
- 2023
- 有趣的東西
- Design
---

<style>
    .floatRight{
        float: right;
    }
</style>

# 扁平化設計理論篇

## 研究動機

## 扁平化
又稱**瑞士風格(Swiss Style)**
有瑞士風格的字體：[Grotesque](https://en.wikipedia.org/wiki/Vox-ATypI_classification#Grotesque) 無襯線字體
### Helvetica Font
這是 [Grotesque](https://en.wikipedia.org/wiki/Vox-ATypI_classification#Grotesque) 裡最常被使用的
<div> 
    <img class="floatRight" width=200px height=200px src="https://i.imgur.com/ix27A4W.png">
    <p class="alignLeft"> 
        ● 平面設計推薦字體
    </p> 
    <p class="alignLeft"> 
        ● 無襯線字體
    </p>
    <p class="alignLeft"> 
        ● 旁系 -> Microsoft Arial Font
    </p>
    <p class="alignLeft"> 
        ● one of Mac OS 11 fonts
    </p>
    <p class="alignLeft"> 
        ● Helvetica Neue Ultra Light 有銳利清晰與時尚感
    </p>
</div>
</br>

### 中文字體

中文線條錯綜複雜，扁平設計會顯得突兀
目前尚未研發出中文字的扁平設計字體，但中文字本身就是如「一幅畫」的存在，因此著重「空間布局」、「黑白平衡」即可。

### 扁平化特徵
* 移除 3D 要素
    1. 陰影
    2. 漸層
    3. 裝飾
    4. 紋理
    5. 反射光澤

    等...

* 專注於
    1. 圖像元素
    2. 排版
    3. 純色
* 畫面更為流線型

### 扁平與擬物
* 扁平設計 1.0 :
    * 著名指標(Microsoft Zune)
    * 色塊單純
    * 容易延伸拓展區域
    * 簡單的排版(simple typography)
    * 極簡主義
    * 平面物件
    * 載入速度快
* 擬物設計 (Skeuomorphism):
    * 著名指標(MacOS Big Sur)
    * 漸層、陰影等延伸畫面時容易扭曲
    * real-world 物件
    * 載入速度相對慢
* 扁平設計 2.0 :
    * 類名: Material Design
    * 在扁平上加入「少許」3D 元素
    * ![](https://i.imgur.com/z66QOTq.gif)
    * 加上柔順的陰影
        * ![](https://i.imgur.com/7WKdAJG.jpg)
    * 加上模糊(圖中前面的聖誕樹)
        * ![](https://i.imgur.com/2sFW3Aa.jpg)


## 扁平化需注意
因為扁平化對於「數位互動經驗」較少的使用者而言相對不直觀，因此要有以下方法去作為「潛意識引導」。
讓使用者容易發現...
* 按鈕
    * 利用圖標表示「這是可以點」的
* 連結
* 可輸入框

### CTA
* Call To Action (行動呼籲設計)
* 呈現方式: 圖、按鈕、文字... 
* 簡述: 提示 user 點擊進行下一步流程
* 有**正向價值**的按鈕都是 CTA
* 頁面中存在多個按鈕時，CTA 優先級最高

#### 根據 B.J. Fogg 的「**福格行為模型**」

$B=MAT$

* B 行為 (Behavior)
* M 動機 (Motivation)
* A 能力 (Ability)
* T 觸發 (Trigger)
* 其中 MA 成反比
    * 想買，一定會去找按鈕訂購
    * 動機普通，但容易看到訂購鍵

因此，按鈕的位置十分重要，也對應 2 種典型的方法論，「古騰堡原則」、「菲茨定律」

1. 古騰堡原則
    * ![](https://i.imgur.com/irn2QLl.jpg)
    * 左上角: user 首先注意到的地方
    * 右下角: 視覺流的終點
    * 右上角: 「較」少被注意
    * 左下角: 「最」少被注意
    * 從左上到右到左下的 Z 字型動線掃視
2. 菲茨定律
    * 任一點移動到目標所需時間與能力，與點到目標的距離和大小有關
    * $T=a+b \cdot log{_2}(\dfrac{D}{W}+1)$
    * T: 移動時長
    * a、b: 經驗常數
    * D: 起始與終點距離
    * W: 目標寬度大小

#### 值得注意的是
* 古騰堡原則
    * 右上通常為「普通功能操作」或「敏感性操作」
        * 敏感性操作
            * ![](https://i.imgur.com/GMP1EXb.jpg)
    * 底部按鈕分析
        * 水平並排，CTA 高放右邊(e.g. `取消` `確認`)
            * 好的反面應用: 避免潛意識或專注其他適的情況下點錯「重要或敏感的行為」，將該消極的行為放置左側。利用「視覺回流」去加以確認該行動
        * 垂直多按鈕，`確認`放上面，`取消`放下面
            * 因眼睛路徑而論，上到下瀏覽效率最高，所以利用「視覺回流」與「背景色顯著」可達到防錯。
            * ![](https://i.imgur.com/s0XOrwz.jpg)

* 菲茨定律
    * D 小、W 大、按鈕放邊或角(因畫面游標會卡住因此用戶所需能力小)

#### 好的 CTA 按鈕概念
1. 按鈕看起來是潛意識可被點擊的
    * 邊角圓滑
    * 陰影 (Flat Design 1.0 不大適用)
    * 顏色突出
    * 大小合適
        * Apple: CTA button at least 44X44 pixel
        * MicroShop: CTA button at least 34X26 pixel
        * 詳細可以[參考這裡](https://explore.reallygoodemails.com/everything-you-wanted-to-know-about-email-cta-buttons-98807ab98806)
2. 用對比色
3. 留白
    * 可以為 CTA 高的創造點擊率(CTR)
    * 留白區域越少，表示按鈕間連結相近
    * 用簡短的話「鼓勵他」或「引導他」點進，e.g. ...`開始使用'
4. 第一人稱
    * `點擊獲取你的...` :x:
    * `點擊獲取我的...` :heavy_check_mark: (提高 90% 點擊率)
5. 動詞設計


## Reference
* [字型大關係！設計師必收藏的兩套英文字型](https://blog.missmetis.me/avantgarde-helvetica-font-download/)
* [Helvetica Wiki](https://zh.wikipedia.org/zh-tw/Helvetica)
* [50 best](https://speckyboy.com/flat-web-design/)
* [Webdesign Inspiration](https://www.webdesign-inspiration.com/web-designs/style/flat)
* [Flat vs. Material vs. Skeuomorphic Design Examples](https://xd.adobe.com/ideas/principles/web-design/flat-vs-material-skeuomorphic-examples/)
* [Flat 2.0: Why Your Website Needs This Design Update](https://www.bluecompass.com/blog/flat-20-why-your-website-needs-this-design-update)
* [網路行銷按鈕怎麼設計，顧客最想點？高手提高轉換率的 5 個心法](https://www.managertoday.com.tw/articles/view/59934?)
* [UI UX設計是什麼？用9個提高網站轉換率的方法告訴你](https://transbiz.com.tw/ui-ux-design-difference/)
* [怎麼讓使用者按下去？8 個技巧設計高轉換率 CTA 按鈕](https://hahow.in/contents/articles/6064373b26c9ad0ab0be1ecc)
* [關於CTA按鈕的全方位解讀——位置篇](https://twgreatdaily.com/nMrbZncBDlXMa8eqcykt.html)
    * [古腾堡原则](https://www.uisdc.com/gutenberg-principle-2)
    * [菲茨定律](https://zhuanlan.zhihu.com/p/25530956)
* [扁平化設計與字型學（iOS 7特別更新版）](https://blog.justfont.com/2013/05/flat-design-and-typography/)
* [扁平化設計配色](https://designmodo.com/flat/)

