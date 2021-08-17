---
title: ACM String 中 STL 的基本應用
date: 2021-08-17 22:27:00
categories: 
- 競程紀錄
tags: 
- Standard Template Library
- 競程
- 模板
--- 

參考文章：[這篇](https://blog.csdn.net/weixin_43093481/article/details/82318377)

## I/O

### 行尾空白
```cpp=
for(int i=0;i<n;i++) cout << v[i] << " \n"[i==n-1];
```
C++ 陣列語法 `a[b]` 等價於 `*(a+b)` 也就是 `" \n"[i==n-1]` 可以看成是 `str=" \n"` `*(str+(i==n-1))`。

### C++ 整行讀取

* cin.getline(name, length, ending character)
```cpp=
cin.getline(str, 100, '\n')
```
* getline(istream, string)
```cpp=
getline(cin,str)
```

## stringstream

### 工能
* 時常用來做字串分割 ( 空白分割 )
```cpp=
stringstream ss;
string s1, s2;
getline(cin, s1);
ss << s1;
while(ss >> s2) cout << s2 << "\n";
```
* int string 之間的轉換

### 清空
```cpp=
stringstream ss1;
...
// 缺一不可
ss1.str("");
ss1.clear();
```

## string STL
### 反轉
```cpp=
reverse(s1.begin(),s1.end());
```
### 反轉並賦值
```cpp=
s2.assign(s1.rbegin(), s1.rend());
```
### 大小寫轉換
* `tranform(,,, :: toupper)` / `tranform(,,, :: tolower)`
```cpp=
#define ALL(X) (X).begin(), (X).end()
transform(ALL(s1), s1.begin(), :: toupper);  // 全部轉大寫
transform(ALL(s1), s1.begin(), :: tolower);  // 全部轉小寫
```
* `toupper` / `tolower`
```cpp=
for(int i=0;i<s1.size();i++) s1[i]=toupper(s1[i]) //逐一轉大寫
for(int i=0;i<s1.size();i++) s1[i]=tolower(s1[i]) //逐一轉小寫
```
### back_insert
* `back_inserter()`
* 好處：不需先知道容器的大小

搭配 `transform()` 的用法
```cpp=
#define ALL(X) (X).begin(), (X).end()
s1="abcdefghijklmn";
s2="123456789";
transform(ALL(s1), back_inserter(s2), :: toupper);
// s2="123456789ABCDEFGHIJKLMN"
```
搭配 `copy()` 的用法
```cpp=
#define ALL(X) (X).begin(), (X).end()
s1="abcdefghijklmn";
s2="123456789";
copy(ALL(s1),back_inserter(s2));
// s2="123456789abcdefghijklmn";
```

### string to int
* `atoi()`

使用 `c_str` 的原因：[參考文章](https://stackoverflow.com/questions/7416445/what-is-use-of-c-str-function-in-c/7416581)
```cpp=
s1="123456789";
int a=atoi(s1.c_str());
// a=123456790;
```

### int to string
* `to_string()`
```cpp=
int a=123456789;
string s1;
s1=to_string(a+1);
// s1="123456790"
```

### assign
* `.assign()`
* `string::npos` 的定義為 `static const size_t npos = -1;`
```cpp=
string s,str;
str="SubaRya";
s.assign(str); // s="SubaRya"
s.assign(str,1,3);// s="uba"
s.assign(str,4,string::npos);// s="Rya"
s.assign("hello"); // s="hello" 
s.assign("SubaRya",8);// 'S','u','b','a','R','y','a','\0' 給 s
s.assign(5,'a');// s="aaaaa"
```

### 刪除連續重複字元
```cpp=
sort(ALL(s2));
s1=s2="aaabbbbnnnaaaafff";
s1.erase(unique(ALL(s1)),s1.end());
s2.erase(unique(ALL(s2)),s2.end());
// s1="abnaf", s2="abfn"
```

### 刪除特定字元
* `erase()`、`remove()`
```cpp=
s1.erase(std::remove(s1.begin(), s1.end(), 'a'/*欲刪除的字元*/), s1.end());
```

### 字串中尋找
* `.find()`
* 如果下述 `s2` 改成 單一字元也可以
```cpp=
int p=s1.find(s2, pos=0) ;// 從 pos 開始查找 s2 在 s1 中出現的位置回傳給 p
if(p==string::npos) cout << "Not found.\n";
else cout << p << "\n";
```
