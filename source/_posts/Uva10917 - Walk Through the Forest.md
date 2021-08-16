---
title: Uva10917 - Walk Through the Forest
date: 2021-08-16 20:30:07
categories: 演算法
tags: 
- UVA
- Dijkstra
- 最短路徑
- DFS
- DP
---
**題目連結: [這裡](https://vjudge.net/problem/UVA-10917)**

## 思路

* 注意這句： 
> * He considers taking a path from A to B to be progress if there exists a route from B to his home that is shorter than any possible route from A.
> * ( A , B ) 為了找存在一條從 B 出發，比『所有』從 A 出發回家的路還短，求這種路有幾條。
* 這題要尋找符合題目所敘的所有回到家裡的路徑，如果利用 Dijkstra 進行單源搜索，反過來以家裡為起點做 Dijkstra 會恰好得到我們想要的結果。
* 再來就是利用 DFS 將符合的路徑 ( dist [ $\forall$A ] < dist [ B ] ) 用一個陣列存取，就有一種 DP 的味道了。

## 參考資源
[[CSDN] UVA 10917 Walk Through the Forest 迪杰斯特拉+记忆化搜索](https://blog.csdn.net/ccsu_cat/article/details/80868520)

## AC 原始碼
```cpp=
#pragma GCC optimize ("O2")
#include<bits/stdc++.h>
#pragma region define/typedef
/*-----define-----*/
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define ALL(X) (X).begin(), (X).end()
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
#define SORT_UNIQUE(c) (sort(c.begin(),c.end()), c.resize(distance(c.begin(),unique(c.begin(),c.end()))))
#define GET_POS(c,x) (lower_bound(c.begin(), c.end(),x)-c.begin())
#define MS0(X) memset((X), 0, sizeof((X)))
#define maxE(x) (*max_element((x).begin(), (x).end()))
#define minE(x) (*min_element((x).begin(), (x).end()))
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
#pragma endregion
VPII adj[1005];
int dist[1005], dp[1005];
priority_queue<PII,vector<PII>,greater<PII>> pq;
void init(void) {
    memset(dist, INF, sizeof(dist));
    REP(i, 1005) dp[i] = -1, adj[i].clear();
}
void dijkstra(void) {
    int vis[1005] = {0};
    dist[2] = 0; //從終點做單源最短有利於從其他點求到終點最短路
    pq.emplace(make_pair(dist[2], 2));
    while(!pq.empty()) {
        PII now = pq.top();
        pq.pop();
        //if(dist[now.first]!=now.second) continue;
        if(vis[now.second]) continue;
        vis[now.second] = 1;
        int now_node = now.second;
        for(auto i : adj[now_node]) { //i.first: 點   i.second: 權重
            if(dist[i.first] > dist[now_node] + i.second) {
                dist[i.first] = dist[now_node] + i.second;
                pq.emplace(make_pair(dist[i.first], i.first));
            }
        }
    }
}
int dfs(int idx) {
    int sum = 0;
    if(dp[idx] != -1) return dp[idx];
    if(idx == 2) return 1;
    //  為了找存在一條從 B 出發，比『所有』從 A 出發回家的路還短，求這種路有幾條。
    for(auto i : adj[idx]) if(dist[i.first] < dist[idx]) sum += dfs(i.first);
    return dp[idx] = sum;
}
signed main() {
    jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    int n, m, a, b, w;
    while(cin >> n && n != 0) {
        init();
        cin >> m;
        REP(i, m) {
            cin >> a >> b >> w;
            adj[a].emplace_back(make_pair(b, w));
            adj[b].emplace_back(make_pair(a, w));
        }
        dijkstra();
        cout << dfs(1) << "\n";
    }
    return 0;
}
```