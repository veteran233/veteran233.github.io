---
title: 牛客 - 区间的连续段
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-22 10:38:11
password:
summary:
tags: [倍增]
categories:
---

## 题目
<!--more-->
https://ac.nowcoder.com/acm/problem/15429

## 思路
倍增模板题

## AC代码
```c
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef long double ld; typedef pair<ll, ll> pll;

const ll maxn = 1e6 + 5;
ll n, m, k, a[maxn], f[maxn][25];

int main()
{
    cin >> n >> m >> k;
    for (ll i = 1; i <= n; ++i) cin >> a[i];
    for (ll i = 1; i <= n; ++i) a[i] += a[i - 1];
    for (ll i = 0; i < 25; ++i) f[n + 1][i] = n + 1;
    for (ll i = n; i > 0; --i)
    {
        ll p = upper_bound(a + i, a + 1 + n, a[i - 1] + k) - a;
        f[i][0] = p;
        for (ll j = 1; j < 25; ++j) f[i][j] = f[f[i][j - 1]][j - 1];
    }
    while (m--)
    {
        ll l, r, ans = 0;
        cin >> l >> r;
        for (ll i = 24; i >= 0; --i)
            if (f[l][i] <= r) l = f[l][i], ans += 1 << i;
        if (f[l][0] > r) cout << ans + 1 << endl;
        else cout << "Chtholly" << endl;
    }
    return 0;
}
```