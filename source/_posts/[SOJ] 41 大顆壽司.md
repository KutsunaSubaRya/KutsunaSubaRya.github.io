---
title: SOJ 41 大顆壽司
date: 2018-08-16 10:12:00
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
categories: 演算法
tags: 
- SOJ
- djijkstra
- 最短路徑
---

題目URL:https://pc2.tfcis.org/dev/index.php/problem/view/41/
來到了清大的營隊，回顧一下最短路徑的裸題~
debug到死才發現我是垃圾~(因為我忘了把adj clear掉)

```cpp=
#pragma GCC optimize("O2")
#include<bits/stdc++.h>
#define int long long int
#define weight first
#define index second
#define IOS ios_base::sync_with_stdio(false)
#define TI cin.tie(NULL)
using namespace std;
using edge=pair<int,int>;
const int INF=2147483647;
int vnum,dist[400005];
int num,M,st,t,a,b,w;
vector<edge> adj[400005];
void dijkstra(int s)
{
    vector<bool> vis(vnum,false);
    fill(dist,dist+vnum+5,INF);
    dist[s]=0;
    priority_queue<edge,vector<edge>,greater<edge>> pq;
    pq.emplace(0,s);
    while(!pq.empty())
    {
        int u=pq.top().index;
        pq.pop();
        if(vis[u]) continue;
        vis[u]=true;
        for(auto v:adj[u])
        {
            if(dist[v.index] > dist[u]+v.weight)
            {
                dist[v.index]=dist[u]+v.weight;
                pq.emplace(dist[v.index],v.index);
            }
        }
    }
    //for(int i=1;i<=vnum;i++)
    // cout << "dist[" << i  << "]" << dist[i] <<endl;
}
signed main()
{
    IOS;TI;
    cin >> num;
    for(int k=0;k<num;k++)
    {
        cin >> vnum >> M >> st >> t;
        for(int i=0;i<M;i++)
        {
            cin >> a >> b >> w;
            adj[a].emplace_back(w,b);//a to b
            adj[b].emplace_back(w,a);//b to a
        }
        dijkstra(st);
        cout << dist[t] << endl;
        for(int i=0;i<=vnum;i++)
            adj[i].clear();
    }
    return 0;
}
```