---
title: 競程用模板
date: 2021-04-11 20:25:00
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
categories: Template
tags: 
- 競程
- 算法
- C++
---  

# 競程用模板 (持續更新中)
```cpp=
#pragma GCC optimize ("O3")
#include<bits/stdc++.h>
/*-----define-----*/
#define jizz ios_base:sync_with_stdio(false),cin.tie(NULL)
#define ALL(X) (X).begin(), (X).end()
#define REP(I,N) for(int I=0;I<(N);I++)
#define SORT_UNIQUE(c) (sort(c.begin(),c.end()), c.resize(distance(c.begin(),unique(c.begin(),c.end()))))
#define GET_POS(c,x) (lower_bound(c.begin(), c.end(),x)-c.begin())
#define pb emplace_back
#define MS0(X) memset((X), 0, sizeof((X)))
#define LEN(X) strlen((X))
#define F first
#define S second
#define LB lower_bound
#define UP upper_bound
#define CN cout<<"\n"
#define maxE(x) (*max_element((x).begin(), (x).end()))
#define minE(x) (*min_element((x).begin(), (x).end()))
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef unsigned long long ULL;
typedef long double LD;
typedef pair<int, int> PII;
typedef pair<LL, LL> PLL;
typedef vector<int> VI;
typedef vector<LL> VLL;
typedef vector<PII> VPII;
typedef vector<PLL> VPLL;

signed main(){
	//jizz;
	return 0;
}
```