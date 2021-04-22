---
title: Sprout OJ RMQ練習
date: 2018-12-26 21:58:00
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
categories: 資料結構
tags: 
- SPOJ
- 線段樹
---
題目來源:https://neoj.sprout.tw/problem/80/
參考: https://slides.com/sylveon/2017wds#/2
```c=
#pragma GCC optimize ("O2")
#include<bits/stdc++.h>
//#define int long long
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define id1(X) (X)*2+1 
#define id2(X) (X)*2+2
#define F first
#define S second
#define max_n 1000000
using namespace std;
int str[max_n+5];
struct node
{
	int val;	
}seg[4*max_n+5];
node pull(const node &x, const node &y)
{
	node tmp;
	tmp.val=min(x.val, y.val);
	return tmp;
}
void build(int l,int r, int id)
{
	//cout << l << " " << r << "\n";
	if(l==r) 
	{
		seg[id].val=str[l];
		return;
	}
	int m=(l+r)/2;
	build(l,m,id1(id));
	build(m+1,r,id2(id));
	seg[id]=pull(seg[id1(id)],seg[id2(id)]);
}
node query(int l, int r, int L, int R, int id)
{
	//cout << l << " " << r << " " << L << " " << R << " " << id <<"\n";
	if(l==L && r==R) return seg[id];
	int M=(L+R)/2;
	if(r<=M) return query(l,r,L,M,id1(id));
	if(M<l) return query(l,r,M+1,R,id2(id));
	return pull(
		query(l,M,L,M,id1(id)),
		query(M+1,r,M+1,R,id2(id))
	);
}
void modify(int i, int v, int L, int R, int id)
{
	if(L==R)
	{
		seg[id].val=v;
		return;
	}
	else
	{
		int M=(L+R)/2;
		if(i<=M) modify(i,v,L,M,id1(id));
		else if(i>M) modify(i,v,M+1,R,id2(id));
	}
	seg[id]=pull(seg[id1(id)],seg[id2(id)]);
}
signed main()
{
	jizz;
	int T,N,de,x,y;
	cin >> T >> N;
	for(int i=0;i<N;i++) cin >> str[i];
	build(0,N-1,0);
	while(T--)
	{
		cin >> de >> x >> y;
		if(de&1) cout << query(x,y,0,N-1,0).val << '\n';
		else modify(x,y,0,N-1,0);
	}
	return 0;
}
```