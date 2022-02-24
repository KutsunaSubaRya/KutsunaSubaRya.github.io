---
title: Facebook Hacker Cup 2021 Qualification Round
date: 2021-08-30 00:00:00
updated: 2021-08-30 00:00:00
categories: 心得
tags: 
- 演算法
- 2021
- 競程
- Implementation
---
## 心得
目前只是資格賽，僅需解開一題即可晉階至 Round 1
比賽時間總共 72 小時，共 5 題
我是在比賽結束前 5 小時才想到要開始來解題，就當作是休閒打，最後寫了兩題水題並提交，分別是 A1 和 B
比賽連結：[這裡](https://www.facebook.com/codingcompetitions/hacker-cup/2021/qualification-round)
## Problem A1: Consistency - Chapter 1
### 題序
這題給定一個 S 字串，$1 \leq |S| \leq 100$ ，接下來會做這件事：
> If her chosen letter is a vowel, then she may replace it with any consonant of her choice. On the other hand, if her chosen letter is a consonant, then she may replace it with any vowel of her choice.

目的如下：
> Connie really only likes nice, consistent strings. She considers a string to be consistent if and only if all of its letters are the same.

每做一次 replace 花費 1 個單位時間

### 思路
先算每個字母出現的數量，再將母音和非母音分成兩個部份，兩區內找出最大值並算最大值字母以外的變換次數。
**變換次數為自己區除了最大值，總數的 2 倍，以及另一區的總和**
最後 2 區取 min

### 原始碼
```cpp=
#include<bits/stdc++.h>
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
map<char,int> mp;
int C1(char id){
    int n_v=0,v=0;
    for(char i='A';i<='Z';i++){
        if(i=='A' || i=='E' || i=='I' || i=='O' || i=='U'){ if(i!=id) v+=(mp[i]*2); }
        else n_v+=mp[i];
    }
    return n_v+v;
}
int C2(char id){
    int n_v=0,v=0;
    for(char i='A';i<='Z';i++){
        if(i!='A' && i!='E' && i!='I' && i!='O' && i!='U'){ if(i!=id) n_v+=(mp[i]*2); }
        else v+=mp[i];
    }
    return n_v+v;
}
int Solve(void){
    int tmp_vm=0,tmp_nvm=0;
    char num='*',n_v='*';
    for(char i='A';i<='Z';i++){
        if(i=='A' || i=='E' || i=='I' || i=='O' || i=='U'){
            if(mp[i]>tmp_vm){
                num=i;
                tmp_vm=mp[i];
            }
        }
        else{
            if(mp[i]>tmp_nvm){
                n_v=i;
                tmp_nvm=mp[i];
            }
        }
    }
    return min(C1(num),C2(n_v));
}
void init(void){
    for(char i='A';i<='Z';i++) mp[i]=0;
}
signed main(){
    int T,fl=0;
    cin >> T;
    while(T--){
        init();
        string str;
        cin >> str;
        int len=str.size();
        REP(i,len) mp[str[i]]++;
        printf("Case #%d: %d\n",++fl,Solve());
    }
    return 0;
}
```

## Problem B: Xs and Os

### 題序
這題給定一個整數 N，$1 \leq N \leq 50$ ，接下來會輸入由 '.'、'O'、'X' 組成的 $N \times N$ 陣列，只有橫排與直排都為 'X' 時才算贏，'.' 表示為還沒填入。
現在你是 'X' 並且是先手，你可以同時在橫排或直排填入多個 'X'，並且想填入最少的 'X' 就獲勝，輸出填入最少 'X' 的數量以及有幾種方法，沒有贏得方法則輸出 Impossible 。

### 思路
把每一行與列的數量都記錄下來，找到最小值後在數數量。
**記得特判最小值為 1 的情況**
OXO
X . X
OXO
這種情況只能算 1 種方法。

### 原始碼
```cpp=
#include<bits/stdc++.h>
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
using namespace std;
char mp[100][100];
int mp_num[2][100],N,fl=0;
void init(void){
    REP(i,100) REP(j,100) mp[i][j]='.';
    REP(i,2) REP(j,100) mp_num[i][j]=0;
}
void Solve(void){
    int lo_min=INF,meth=0;
    REP(i,N){
        int tmp=0,det=0;
        REP(j,N){
            if(mp[i][j]=='O') det=1;
            else if(mp[i][j]=='.') tmp++;
        }
        if(det==1) tmp=INF;
        mp_num[0][i]=tmp;
        lo_min=min(lo_min,tmp);
    }
    REP(j,N){
        int tmp=0,det=0;
        REP(i,N){
            if(mp[i][j]=='O') det=1;
            else if(mp[i][j]=='.') tmp++;
        }
        if(det==1) tmp=INF;
        mp_num[1][j]=tmp;
        lo_min=min(lo_min,tmp);
    }
    if(lo_min==1){
        REP(i,N) if(mp_num[1][i]==mp_num[0][i] && mp_num[0][i]==1 && mp[i][i]=='.') mp_num[0][i]=0;
    }
    REP(i,2) REP(j,N) if(mp_num[i][j]==lo_min) meth++;
    if(lo_min==INF) printf("Case #%d: Impossible\n",++fl);
    else printf("Case #%d: %d %d\n",++fl,lo_min,meth);
}
signed main(){
    int T;
    cin >> T;
    while(T--){
        init();
        cin >> N;
        REP(i,N) REP(j,N) cin >> mp[i][j];
        Solve();
    }
    return 0;
}
```