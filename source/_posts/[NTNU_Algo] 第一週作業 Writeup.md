---
title: NTNU 演算法課程第一週作業 Writeup
date: 2022-03-11 13:56:40
updated: 2022-03-11 13:56:40
categories: Writeup
tags: 
- NTNU
- 2022
- 算法
- BIT
- RMQ
- Greedy
- 快速冪
---

## pA [293] Triangle

題目概述：
最初狀態有一個正三角形，每一個時間單位會使每一個三角形分割成 4 個正三角形(一個朝下、三個朝上)。
詢問時間點 n ，求有幾個**朝上**的三角形。

作法：
1. 找到三角形朝上的規律
2. 快速冪

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
#define getchar getchar_unlocked
#define INF 0x3f3f3f3f
#define NINF 0xc0c0c0c0
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
template<typename __Type_of_scan>
inline void scan(__Type_of_scan &x){
	__Type_of_scan _tmp=1;
    x=0;
    char s=getchar();
	while(s<'0'||s>'9'){
        if(s=='-') _tmp=-1;
        s=getchar();
    }
	while(s>='0'&&s<='9'){
        x=x*10+s-'0';
        s=getchar();
    }
	x*=_tmp;
}
template<typename __Type_of_print>
void print(__Type_of_print x)
{
	if(x<0){putchar('-'); x=-x;}
	if(x>9) print(x/10);
	putchar(x%10+'0');
}
#pragma endregion

const int mod=1e9+7;
LL power(LL A,LL n){
    LL ans=1;
    A=A%mod;
    while(n){
        if(n&1) {
            ans=(ans*A)%mod;
            n--;
        }
        n>>=1;
        A=(A*A)%mod;
    }
    return ans;
}
signed main(){
    jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    LL d,tmp,ans;
    scan(d);
    tmp=power(2,d);
    ans=(((1+tmp)*tmp)>>1)%mod;
    print(ans);
    return 0;
}
```

## pB []

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
signed main(){
    jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    int tot=1,n,a,b,chk[100005];
    PII arr1[100005],arr2[100005];
    cin >> n;
    REP(i,n){
        cin >> a >> b;
        arr1[i]=make_pair(a,b);
    }
    sort(arr1,arr1+n);
    PII tmp=arr1[n-1],tmp1=arr1[n-1];
    int det=0;
    for(int i=n-2;i>=0;i--){
        if(tmp1.first>arr1[i].first && det){
            det=0;
            tmp=tmp1;
        }
        if(tmp.first==arr1[i].first){
            tot++;
        }
        else if(tmp.first>arr1[i].first && tmp.second<=arr1[i].second){
            det=1;
            tmp1=arr1[i];
            tot++;
        }
    }
    cout << tot << "\n";
    return 0;
}
```

## pC []

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
#define Mid(l,r) (l)+(((r)-(l))>>1)
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef pair<int, int> PII;
typedef pair<LL, int> PLI;
typedef vector<int> VI;
typedef vector<PII> VPII;
#pragma endregion
const int mod=1e9+7;
LL arr[200005];
struct Leaf{
    long long val;
    int pos;
};
Leaf Tree[800005];
void up(int idx){
    if(Tree[idx*2+2].val<Tree[idx*2+1].val){
        Tree[idx].val=Tree[idx*2+2].val;
        Tree[idx].pos=Tree[idx*2+2].pos;  
        return;      
    }
    Tree[idx].val=Tree[idx*2+1].val;
    Tree[idx].pos=Tree[idx*2+1].pos;
    return;
}
void buildTree(int l, int r, int idx){
    if(l==r){
        Tree[idx].val=arr[l];
        Tree[idx].pos=l;
        return;
    } 
    int mid=Mid(l,r);
    buildTree(l,mid,idx*2+2);
    buildTree(mid+1,r,idx*2+1);
    up(idx);
    return;
}
Leaf Query(int x, int y, int l, int r, int idx){
    if(x==l && y==r) return Tree[idx];
    int mid=Mid(l,r);
    if(y<=mid) return Query(x,y,l,mid,idx*2+2);
    if(x>mid) return Query(x,y,mid+1,r,idx*2+1);
    Leaf tmp1,tmp2;
    tmp1=Query(x,mid,l,mid,idx*2+2);
    tmp2=Query(mid+1,y,mid+1,r,idx*2+1);
    return tmp1.val<tmp2.val?tmp1:tmp2;
}
void Update(int l, int r, int pos, int idx, LL val){
    if(l==r){
        Tree[idx].val=val;
        Tree[idx].pos=l;
        return;
    }
    int mid=Mid(l,r);
    if(pos<=mid) Update(l,mid,pos,idx*2+2,val);
    else if(pos>mid) Update(mid+1,r,pos,idx*2+1,val);
    up(idx);
}
signed main(){
    jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    int n;
    LL tot=0;
    cin >> n;
    REP1(i,1,n+1) cin >> arr[i];
    buildTree(1,n,0);
    REP1(i,1,n+1){
        Leaf tmp;
        tmp=Query(i,n,1,n,0);
        // cout << "tmp= " << tmp.val << " " << tmp.pos << "\n";
        // cout << "arr[i]= " << arr[i] << " " << i << "\n\n";
        if(tmp.pos!=i && tmp.val<arr[i]){
            cout << "tmp= " << tmp.val << " " << tmp.pos << "\n";
            cout << "arr[i]= " << arr[i] << " " << i << "\n\n";
            tot=(tot+((tmp.val+arr[i])*abs(tmp.pos-i))%mod)%mod;
            Update(1,n,tmp.pos,0,arr[i]);
            arr[tmp.pos]=arr[i];
        }
    }
    cout << tot << "\n";
    return 0;
}
```

## pD []

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
#define lowbit(x) (x&-x)
/*-----typedef-----*/
using namespace std;
typedef long long LL;
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<PII> VPII;
vector<LL> status,lisan;
inline vector<LL> discretization(vector<LL> a){
    status.resize(a.size());
    lisan = a;
    sort(lisan.begin(),lisan.end());
    lisan.resize(unique(lisan.begin(),lisan.end())-lisan.begin());
    for(int i = 0; i < (int)a.size(); i++) 
        status[i] = lower_bound(lisan.begin(),lisan.end(),a[i])-lisan.begin();
    return status;
}
LL Bit[200005],n;
inline void modify(int x, int d){
    for(;x<=n;x+=lowbit(x)) Bit[x] += d;
}
inline LL query(int x){
    LL sum=0;
    for(;x;x-=lowbit(x)) sum += Bit[x];
    return sum;
}
#pragma endregion
signed main(){
    jizz;
#ifndef ONLINE_JUDGE
    freopen("output.txt", "w", stdout);
    freopen("input.txt", "r", stdin);
#endif
    LL tot_elem=0,det=0,tmp;
    vector<LL> arr,v1,v2;
    cin >> n;
    REP(i,n) { cin >> tmp; arr.emplace_back(tmp); }
    arr=discretization(arr);
    REP(i,n){
        arr[i]++;
        if(!(i&1)) v1.emplace_back(arr[i]);
        else v2.emplace_back(arr[i]);
    }
    LL ans=0;
    MS0(Bit);
    for(auto i:v1){
        ans+=tot_elem-query(i);
        modify(i,1);
        tot_elem++;
    }
    MS0(Bit);
    tot_elem=0;
    for(auto i:v2){
        ans+=tot_elem-query(i);
        modify(i,1);
        tot_elem++;
    }
    sort(arr.begin(),arr.end());
    sort(v1.begin(),v1.end());
    sort(v2.begin(),v2.end());
    REP(i,n){
        int pos=i/2;
        if((!(i&1) && arr[i]!=v1[pos]) || (i&1 && arr[i]!=v2[pos])){
            det=1;
            break;
        }
    }
    if(det) cout << "no\n";
    else cout << "yes\n";
    cout << ans << "\n";
    return 0;
}
```