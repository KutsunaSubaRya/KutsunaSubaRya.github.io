---
title: Timus Online Judge 1297.Palindrome
date: 2021-08-18 18:45:24
categories: 演算法
tags: 
- Timus Online Judge
- Manacher Algorithm
- 最長回文字串
- Palindrome
---

## 思路
* Manacher 模板題，要先了解 Manacher 演算法的原理
* 最後不是求長度，但我們可以將回文的 `center` 的位置以及回文長度 `mpl (max_palindrome_len)` 回傳，印出 $center-mpl \leq i \leq center+mpl-1 $  (s[i] $\neq$ '#')

## 參考資源
[[Youtube] Longest Palindromic Substring O(N) Manacher's Algorithm](https://www.youtube.com/watch?v=nbTSfrEfo6M&ab_channel=IDeserve)

## AC 原始碼
```cpp=

#include<bits/stdc++.h>
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
#define MS0(X) memset((X), 0, sizeof((X)))
#define maxEP(x) (max_element((x).begin(), (x).end()))-(x).begin()
#define maxE(x) (*max_element((x).begin(), (x).end()))
using namespace std;
typedef pair<int, int> PII;
typedef vector<int> VI;
VI p(10005);
PII manacher(string s) {
    int len = s.size(), Center = 0, R = 0;
    for(int i = 0; i < len; i++) {
        int mirror = 2 * Center - i;
        if(i + p[mirror] < R) p[i] = min(p[mirror], R - i);
        while(s[i + (1 + p[i])] == s[i - (1 + p[i])]) p[i]++;
        if(i + p[i] > R) {
            Center = i;
            R = i + p[i];
        }
    }
    return make_pair(maxE(p),maxEP(p));
}
inline void init(void) {
    REP(i, 10005) p[i] = 0;
}
signed main() {
    //jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    init();
    string s, new_s;
    cin >> s;
    int flag = 1, len = s.size();
    new_s.insert(0, "@");
    for(int i = 0; i < len; i++) {
        new_s += "#";
        new_s += s[i];
    }
    new_s += "#$";
    PII max_pa=manacher(new_s);
    int pa_len=max_pa.first, pos=max_pa.second;
    REP1(i,pos-pa_len,pos+pa_len) if(new_s[i]!='#') cout << new_s[i];
    cout << "\n";
    return 0;
}
```