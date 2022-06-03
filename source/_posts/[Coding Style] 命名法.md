---
title: Coding Style 命名法
date: 2022-04-08 16:50:24
updated: 2022-04-08 16:50:24
categories: Coding Style
tags:
- Coding Style
- 2022
---

參考資料：
[CMAndroidBook - Coding Style](https://cmmobile.gitbook.io/androidbook/xin-ren-xun-lian/code-style#ming-ming)
[2021 iThome 鐵人賽 Day 11. Coding style](https://ithelp.ithome.com.tw/articles/10263403)
[Wiki 駝峰式大小寫](https://zh.wikipedia.org/wiki/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)

## 駝峰式大小寫 (Camel-Case)
* 單字間不以空格、連接號、底線連結
    * 錯誤寫法 -> my variable、my-variable、my_variable  
* 命名形式分為兩種
    1. 小駝峰式命名法（lower camel case）
        * 第一個單字以小寫字母開始，第二個單字的首字母大寫
        * Ex. myVariable
    2. 大駝峰式命名法（upper camel case）
        * 每一個單字的首字母都採用大寫字母
        * Ex. MyVariable
        * 也可以稱為**Pascal命名法（Pascal Case）**

## 整體結論

1. Code 裡除了**註解**和**字串**，其餘不得使用中文字
2. No `region`、`endregion`
3. MagicNumber 請一律宣告成**常數**、**變數**或**enum**
4. 禁用無大括號單行 else -> [Dangling Else](https://en.wikipedia.org/wiki/Dangling_else)
5. Compile 後有 Warning 的程式碼就是有問題，更不用說有 Error
6. 常數命名一律用**全大寫配底限分隔** Ex. MAX_MAP_WIDTH
7. 命名時請勿自行縮寫單字，除非該單字縮寫是通用的
8. Class、方法須遵守 大駝峰式命名 規則
9. 參數、區域變數須遵守 小駝峰式命名 規則
10. 方法的第一個單字必須為動詞(Get、Push、Delete、Update...等等)
11. 單字不能超過3個(太長的專有名詞須先建立縮寫表，統一所有人使用的縮寫)

## 檔案

1. 大駝峰命名 Ex. StringParser.cpp
2. 檔案名與類別需同名
```cpp=
// StringParser.cpp
// 正確
class StringParser{
}
// 錯誤
class OtherClass{
}
```
3. 理論上一個檔案**只有**一個主要類別
```cpp=
// StringParser.cpp
class StringParser{
}
class DoAnything{
}
```

## 類別的命名

1. 大駝峰命名
2. 使用**名詞**或**形容詞 + 名詞**的方式命名
```cpp=
// 正確
class NestedStructure{
}
//錯誤
class GetPosition{
}
```

## 方法的命名

1. 小駝峰命名
2. 使用**動詞**或**動詞 + 名詞**的方式命名
```cpp=
std::pair<int,int>  getPosition{
}
```
3. **方法的參數**或**區域變數**名使用**小駝峰命名**
```cpp=
std::pair<int,int> getPosition(std::string objectName){
}
```
4. 以 Boolean 作為回傳值的命名，需使用 isXXX, canXXX, hasXXX, 或 XXXable 命名。
```cpp=
bool isEmpty(){
}
```

## Package name 的命名

* 全小寫

## Resource 的命名

* 全小寫
* 以畫面使用的位置＋功能 來命名
    * activity_xxx.xml
    * layout_xxx.xml
    * selector_xxx.xml
    * ...

## View id 的命名

* 功能＋使用元件 來命名
    * login_textView
    * reply_us_editText
    * xxx_textView
    * xxx_editText
    * xxx_button
    * xxx_linearLayout(constraintLayout,..)
    * ...
