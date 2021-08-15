---
title: 自定義 Stylish Plugin
date: 2021-08-15 20:10:24
categories: Plugin Template
tags:
- CSS
- Stylus
---
參考自： [這裡](https://userstyles.org/styles/169533/g36)
並使用 Chrome 的 Plugin [Stylus](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=zh-TW)
## 原始碼
```css=

body{
  background: #F1F4ED url(https://cdn.jsdelivr.net/gh/KutsunaSubaRya/photo@main/page.webp) no-repeat fixed top right !important; 
    background-attachment: fixed !important;
    background-position: center !important;
	background-size:cover !important;
    color: #e6ff02 ;
}
div.ifM9O{
    background-color: #2869c5 !important;
}

.appbar{
    background:transparent !important
}

div.rl_feature, .rlc__slider, .rlc__slider-page{
    background: transparent;
}
.rl_center{
    border: none
}

.kp-blk{
    border-radius:0;
    border: none
}
.bkWMgd .g a:hover h3 {
 text-decoration:none
}
.B6fmyf {
    visibility: visible;
    top: 8px
}
.NJjxre {
    display: none;
}

.aajZCb,.minidiv .aajZCb{
    box-shadow: 0 5px 10px rgba(0,0,0,0.05);
    border-radius: 0
}


.GHDvEf.ab_button,.GHDvEf.ab_button:hover{
    background: transparent
}

.Wnoohf {
	background: rgba(255,255,255,0.5) ;
    box-shadow: 0 5px 10px rgba(0,0,0,0.05);
    transition: all 0.5s;
}
.mnr-c  :hover {
    background: rgba(255,255,255,1) ;
    box-shadow: none;
        transition: all 1s;
}
.kno-ftr :hover{
    background: transparent
}
.rhsvw .kno-ftr:hover{
       background:rgba(255,255,255,0);
}
#main .mw #rhs {
    margin-left: 1020px;
}
.kp-header:first-child .kno-liu .img-kc-m,.kp-blk .kp-header:first-child .img-brk {
    border-top-right-radius: 0px !important;
    border-top-left-radius: 0px !important;
}

.srg .g:last-of-type {
    margin-bottom: 20px;
}
.vk_c, .vk_cxp, .vk_ic{
       background-color: rgba(255,255,255,0.3);
    border: none;
    border-radius: 0 !important;
        box-shadow: 0 5px 10px rgba(0,0,0,0.05);
    width:104%;
    margin-left: -17px;
    margin-bottom:20px !important;
       transition: all 0.5s
}
.vk_c:hover, .vk_cxp:hover, .vk_ic:hover{
       background-color: rgba(255,255,255,1);
    box-shadow: none
}
.lu-fs{
       width:100%;
       height:120%
}
.yyjhs {
    background: rgba(255, 255, 255, .8);
    padding: 3px 16px;
}

#myuser .myuserconfig{
    background:transparent !important;
    color:grey  !important;
    box-shadow:none  !important;
    border:0 !important;
    margin-right:10px !important
}
#myuser .myuserconfig:hover{
	background:transparent !important;
    color:black  !important;
	border:0
}

.minidiv .sfbg{
background: rgba(255,255,255,0.8) !important;
    box-shadow: 0 5px 10px rgba(0,0,0,0.05);
}
#hdtbSum{
background: transparent !important;
}
#hdtb {
    border: none;
}

.RNNXgb, .minidiv .RNNXgb{
    border-color: rgba(0,0,0,0.1);
    border-style: solid;
    border-width: 0px 0px 1px;
    border-radius: 0;
    background: transparent ;
    transition: all 0.5s
}
.emcav .RNNXgb{
    box-shadow:none;
    background: #fff !important;
    padding:0 0 0 2px
}
.sbfc{
    background: #fff !important;
    box-shadow: none;
    border-color: rgba(0,0,0,0.1)!important;
}
.RNNXgb:hover, .minidiv .RNNXgb:hover{
    background: #fff !important;
    box-shadow: none !important;
    border-color: rgba(0,0,0,0);
    border-style: solid;
    border-width: 0px 0px 1px;
    border-radius: 0;
}

.aR1mEe{
       border-bottom: none !important;
}

g-section-with-header {
    margin: 0;
    padding: 10px 5px;
}

div.res_top_banner #foot,
#page .fk,
#head .headBlock,
#rs_top_new,
#content_right,
.srg>table,
.srg>div[id*="30"],
.srg .c-recommend,
.srg .leftBlock,
.srg .hit_top_new,
.srg #fld,
.srg div.rrecom-btn-parent,
#content_right,
#demo {
 display:none!important
}
body[google] {
 background-color:#fdfdfd
}
#form .bdsug {
 width:76%
}
#ala_img_results {
 overflow:hidden
}
a,
a em {
   color: #e6ff02 !important;
 text-decoration:none
}
#head,
#s_tab {
 background-color:#f8f8f8
     
}
#head {
    color: #e6ff02 !important;
 border-bottom:0
}
#form {
    color: #e6ff02 !important;
 background-color:unset
}
#form .bdsug li {
 width:auto;
 color:#000;
 font:15px arial;
 line-height:26px
}
#form .s_ipt_wr.bg {
 background:#fff;
 width:76%
}
#form .s_btn {
 background:#2866bd;
 border-bottom:1px solid #4879bd
}
#form .s_btn:hover {
 background:#4879bd;
 border-bottom:1px solid #2866bd
}
#s_tab b {
 color:#2866bd;
 border-bottom:3px #2866bd solid
}
#s_tab {
 border-bottom:#e0e0e0 1px solid
}
#u a {
 text-decoration:none
}
#container .head_nums_cont_outer .search_tool_conter,
#container .head_nums_cont_outer .nums {
 width:630px
}
#search .srg,
.med>#ires .bkWMgd>.srg {
 animation-name:left_logoR;
 -webkit-animation-duration:.1s;
 -webkit-animation-timing-function:ease
}
#rs,
.bkWMgd .g {
 width:760px;
 padding:0 20px 15px;
 margin-top:0;
 margin-bottom:40px;
 background-color:rgba(255,255,255,0.3);
 transition: all 0.5s;
 box-sizing:border-box;
 box-shadow: 0 5px 10px rgba(0,0,0,0.05);
 border-radius: 0
}
#rs,
.bkWMgd .g div.rc .s {
 max-width:unset
}
.bkWMgd .g:hover {
 box-shadow:none;
 background:rgb(255, 255, 255)!important
}
.bkWMgd .g[tpl=soft] .op-soft-title,
.bkWMgd .g div.r {
 background-color:rgba(248, 248, 248, .6);
 margin:0 -20px 10px;
 padding:8px 20px 5px;
    border-radius: 0
}
.srg .f13 a,
.srg .f13 em,
.srg .c-span18 a,
.srg .subLink_factory a,
.srg .c-tabs-content a,
.srg .op_offical_weibo_content a,
.srg .op_offical_weibo_pz a,
.srg .op_tieba2_tablinks_container a,
.srg .op-tieba-general-right,
.srg .op_dq01_title,
.srg .op_dq01_table a,
.srg .op_dq01_morelink a,
.srg .op-tieba-general-mainpl a,
.srg .op-se-listen-recommend,
.srg .c-offset>div a {
 text-decoration:none;
 color:#2866bd
}
.srg .f13 a:hover,
.srg .f13 em:hover,
.srg .subLink_factory a:hover,
.srg .c-tabs-content a:hover,
.srg .op_tieba2_tablinks_container a:hover,
.srg .op-tieba-general-right:hover,
.srg .op_dq01_title:hover,
.srg .op_dq01_table a:hover,
.srg .op_dq01_morelink a:hover,
.srg .op-tieba-general-mainpl a:hover,
.srg .op-se-listen-recommend:hover,
.srg .c-offset>div a:hover {
 text-decoration:underline!important
}
.srg .f13 a {
 color:green
}
.srg .c-span18,
.srg .c-span24 {
 width:100%;
 min-width:unset
}
.srg .c-border {
 width:auto;
 border:0;
 border-bottom-color:transparent;
 border-right-color:transparent;
 box-shadow:0 0 0 transparent;
}
.srg .se_com_irregular_gallery ul li,
.srg .op_jingyan_list,
.bkWMgd .g .op-img-address-link-type {
 display:inline-block;
 margin-left:10px
}
.bkWMgd .g[tpl=soft] .op-soft-title a,
.bkWMgd .g[tpl=soft] .op-soft-title a em,
.bkWMgd .g div.r>a,
.bkWMgd .g a h3,
.bkWMgd .g div.r>a em {
 color:#2866bd;
 font-weight:700
}
.srg .op-soft-title a:visited,
.srg .op-soft-title a:visited em,
.bkWMgd .g div.r>a:visited,
.bkWMgd .g div.r>a:visited em,
.bkWMgd .g a:visited h3 {
 color:#609
}
.srg .op-soft-title a,
.bkWMgd .g div.r>a {
 position:relative
}
.srg .op-soft-title a em,
.bkWMgd .g div.r>a em {
 text-decoration:none
}
.srg .op-soft-title a:hover:after,
.bkWMgd .g div.r>a:hover:after {
 left:0;
 width:100%;
 -webkit-transition:width 350ms;
 -moz-transition:width 350ms;
 transition:width 350ms
}
.srg .op-soft-title a:after,
.bkWMgd .g div.r>a:after {
 content:"";
 position:absolute;
 border-bottom:2px solid #2866bd;
 bottom:-2px;
 left:100%;
 width:0;
 -webkit-transition:width 350ms,left 350ms;
 -moz-transition:width 350ms,left 350ms;
 transition:width 350ms,left 350ms
}
.srg .op-soft-title a .bkWMgd .g div.r>a {
 position:relative
}
.srg .op-soft-title a:visited:hover:after,
.bkWMgd .g div.r>a:visited:hover:after {
 left:0;
 width:100%;
 -webkit-transition:width 350ms;
 -moz-transition:width 350ms;
 transition:width 350ms
}
.srg .op-soft-title a:visited:after,
.bkWMgd .g div.r>a:visited:after {
 content:"";
 position:absolute;
 border-bottom:2px solid #609;
 bottom:-2px;
 left:100%;
 width:0;
 -webkit-transition:width 350ms,left 350ms;
 -moz-transition:width 350ms,left 350ms;
 transition:width 350ms,left 350ms
}
#rs {
 margin-top:0;
 padding:0 20px 15px;
}
#rs .tt {
 margin:0 -20px 5px;
 padding:5px 20px;
 background-color:#f8f8f8;
}
#rs table {
 width:630px;
 padding:5px 15px
}
#rs table tr a {
 margin-top:5px;
 margin-bottom:5px;
 color:#2866bd
}
#rs table tr a:hover {
 text-decoration:underline
}
#page {
 min-width:710px;
 height:40px;
 line-height:40px;
 padding-top:5px;
 margin:0 0 50px 80px
}
.op-img-address-desktop-cont {
 overflow:hidden
}
#page a,
#page strong {
 color:#2866bd;
 height:auto;
 box-shadow:none
}
#page .n:hover,
#page a:hover .pc {
 border:1px solid rgba(237, 255, 0, 0);
 background:#d8d8d8;
 color:#0057da
}
#page strong .pc {
 background:#4879bd;
 color:#fff
}
@-webkit-keyframes left_logoR {
 0% {
  -webkit-transform:translateY(64px);
  opacity:0
 }
 50% {
  opacity:0
 }
 100% {
  opacity:1
 }
}
.bkWMgd>div:not([class]) {
 width:730px
}
.bkWMgd>div:not([class]) {
 margin-left:18px;
 margin-right:18px
}
.bkWMgd .g .exp-outline {
 display:none
}
#main .mw #rhs {
 margin-left:1020px
}
#res .g .ts {
 max-width:unset
}
@media screen and (max-width:1400px) {
 .mw #rhs {
  display:none
 }
}
cite {
    color: #e6ff02 ;
 font-weight:400
}
#res .r {
 line-height:1.3
}
#res {
 padding:0
}
#rs,
.bkWMgd .g {
 margin-bottom:20px;
}
.c2xzTb .g,
.ruTcId .g,
.fm06If .g,
.cUnQKe .g,
.HanQmf .g {
 width:758px;
 padding-left:20px!important;
 padding-right:20px!important;
 box-shadow:0 0 0 0 rgba(0,0,0)
}
div .xfxx5d {
 margin-bottom:-18px!important;
 margin-top:-25px!important
}
div .xaqJzf.xfxx5d .kno-ftr {
 margin-top:10px!important
}
div .kno-ftr a {
 position:sticky
}
div .LHJvCe {
    color: #e6ff02 !important;
}
.yDYNvb.lyLwlc {
    color: #e6ff02 !important;
}
.yXK7lf em {
    color: #e6ff02 !important;
}
.dyjrff {
    color: #e6ff02 !important;
}
.O3JH7.qLBFXd {
    color: #e6ff02 !important;
}
.dvDNH {
    color: #e6ff02 !important;
}
.gyWzne {
    color: #e6ff02 !important;
}
.WZ8Tjf {
    color: rgb(91, 247, 135);
}
.k8XOCe {
    background-color: rgba(236, 255, 0, .5);
}
.GmE3X {
    color: #e6ff02 !important;
}
.MXl0lf {
    background: #52ef7a;
    border: 1px solid #263a62;
}
.GHDvEf, .GHDvEf:hover, .GHDvEf.selected, .GHDvEf.selected:hover {
    background-color: #fe8a12
}
.I6TXqe {
    background: rgb(192 243 3 / 35%);
}
.uo4vr {
    color: #e6ff02 !important;
}
.sWISj {
    color: #e6ff02 !important;
}
.S1FAPd {
    color: #e6ff02 !important;
}
.O3JH7 {
    color: #e6ff02 !important;
}
.YyVfkd {
    color: rgb(0 250 227);
}
.AaVjTc a:link {
    color: #e6ff02 !important;
}
.sfbg {
    background: rgb(0 255 255 / 50%);
}
.gLFyf {
    color: rgb(3 22 251);
}
.yg51vc {
    background: rgb(0 255 255 / 50%);
}
.hdtb-mitem.hdtb-msel {
    color: #4b00ff;
}
.hdtb-mitem a {
    color: #e4f304;
}
.hdtb-mitem .GOE98c, .hdtb-mitem a, .hdtb-mitem.hdtb-msel, .t2vtad {
    color: #e4f304;
}
.iv236 {
    color: #1c00ff;
}
.Lj9fsd {
    background: rgb(0 255 255 / 50%);
}
.s8GCU {
    background: rgb(0 255 255 / 50%);
}
.usJj9c .st {
    color: #e6ff02 !important;
}
.TXwUJf {
    color: #e6ff02 !important;
}
a.fl:link, .fl a, .gl a:link {
    color: #e6ff02 !important;
}
.f6F9Be {
    background: #e10b0b;
}
#Wprf1b {
    color: #e6ff02 !important;
}
.unknown_loc {
    background: #007efc;
}
g-inner-card {
    background-color: #140f5d;
}
.UlIbl {
    background-color: #93c2f3;
}
.KFFQ0c .YfftMc, .KFFQ0c .YfftMc span, .KFFQ0c .YfftMc div, .KFFQ0c .YfftMc a {
    color: #e6ff02 !important;
}
.hMJ0yc {
    color: #e6ff02 !important;
}
.FzCfme {
    color: #e6ff02 !important;
}
.FalWJb {
    background-color: #93c2f3;
}
.c93Gbe {
    background: #ff0000;
}
#SIvCob {
    color: #e6ff02 !important;
}
.QCzoEc {
    color: #e6ff02 !important;
}
#tw-target {
    background-color: #317cc7;
}
textarea {
    color: #e6ff02
}
#tw-target-rmn.tw-data-text, #tw-source-rmn.tw-data-text {
    color: #e6ff02
}
.MaH2Hf {
    color: #e6ff02
}
.QXzCSe {
    color: #e6ff02
}
.SvKTZc {
    color: #e6ff02
}
.bHOicb {
    color: #e6ff02
}
.tw-bilingual-pos {
    color: #e6ff02
}
.gXmnc {
    background-color: rgb(1 1 1 / 37%);
}
div#tsuid15.xpdbox.xpdclose{
    background-color: rgb(34 133 255 / 59%);
}
.YrbPuc, .qHx7jd {
    color: #e6ff02
}
.ubHt5c {
    color: #e6ff02
}
.pdpvld {
    color: #e6ff02
}
```