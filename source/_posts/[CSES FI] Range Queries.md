---
title: 區間詢問類題目總集(更新中)
date: 2022-02-22 02:56:24
updated: 2022-02-22 02:56:24
categories: 競程紀錄
tags:
- RMQ
- 線段樹
- 2022
- 前綴和
- ST表
---

## 靜態區間總和 (非帶修改)
參考題目：[Static Range Sum Queries](https://cses.fi/problemset/task/1646)

題目類型：前綴和

## 靜態區間最大值/最小值 (非帶修改)
參考題目：[Static Range Minimum Queries](https://cses.fi/problemset/task/1647)

題目類型：線段樹、ST表

### 單純遞迴並回傳值 (TLE)
該題目以**單純遞迴並回傳值**方向思考會 TLE，仔細觀察會發現有重複區間被詢問到。

```cpp=
// This code will get TLE absolutly
int SRMQ(int l, int r){
    int mid=l+((r-l)>>1);
    if(l+1==r || l==r) return min(arr[l],arr[r]);
    else return min(SRMQ(l,mid),SRMQ(mid+1,r));
}
```

### Sparse Table (稀疏表、ST 表)

參考資料：
* [WIWIHO 的競程筆記 Sparse Table](https://cp.wiwiho.me/sparse-table/)
* [OI Wiki ST 表](https://oi-wiki.org/ds/sparse-table/)

用於透過**預處理**快速查詢非帶修改資料結構最大/小值，一般像是 RMQ 問題。
#### 思路
主要思路：倍增、DP
設 $f(i,j)$ 為區間 $[i,i+2^j-1]$  最大/小值
可先得知 $f(i,0)=$ 區間 $[i,i]$ 最大/小值 $=a_i$

#### 轉移方程
$2^j$ 長度的區間是由 2 個 $2^{j-1}$ 所組成，因此
$$f(i,j)=min/max(f(i,j-1),f(i+2^{j-1},j-1))$$

#### 求區間極值
用 2 個 $2^{\lfloor{log_2{(r-l+1)}}\rfloor}$ 長度的區間求極值即可，也就是 $[l,l+2^s-1]$ 和 $[r-2^s+1,r]$ 區間，而 $s=\lfloor{log_2{(r-l+1)}}\rfloor$

#### 結論
區間重疊無妨，因為兩區間的聯集是欲求之區間即可，這也說明了**同一個元素會被多個區間所包含**，因此 **Sparse Table 不支援帶修改**。

```cpp=
#include<bits/stdc++.h>
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define REP(I,N) for(int I=0;I<(N);I++)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
using namespace std;
const int max_lg=20,max_arrlen=200000;
int Log[max_arrlen+5],F[max_arrlen+5][max_lg+5];
inline void build_logn(void){ // 建立 Log 表
    Log[1]=0; Log[2]=1; 
    for(int i=3;i<max_arrlen;i++) Log[i]=Log[i/2]+1;
    // REP1(i,3,max_arrlen) Log[i]=Log[i/2]+1;
}
inline void build_ST(int n){ // 預處理 ST 表
    REP1(j,1,max_lg+1) 
        for(int i=1;i+(1<<j)-1<=n;i++)
            F[i][j]=min(F[i][j-1],F[i+(1<<(j-1))][j-1]);
}
inline void init(int n){
    build_logn();
    REP1(i,1,n+1) cin >> F[i][0]; // Input
    build_ST(n);
}
signed main(){
    jizz;
    int n,q,a,b,log_j;
    cin >> n >> q;
    init(n);
    REP(i,q){
        cin >> a >> b;
        int log_j=Log[b-a+1];
        cout << min(F[a][log_j], F[b-(1<<log_j)+1][log_j]) << "\n"; // 兩區間聯集的極值
    }
    return 0;
}
```

## 動態區間總和/極值（單點修改）
參考題目：
* [Dynamic Range Sum Queries](https://cses.fi/problemset/result/3580210/)
* [Dynamic Range Minimum Queries](https://cses.fi/problemset/task/1649)

題目類型：線段樹

### Pull
```cpp=
LL up(LL a, LL b){
    return a+b; // 求區間總和
    // return min(a,b); // 求區間最小值
    // return max(a,b); // 求區間最大值
}
```
### 建樹
```cpp=
void buildTree(int l, int r, int idx){
    if(l==r){
        Tree[idx]=arr[l];
        return;
    } 
    int mid=Mid(l,r);
    buildTree(l,mid,idx*2+2);
    buildTree(mid+1,r,idx*2+1);
    Tree[idx]=up(Tree[idx*2+2],Tree[idx*2+1]); // 記得 pull
    return;
}
```
### 詢問
```cpp=
LL Query(int x, int y, int l, int r, int idx){
    if(x==l && y==r) return Tree[idx];
    int mid=Mid(l,r);
    if(y<=mid) return Query(x,y,l,mid,idx*2+2);
    if(x>mid) return Query(x,y,mid+1,r,idx*2+1);
    return up(Query(x,mid,l,mid,idx*2+2),Query(mid+1,y,mid+1,r,idx*2+1)); // 記得 pull
}
```
### 單點修改
```cpp=
void Update(int l, int r, int pos, int idx, LL val){
    if(l==r){
        Tree[idx]=val;
        return;
    }
    int mid=Mid(l,r);
    if(pos<=mid) Update(l,mid,pos,idx*2+2,val);
    else if(pos>mid) Update(mid+1,r,pos,idx*2+1,val);
    Tree[idx]=up(Tree[idx*2+2],Tree[idx*2+1]); // 記得 pull
}
```

### [Dynamic Range Sum Queries] Solution
```cpp=
#include<bits/stdc++.h>
#define jizz ios_base::sync_with_stdio(false),cin.tie(NULL)
#define REP1(I,N,M) for(int I=(N);I<(M);I++)
using namespace std;
typedef long long LL;
#define Mid(l,r) (l)+(((r)-(l))>>1)
LL arr[200005],Tree[800005];
LL up(LL a, LL b){
    return a+b;
}
void buildTree(int l, int r, int idx){
    if(l==r){
        Tree[idx]=arr[l];
        return;
    } 
    int mid=Mid(l,r);
    buildTree(l,mid,idx*2+2);
    buildTree(mid+1,r,idx*2+1);
    Tree[idx]=up(Tree[idx*2+2],Tree[idx*2+1]);
    return;
}
LL Query(int x, int y, int l, int r, int idx){
    if(x==l && y==r) return Tree[idx];
    int mid=Mid(l,r);
    if(y<=mid) return Query(x,y,l,mid,idx*2+2);
    if(x>mid) return Query(x,y,mid+1,r,idx*2+1);
    return up(Query(x,mid,l,mid,idx*2+2),Query(mid+1,y,mid+1,r,idx*2+1));
}
void Update(int l, int r, int pos, int idx, LL val){
    if(l==r){
        Tree[idx]=val;
        return;
    }
    int mid=Mid(l,r);
    if(pos<=mid) Update(l,mid,pos,idx*2+2,val);
    else if(pos>mid) Update(mid+1,r,pos,idx*2+1,val);
    Tree[idx]=up(Tree[idx*2+2],Tree[idx*2+1]);
}
signed main(){
    jizz;
    int n,q,det,k,u,a,b;
    cin >> n >> q;
    REP1(i,1,n+1) cin >> arr[i];
    buildTree(1,n,0);
    while(q--){
        cin >> det;
        if(det==1){
            cin >> k >> u;
            Update(1,n,k,0,(LL)u);
        }
        else{
            cin >> a >> b;
            LL ans=Query(a,b,1,n,0);
            cout << ans << "\n";
        }
    }
    return 0;
}
```
