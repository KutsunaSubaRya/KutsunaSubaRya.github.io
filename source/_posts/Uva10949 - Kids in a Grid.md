---
title: Uva10949 - Kids in a Grid
date: 2021-08-06 14:30:24
categories: 演算法
tags: 
- UVA
- Dynamic Programming (DP)
- 最長遞增子序列 (LIS)
- 最長共同子序列 (LCS)
---
**題目連結: [這裡](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=1890)**
## 題目大意 
>* 輸入大小為 $H \times W$的二維字串 $(1\leq H,W\leq 20)$
>* 有兩個人利用東西南北拜訪二維字串(往一個方向走一單位，並保證走法是合法的(不會走出邊界)) 
>* 輸入 $N$ , $X_0$ , $Y_0$ ，接下來從 ($X_0$, $Y_0$) 開始走 $N$ 步 ($1$ $\leq$ $X_0$ $\leq$ $H,$ $1$ $\leq$ $Y_0$ $\leq$ $W,$ $1$ $\leq$ $N$ $\leq$ $20000$)
>* 將兩個人最長共同子序列印出
>
>輸入測資不大於 15 筆

## 思路
* $O(NM)$ 的 LCS 一定會 TLE，因此使用 Hunt-Szymanski Algorithm 把 LCS 轉化成 LIS
* 再將 LIS 用 Robinson-Schensted-Knuth Algorithm 所得 **LIS 長度 $=$ LCS 長度**

**Hunt-Szymanski Algorithm**
>步驟如下:
>* 將 s1 每個字元的位置用 `vec[128]` 存起來
>* 將每個字元所對應的位置由大到小排序
>* 把 s2 所有字元換成 `vec[128]` 的位置，形成儲存所有位置的 `arr` 一維數列 
>* 算出 arr 的 LIS 長度即為原 LCS 長度
>
>舉個例子:
>* s1[]="abcaabbc", s2="abbacadbc"
>* 紀錄 s1 每個字元所出現的位置
>   * a: 0,3,4 -排列後--> 4,3,0
>   * b: 1,5,6 -排列後--> 6,5,1
>   * c: 2,7 -排列後--> 7,2
>   * d: 
>* 將 s2 的字元換成上一步中所對應的數字
>   * "abbacadbc"={4,3,0,6,5,1,6,5,1,4,3,0,7,2,4,3,0,6,5,1,7,2}
>   * {0,1,2,3,5,7}，長度為 6，包含真正的 {0,1,3,4,6,7}
>
>時間複雜度 $O($ 數對位置數目$\times$ $min(N,M)$$)$
> 最差情況 s1="aaa...aa", s2="aaa...aa"，退化成 $O(N^2logN)$，比 $O(N^2)$ 的 LCS 更差，所以要看時機使用


**Robinson-Schensted-Knuth Algorithm**
>使用 Greedy 策略，並以二分搜加速，時間複雜度為 $O(NlogL)$
>* 前半段是 LIS，後半段是 LDS
>* 利用 LIS 從左至右、從右至左各做一次
>* 邊建構 LIS 邊將長度記錄下來
>* 將 LIS 和 LDS 的長度陣列相同斷點的值取 $max(min(LIS[i] , LDS[i])*2+1)$ 即可

## 參考資源
[臺大資訊之芽 - LIS/LCS](https://www.csie.ntu.edu.tw/~sprout/algo2019/ppt_pdf/week07/dynamic_programming_2_1.pdf)

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
char arr[105][105],s[20005],str[2][20005];
int a,b,n;
inline void generate_string(int Step, int x, int y, int idx){
	int fl=1;
	REP(i,Step){
		if(s[i]=='N') str[idx][fl++]=arr[--x][y];
		else if(s[i]=='E') str[idx][fl++]=arr[x][++y];
		else if(s[i]=='S') str[idx][fl++]=arr[++x][y];
		else if(s[i]=='W') str[idx][fl++]=arr[x][--y];
	}
}
inline void init(void){
	memset(arr,0,sizeof(arr));
	memset(s,0,sizeof(s));
	memset(str,0,sizeof(str));
}
bool cmp(const int a, const int b) { return a>b; } 
signed main(){
    //jizz;
	//freopen("out.txt", "w", stdout);
    //freopen("in.txt", "r", stdin);
	cin >> n;
	REP1(k,1,(n+1)){
		int Step,Step1,x,y;
		vector<int> table[128],LIS_table,LIS;
		init();
		cin >> a >> b;
		REP(i,a) REP(j,b) cin >> arr[i][j];
		cin >> Step >> x >> y;
		REP(i,Step) cin >> s[i];
		str[0][0]=arr[x-1][y-1];
		generate_string(Step,x-1,y-1,0);
		cin >> Step1 >> x >> y;
		REP(i,Step1) cin >> s[i];
		str[1][0]=arr[x-1][y-1];
		generate_string(Step1,x-1,y-1,1);
		Step++;Step1++;
		REP(i,Step) table[(int)str[0][i]].pb(i);
		REP(i,128) sort(ALL(table[i]),cmp);
		REP(i,Step1) for(auto j:table[(int)str[1][i]]) LIS_table.pb(j);
		if(LIS_table.empty()){
			printf("Case %d: %d %d\n",k,Step,Step1);
			continue;
		}
		int len=LIS_table.size();
		LIS.pb(LIS_table[0]);
		REP1(i,1,len){
			int tmp=LIS_table[i];
			if(LIS.back()<tmp) LIS.pb(tmp);
			else *lower_bound(ALL(LIS),LIS_table[i])=LIS_table[i];
		}
		int LIS_len=LIS.size();
		printf("Case %d: %d %d\n",k,Step-LIS_len,Step1-LIS_len);
	}
	return 0;
}
```