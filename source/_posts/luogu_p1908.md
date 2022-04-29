---
title: 洛谷 - 逆序对
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-02 22:06:25
password:
summary:
tags: [洛谷,树状数组,线段树,离散化]
categories:
---

## 题目
<!--more-->
https://www.luogu.com.cn/problem/P1908

## 思路
权值树状数组，权值线段树模板题，注意要离散化。

## AC代码
权值树状数组
```c
#include <bits/stdc++.h>
#define endl '\n'
#define lson(x) x<<1
#define rson(x) (x<<1)|1
#define lowbit(x) x&-x
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef long double ld; typedef pair<ll, ll> pll;

const ll maxn = 5e5 + 5;
ll n, a[maxn], b[maxn], tree[maxn];

void update(ll p)
{
    while (p <= n)
    {
        ++tree[p];
        p += lowbit(p);
    }
}
ll query(ll p)
{
    ll res = 0;
    while (p > 0)
    {
        res += tree[p];
        p -= lowbit(p);
    }
    return res;
}

int main()
{
    ll ans = 0;
    cin >> n;
    for (ll i = 1; i <= n; ++i) cin >> a[i], b[i] = a[i];

    sort(b + 1, b + 1 + n);
    ll len = unique(b + 1, b + 1 + n) - b - 1;
    for (ll i = 1; i <= n; ++i)
    {
        ll t = lower_bound(b + 1, b + 1 + len, a[i]) - b;
        a[i] = t;
    }
    for (ll i = 1; i <= n; ++i)
    {
        ans += query(len) - query(a[i]);
        update(a[i]);
    }
    cout << ans << endl;
    return 0;
}
```

权值线段树写法1
```c
#include <bits/stdc++.h>
#define endl '\n'
#define lson(x) x<<1
#define rson(x) (x<<1)|1
#define lowbit(x) x&-x
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef long double ld; typedef pair<ll, ll> pll;

const ll maxn = 5e5 + 5;
ll n, a[maxn], b[maxn], tree[maxn << 2];

inline void pushup(ll p)
{
    tree[p] = tree[lson(p)] + tree[rson(p)];
}
void update(ll s, ll t, ll p, ll x)
{
    if (s == t)
    {
        ++tree[p];
        return;
    }
    ll mid = (s + t) >> 1;
    if (x <= mid) update(s, mid, lson(p), x);
    else update(mid + 1, t, rson(p), x);
    pushup(p);
}
ll query(ll l, ll r, ll s, ll t, ll p)
{
    if (l <= s && t <= r)
    {
        return tree[p];
    }
    ll mid = (s + t) >> 1;
    ll res = 0;
    if (l <= mid) res += query(l, r, s, mid, lson(p));
    if (r > mid) res += query(l, r, mid + 1, t, rson(p));
    return res;
}

int main()
{
    ll ans = 0;
    cin >> n;
    for (ll i = 1; i <= n; ++i) cin >> a[i], b[i] = a[i];

    sort(b + 1, b + 1 + n);
    ll len = unique(b + 1, b + 1 + n) - b - 1;
    for (ll i = 1; i <= n; ++i)
    {
        ll t = lower_bound(b + 1, b + 1 + len, a[i]) - b;
        a[i] = t;
    }
    for (ll i = 1; i <= n; ++i)
    {
        ans += query(a[i] + 1, len, 1, len, 1);
        update(1, len, 1, a[i]);
    }
    cout << ans << endl;
    return 0;
}
```

权值线段树写法2
```c
#include <bits/stdc++.h>
#define endl '\n'
#define lson(x) x<<1
#define rson(x) (x<<1)|1
#define lowbit(x) x&-x
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef long double ld; typedef pair<ll, ll> pll;

const ll maxn = 5e5 + 5;
struct s_node
{
    ll l, r, v;
}node[maxn << 2];
ll n, a[maxn], b[maxn];

inline void pushup(ll p)
{
    node[p].v = node[lson(p)].v + node[rson(p)].v;
}
void build(ll s, ll t, ll p)
{
    node[p].l = s;
    node[p].r = t;
    node[p].v = 0;
    if (s == t) return;
    ll mid = (s + t) >> 1;
    build(s, mid, lson(p));
    build(mid + 1, t, rson(p));
}
void update(ll p, ll x)
{
    if (node[p].l == node[p].r)
    {
        ++node[p].v;
        return;
    }
    ll mid = (node[p].l + node[p].r) >> 1;
    if (x <= mid) update(lson(p), x);
    else update(rson(p), x);
    pushup(p);
}
ll query(ll l, ll r, ll p)
{
    if (l <= node[p].l && node[p].r <= r)
    {
        return node[p].v;
    }
    ll mid = (node[p].l + node[p].r) >> 1;
    ll res = 0;
    if (l <= mid) res += query(l, r, lson(p));
    if (r > mid) res += query(l, r, rson(p));
    return res;
}

int main()
{
    ll ans = 0;
    cin >> n;
    for (ll i = 1; i <= n; ++i) cin >> a[i], b[i] = a[i];

    sort(b + 1, b + 1 + n);
    ll len = unique(b + 1, b + 1 + n) - b - 1;
    for (ll i = 1; i <= n; ++i)
    {
        ll t = lower_bound(b + 1, b + 1 + len, a[i]) - b;
        a[i] = t;
    }
    build(1, len, 1);
    for (ll i = 1; i <= n; ++i)
    {
        ans += query(a[i] + 1, len, 1);
        update(1, a[i]);
    }
    cout << ans << endl;
    return 0;
}
```