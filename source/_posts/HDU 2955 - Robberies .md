---
title: HDU 2955 - Robberies 
date: 2021-08-14 19:55:24
categories: 演算法
tags: 
- HDU
- Dynamic Programming (DP)
- 0/1 背包問題
---
題目連結： [這裡](https://vjudge.net/problem/HDU-2955)

## 思路： 
* 如果是用正常 0/1 背包的思維構造，會發現這題的風險 (重量) 是小數，無法以重量做狀態轉移。
* 換個思路，把風險變成 ` 1 - 風險（重量）` ，也就是 **成功率** 。
* 轉移式： `dp[i]=max(dp[j], dp[j-cost[i]]*weight[i])`
	因為已將題意轉為 **成功率** ，所以連續地成功是用乘法。
* 最後取大於原『成功率』的最大 cost 即可。

## AC 原始碼
```cpp=
#include<bits/stdc++.h>
/*-----define-----*/
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
using namespace std;
signed main(){
	jizz;
#ifndef ONLINE_JUDGE
	freopen("output.txt", "w", stdout);
	freopen("input.txt", "r", stdin);
#endif
	int T,n;
	double P;
	cin >> T;
	while(T--){
		int cost[10005],tot_w=0,tmp=0;
		double weight[10005],dp[10005]={0};
		cin >> P >> n;
		dp[0]=1; //不搶就必定成功
		REP(i,n){
			cin >> cost[i] >> weight[i];
			weight[i]=1-weight[i]; //轉成成功的機率
			tot_w+=cost[i];
		}
		REP(i,n) for(int j=tot_w;j>=cost[i];j--) dp[j]=max(dp[j],dp[j-cost[i]]*weight[i]);
		REP(i,tot_w+1)
			if(dp[i]>=1-P) tmp=i; //成功率大於原定的，表示成功
		cout << tmp << "\n";
	}
	return 0;
}
```