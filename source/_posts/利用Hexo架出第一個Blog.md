---
title: 利用Hexo架出第一個Blog
date: 2020-10-11 05:07:00
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace: ''
  wrap: true
  hljs: true
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: ''
categories: 心得
tags: 
- Blog
- 2020
- Hexo
---


我之前都是用google的Blogger，如今我想架出自由度高的Blog（~~好啦，就是想要很油很宅的Blog啦~~），於是就被我[學長](https://theriseofdavid.github.io/)推坑用Hexo。

[關於Hexo](https://hexo.io/zh-tw/)

## 美中不足之處

我用的themes是Diaspora(~~一個很適合放很多油圖的themes~~)，但是有些小缺陷如下：

1. Markdown的**語法行號**和**粗體字**無法好好的顯示。
2. hitokoto(一言)需要手動去```diaspora.js```自行調整，首先要對javascript的string用法要熟悉。
3. 如果是繁體字使用者，需要去很多地方改成繁體字-> ```themes/diaspora/layout/```

### 解決方法如下

1. * **行號不出現的問題** -> 到 ```source/css/diaspora.css``` 裡把 ```.content .gutter {display:none;}``` 改成 ```.content .gutter {display:table;}```
   * **無法粗體字**->```source/css/diaspora.css```裡把```.content strong {font-weight:500;}```刪掉。
2. * **hitokoto** -> 直接去```source/js/diaspora.js```裡改他的string。


## 有關Diaspora的套用

相信我，沒有什麼比看原作的流程更詳細的。

[Diaspora連結](https://github.com/Fechin/hexo-theme-diaspora)


## 有關架網站的總過程

我大多是看這篇學的 -> [如何搭建個人 Blog 使用 Hexo + Gitpage](https://medium.com/@bebebobohaha/%E4%BD%BF%E7%94%A8-hexo-gitpage-%E6%90%AD%E5%BB%BA%E5%80%8B%E4%BA%BA-blog-5c6ed52f23db)

如果想把 ```favicon``` 跟 ```logo``` 換成自己的話，我個人是用[這個網站](http://www.akuziti.com/)生成自己想要的字體圖檔，然後再去 ```themes/diaspora/source/css/diaspora.css``` 把 ```.image-logo``` 的px改成自己所需的，但理論上 ```favicon``` 64px就行了，logo則是要去css檔裡修改（看你的logo多長啦）。

還有，如果想在每一篇文章都放圖片的話，那就在```_posts```的```md```檔裡的```yaml```部份加上```cover: /img/....jpg```

舉例：
```yml=
---
title: SOJ 43 Lacy 路網
date: 2018-08-16 15:46:00
categories: 演算法
welcome_cover: false
cover: /img/SOJ43.jpg
tags: 
- SOJ
- MST
- 並查集
---
```

如果想要讓首頁圖片跟著分類一起片換的話（也就是當那篇文章是第一個時，以那一篇cover為當前封面）

那就把```themes/_config.yml```裡的```welcome_cover: .../```給註記掉```#welcome_cover: .../```

另外，記得每個```md```的```yaml```的```title```部份，名字絕對不要有```[ ]```符號，不然會出錯。
## 如何將你的 Hexo + Github Pages

我是看這篇的。

[使用 GitHub Pages + Hexo 來架設個人部落格](https://ed521.github.io/2019/07/hexo-install/)

**但切記：我犯了一個重大的錯誤，就是建repo時一定要```<username>.github.io```，我就是因為取錯repo的名字導致我的網站爛掉。**

## 當你要```npm install hexo-deployer-git --save```時吃了Warning

這篇一定可以幫助到你。

[為什麼npminstall的時候會顯示嚴重漏洞](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/682735/)