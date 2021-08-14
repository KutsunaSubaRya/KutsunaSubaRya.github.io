---
title: Uva10534 - Wavio Sequence
date: 2021-08-06 12:34:07
categories: 演算法
tags: 
- UVA
- Dynamic Programming (DP)
- 最長遞增子序列 (LIS)
---
Uva10534 - Wavio Sequence
**題目連結: [這裡](https://onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1475)**
## 題目大意 
>Wavio sequence 的定義 :
>* 長度為 $2n+1$ 
>* 前 $n+1$ 個必為嚴格遞增
>* 後 $n+1$ 個必為嚴格遞減
>
>輸入測資不大於 75 筆，每一筆給定長度為 N ( $1\leq N\leq 10000$ ) 的數列，請在數列中找出**最長 Wavio sequence**

## 思路
* $O(n^2)$ 的 LIS 一定會 TLE，因此使用精簡的 Robinson-Schensted-Knuth Algorithm 

**Robinson-Schensted-Knuth Algorithm**
>使用 Greedy 策略，並以二分搜加速，時間複雜度為 $O(NlogL)$
* 前半段是 LIS，後半段是 LDS
* 利用 LIS 從左至右、從右至左各做一次
* 邊建構 LIS 邊將長度記錄下來
* 將 LIS 和 LDS 的長度陣列相同斷點的值取 $max(min(LIS[i] , LDS[i])*2+1)$ 即可

## 參考資源
[臺師大演算法筆記-LIS](https://web.ntnu.edu.tw/~algo/Subsequence.html#3)

## AC 原始碼
```cpp=
#pragma GCC optimize ("O3")
#include<bits/stdc++.h>
/*-----define-----*/
#define jizz ios_base:sync_with_stdio(false),cin.tie(NULL)
#define ALL(X) (X).begin(), (X).end()
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
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
#define lowbit(x) ((x)&(-x))
#define maxE(x) (*max_element((x).begin(), (x).end()))
#define minE(x) (*min_element((x).begin(), (x).end()))
//#define int long long
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef unsigned long long ULL;
typedef long double LD;
typedef pair<int, int> PII;
typedef pair<int, pair<int, int>> PIII;
typedef pair<LL, LL> PLL;
typedef vector<int> VI;  
typedef vector<LL> VLL;
typedef vector<PII> VPII;
typedef vector<PLL> VPLL;
signed main(){
    //jizz;
    //freopen("out.txt", "w", stdout);
    //freopen("in.txt", "r", stdin);
    int n,arr[10005];
    while(cin >> n){
    	vector<int> v,v1;
    	int pos[10005]={0},pos1[10005]={0},fl=0;
    	REP(i,n) cin >> arr[i];
    	v.pb(arr[0]);
    	if(n==1){
    	    cout << 1 << "\n";
    	    continue;
        }
    	REP1(i,1,n){
    		int tmp=arr[i];
    		if(tmp > v.back()) {
    			v.pb(tmp);
				pos[i]=++fl;
	        }
    		else {
    			*lower_bound(ALL(v),tmp)=tmp;
				pos[i]=fl;	
			}
		}
		v1.pb(arr[n-1]); fl=0;
		for(int i=n-2;i>=0;i--){
			int tmp=arr[i];
    		if(tmp > v1.back()){
    			v1.pb(tmp);
				pos1[i]=++fl;	
			} 
    		else{
				*lower_bound(ALL(v1),tmp)=tmp;
				pos1[i]=fl;
			}
		}
		//REP(i,n) cout << pos[i] << " ";CN;
		//REP(i,n) cout << pos1[i] << " ";CN;
		int maxu=1,vs=v.size(),v1s=v1.size();
		REP(i,n) min(pos[i],pos1[i])*2+1 > maxu ? maxu=min(pos[i],pos1[i])*2+1 : maxu=maxu;
		cout << maxu << "\n";
	}
	return 0;
}
```