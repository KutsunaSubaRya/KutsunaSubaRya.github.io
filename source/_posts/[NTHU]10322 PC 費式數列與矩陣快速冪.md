---
title: NTHU 10322 PC 費式數列與矩陣快速冪
date: 2019-02-21 14:30:00
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
- NTHU
- 矩陣快速冪
- 數論
---  
題目連結:https://acm.cs.nthu.edu.tw/problem/10322/

一題矩陣快速冪裸題(題目就表明(?)

當然是好好地把矩陣的乘法定義定好，注意一些0/1擺放的細節，把其套上快速冪的模板，就大功告成了。><
```cpp=
#pragma GCC optimize("O2")
#include<bits/stdc++.h>
#define int long long int
#define jizz ios_base::sync_with_stdio(false) , cin.tie(NULL) , cout.tie(NULL);
#define pb push_back
#define po pop_back;
#define F first
#define S second
#define CN cout<<"\n"
#define m 100000007
using namespace std;
typedef array<array<int,2>,2> Matrix;
Matrix operator*(Matrix A , Matrix B)
{
    Matrix C;
    for(int i=0;i<2;i++)
    {
        for(int j=0;j<2;j++)
        {
            C[i][j]=0;
            for(int k=0;k<2;k++)
                C[i][j]=(C[i][j]%m+(A[i][k]%m*B[k][j]%m)%m)%m;
        }
    }
    return C;
}
Matrix power(Matrix A,int n)
{
    Matrix ans={{{1,0},{0,1}}};
    while(n)
    {
        if(n&1)
            ans=ans*A;
        n>>=1;
        A=A*A;
    }
    return ans;
}
signed main()
{
    jizz;
    int num;
    while(cin >> num || num==0)
    { 
        if(num==0)
        {
            cout << 0 <<"\n";
            continue;
        }
        else if(num==-1)
            break;
        else
        {
            Matrix A={{{1,1},{1,0}}};
            Matrix C=power(A,num-1);
            cout << C[0][0] <<"\n";
        }
    }
    return 0;
}
```