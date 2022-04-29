---
title: 数论模板
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-09 23:02:46
password:
summary:
tags: [模板]
categories: "模板"
---

## 欧拉筛法
<!--more-->
```c
void euler()
{
    phi[1] = 1;
    ll cnt = 0;
    for (ll i = 2; i < maxn; ++i)
    {
        if (!isnotpri[i]) pri[cnt++] = i, minprifactor[i] = i, phi[i] = i - 1;//储存质数，数i的最小质因数，欧拉函数
        for (ll j = 0; j < cnt && i * pri[j] < maxn; ++j)
        {
            minprifactor[i * pri[j]] = pri[j];
            isnotpri[i * pri[j]] = 1;
            if (i % pri[j]) phi[i * pri[j]] = phi[i] * (pri[j] - 1);
            else
            {
                phi[i * pri[j]] = phi[i] * pri[j];
                break;
            }
        }
    }
}
```

## 唯一分解定理
```c
void solve(ll x)
{
    for (ll i = 0; pri[i] * pri[i] <= x; ++i)
    {
        if (!(x % pri[i]))
        {
            ll cnt = 0;
            while (!(x % pri[i])) ++cnt, x /= pri[i];
            factor.push_back({ pri[i],cnt });
        }
    }
    if (x > 0) factor.push_back({ x,1 });
}
```

## 卢卡斯定理
```c
ll comb(ll n, ll r)
{
    if (r > n) return 0;
    return (((frac[n] * getInv(frac[r], p)) % p) * getInv(frac[n - r], p)) % p;
}
ll lucas(ll n, ll r)
{
    if (r == 0) return 1;
    return (comb(n % p, r % p) * lucas(n / p, r / p)) % p;
}
```

## 线性求逆元
```c
inv[0] = -1;
inv[1] = 1;
for (ll i = 2; i <= n; ++i)
{
    inv[i] = ((p - p / i) * inv[p % i]) % p;
}
```