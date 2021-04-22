---
title: For自己競程的解題紀錄
date: 2020-12-01 00:27:00
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
categories: 
- 競程紀錄
tags: 
- Failure Function
- KMP
- 競程
- 算法
---  

# 紀錄大學競程隊伍我所負責解題的領域（字串,序列,一些雜題）
隊伍網站: [Link](https://ntnu-import-magic.github.io/)
## Failure Function
參考資料：
* [資訊之芽算法班第13週KMP](https://www.csie.ntu.edu.tw/~sprout/algo2018/)
* [演算法筆記 String Searching](http://web.ntnu.edu.tw/~algo/StringSearching.html#2)
模板題：[Uva-10298](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1239)
這題先做完Failure Function後直接判斷其size-Failure Function最後一個的值的結果是否整除原本的size。
```cpp=
#include<bits/stdc++.h>
using namespace std; 
int F[1000005];
inline void fail(string& str){	
	int j=-1; F[0]=-1;
	for(int i=1;i<str.size();i++){
		while(j>=0 && str[j+1]!=str[i]) j=F[j];
		if(str[j+1]==str[i]) j++;
		F[i]=j;
	}
}
signed main(){
	string str;
	while(cin >> str && str!="."){
		fail(str);
		int det=str.size()%(str.size()-(F[str.size()-1]+1));
		int ans=str.size()/(str.size()-(F[str.size()-1]+1));
		if(det==0) cout << ans <<"\n";
		else cout << "1\n";
	}
	return 0;
 } 
```
## KMP
模板題：[Uva-11475](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=2470)
參考資料：
* [資訊之芽算法班第13週KMP](https://www.csie.ntu.edu.tw/~sprout/algo2018/)

題序：利用KMP去產生既定字串的最小的回文字串。

想法：首先把字串倒過來，將它的Failure Function列出來後，利用KMP的方式去尋找失敗值在哪裡，這樣就可以從失敗值的位置倒著輸出即為所求。
```cpp=
#include<bits/stdc++.h>
//#define int long long
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
using namespace std;
int F[1000005];
vector<int> ans;
inline void failure(string& str){
    F[0]=-1;
    int sz=str.size();
    for(int i=1,j=-1;i<sz;i++){
        while(j>=0 && str[j+1]!=str[i]) j=F[j];
        if(str[j+1]==str[i]) j++;
        F[i]=j;
    }
} 
inline int KMP(string& str, string& rev){
    failure(rev);
    int len=str.size(),j=-1;
    for(int i=0;i<len;i++){
        while(j>=0 && rev[j+1]!=str[i]) j=F[j];
        if(rev[j+1]==str[i]) j++;
    }
    return j;
}
signed main(){
    string str,str1,rev;
    while(cin >> str1 && str[0]!=EOF){
        int l=str1.size();
        str=str1;
        reverse(str1.begin(),str1.end());
        rev=str1;
        int j=KMP(str,rev);
        for(int i=0;i<l;i++) cout << str[i] ;
        for(int i=l-j-2;i>=0;i--) cout << str[i] ;//也可以這樣寫for(++j;j<l;j++) cout << rev[j];
        cout << "\n";
    }
    return 0;
}
```
## strstr應用
題目：[Uva-10679](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1620)
```cpp=
#include<bits/stdc++.h>
using namespace std;
char str[100005],cmp[100005];
signed main()
{
	int n,m;
	cin >> n;
	while(n--)
	{
		cin >> str;
		cin >> m;
		while(m--)
		{
			cin >> cmp;
			char *deter=strstr(str,cmp);
			if(deter==nullptr) cout << "n\n";
			else cout << "y\n";  
		}
	}
} 
```