---
title: SOJ 43 Lacy 路網
date: 2018-08-16 15:46:00
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
- MST
- 並查集
---

題目URL:https://pc2.tfcis.org/dev/index.php/problem/view/43/

MST的裸題，簡單來說只要把operator的比較換一下，就變最大生成樹了，然後順便善用dsu並且小複習。(清大營隊進修ing)

```cpp=
#pragma GCC osptimize("O2")
#include<bits/stdc++.h>
#define int long long int
#define IOS ios_base::sync_with_stdio(false)
#define TI cin.tie(NULL)
using namespace std;
const int MI=100005;
int num,a,b,w;
struct edge{
    int from,to,weight;
};
bool operator < (edge &a,edge &b)
{
    return a.weight > b.weight;
}
vector<edge> v;
int vnum,vedge;
struct disjointset{
    int f[MI],rank[MI];
    void init(int N)
    {
        for(int i=0;i<=N;i++)
        {
            f[i]=i;
            rank[i]=0;
        } 
    }
    int find(int val)
    {
        if(f[val]==val) return val;
        return f[val]=find(f[val]);
    }
    bool same(int a,int b)
    {
        return find(a)==find(b);
    }
    void Union(int l,int r)
    {
        if(!same(l,r))
        {
            if(rank[l]<rank[r]) swap(l,r);
            f[f[r]]=f[l];
            rank[l]++;
        }
    }
};
int kruskal(void)
{
    sort(v.begin(),v.end());
    struct disjointset dsu;
    dsu.init(vnum);
    int total=0;
    for(int i=0,cou=0; i<vedge && cou<vnum ;i++)
    {
        int x=v[i].from,y=v[i].to;
        //cout << "x=" <<x << "y=" << y <<endl;
        if(!dsu.same(x,y))
        {  
            total+=v[i].weight;
            cou++;
            dsu.Union(x,y);
        }
    }
    //cout <<"total="<<total <<endl;
    return total;
}
main()
{
    IOS;TI;
    cin >> num;
    while(num--)
    {
        cin >> vnum >> vedge;
        struct edge cpy;
        for(int i=0;i<vedge;i++)
        {
            cin >> a >> b >> w;
            cpy.from=a;
            cpy.to=b;
            cpy.weight=w;
            v.push_back(cpy);
        }
    int ans=kruskal();
    cout << ans << endl;
    while(!v.empty())
        v.pop_back();
    }
    return 0;
}
```