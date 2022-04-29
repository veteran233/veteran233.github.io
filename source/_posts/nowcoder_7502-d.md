---
title: 2020ICPC·小米 网络选拔赛第二场 - D - Determinant
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-01 16:59:34
password:
summary:
tags: [牛客,数学]
categories:
---

## 题目
<!--more-->
https://ac.nowcoder.com/acm/contest/7502/D

## 思路
回忆一下线性代数的知识，用升阶法得：
$$
\left|\begin{array}{cccc}
x+a_{1} b_{1} & a_{1} b_{2} & \cdots & a_{1} b_{n} \\
a_{2} b_{1} & x+a_{2} b_{2} & \cdots & a_{2} b_{n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{n} b_{1} & a_{n} b_{2} & \cdots & x+a_{n} b_{n}
\end{array}\right|
\rightarrow
\frac{1}{x}\left|\begin{array}{ccccc}
x & b_{1} & b_{2} & \cdots & b_{n} \\
0 & x+a_{1} b_{1} & a_{1} b_{2} & \cdots & a_{1} b_{n} \\
0 & a_{2} b_{1} & x+a_{2} b_{2} & \cdots & a_{2} b_{n} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & a_{n} b_{1} & a_{n} b_{2} & \cdots & x+a_{n} b_{n}
\end{array}\right|
\rightarrow
$$
$$
\frac{1}{x}\left|\begin{array}{ccccc}
x & b_{1} & b_{2} & \cdots & b_{n} \\
-x a_{1} & x & 0 & \cdots & 0 \\
-x a_{2} & 0 & x & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
-x a_{n} & 0 & 0 & \cdots & x
\end{array}\right|
\rightarrow
\frac{1}{x}\begin{array}{|ccccc|}
x+a_{1} b_{1}+a_{2} b_{2}+\cdots+a_{n} b_{n} & b_{1} & b_{2} & \cdots & b_{n} \\
0 & x & 0 & \cdots & 0 \\
0 & 0 & x & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & x
\end{array}
$$
答案为$\left(x+a_{1} b_{1}+a_{2} b_{2}+\cdots+a_{n} b_{n}\right) x^{n-1}$。

## AC代码
```c
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef long double ld; typedef pair<ll, ll> pll;

ll fpow(ll a, ll b, ll mod) { if (b < 0) return -1; ll res = 1; while (b) { if (b & 1) res = (res * a) % mod; a = (a * a) % mod; b >>= 1; }return res; }

const ll maxn = 1e5 + 5, mod = 1e9 + 7;
ll n, x, a[maxn], b[maxn];

int main()
{
    while (cin >> n >> x)
    {
        for (ll i = 0; i < n; ++i) cin >> a[i];
        for (ll i = 0; i < n; ++i) cin >> b[i];

        ll ans = x;
        for (ll i = 0; i < n; ++i)
        {
            ans = (ans + ((a[i] * b[i]) % mod)) % mod;
        }
        ans = (ans * fpow(x, n - 1, mod)) % mod;

        cout << ans << endl;
    }
    return 0;
}
```