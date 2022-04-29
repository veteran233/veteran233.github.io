---
title: 牛客 - Matrix
top: false
cover: false
toc: true
mathjax: true
date: 2020-12-10 00:12:24
password:
summary:
tags: [牛客,hash]
categories:
---

## 题目
<!--more-->
https://ac.nowcoder.com/acm/problem/51003

## 思路
两次hash，对于M行N列的矩阵arr，对每一行进行一次hash，得到h；然后对于列，取长度为B的hash，即$h[i][r]-h[i][l-1]\times bash^B$，对每一列再用一次hash，得到h2。此时的$h2[i+a-1]-h2[i-1]\times bash2^A$即为小矩阵（该矩阵的左上角位于i行j列，右下角位于(i+a-1)行(j+b-1)列）的hash，简单来说就是把字符串的hash搬到了矩阵上来，使之变成矩阵的hash，这样每一个矩阵都对应唯一的一个hash值。
特别注意的是，该题不能用set来保存矩阵的hash，否则会MLE，必须使用数组保存然后排序。对于Q个询问，用二分查找判断是否存在于数组中，是则输出1，否则输出0。推荐用stl里的binary_search。
<del>我不会告诉你我用lower_bound，然后wa了3发</del>

## AC代码
```c
#include <bits/stdc++.h>
#define endl '\n'
#define lson(x) (x<<1)
#define rson(x) (x<<1|1)
#define lowbit(x) (x&-x)
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); } }_io;
typedef long long ll; typedef unsigned long long ull; typedef long double ld; typedef pair<ll, ll> pll;

ll exgcd(ll a, ll b, ll &x, ll &y) { if (!b) { x = 1; y = 0; return a; }ll d = exgcd(b, a % b, x, y); ll t = x; x = y; y = t - (a / b) * y; return d; }
ll getInv(ll a, ll mod) { ll x, y; return exgcd(a, mod, x, y) == 1 ? (x % mod + mod) % mod : -1; }
template<typename type>type fpow(type a, type b, type mod) { if (b < 0) return -1; type res = 1; while (b) { if (b & 1) res = (res * a) % mod; a = (a * a) % mod; b >>= 1; }return res; }
template<typename type>type fpow(type a, type b) { if (b < 0) return -1; type res = 1; while (b) { if (b & 1) res *= a; a *= a; b >>= 1; }return res; }

const ull maxn = 1005, base = 233, base2 = 19260817, N = 105;
ull h[maxn][maxn], th[N], h2[maxn], s[maxn * maxn], cnt;

ull mh(string str)
{
    ll len = str.size() - 1;
    ull res = 0;
    for (ll i = 1; i <= len; ++i)
    {
        res = res * base + str[i];
    }
    return res;
}
void mh(ll k, string str)
{
    ll len = str.size() - 1;
    for (ll i = 1; i <= len; ++i)
    {
        h[k][i] = h[k][i - 1] * base + str[i];
    }
}

int main()
{
    ll m, n, a, b;
    cin >> m >> n >> a >> b;
    ull b_powb = fpow(base, (ull)b), b_powa = fpow(base2, (ull)a);
    string arr;
    for (ll i = 1; i <= m; ++i)
    {
        cin >> arr;
        arr = ' ' + arr;
        mh(i, arr);
    }

    for (ll j = 1; j + b - 1 <= n; ++j)
    {
        ll l = j, r = j + b - 1;
        for (ll i = 1; i <= m; ++i) h2[i] = h2[i - 1] * base2 + h[i][r] - h[i][l - 1] * b_powb;
        for (ll i = 1; i + a - 1 <= n; ++i)
        {
            s[++cnt] = h2[i + a - 1] - h2[i - 1] * b_powa;
        }
    }
    sort(s + 1, s + 1 + cnt);
    cnt = unique(s + 1, s + 1 + cnt) - s - 1;

    ll q;
    cin >> q;
    while (q--)
    {
        string str;
        for (ll i = 1; i <= a; ++i)
        {
            cin >> str;
            str = ' ' + str;
            th[i] = mh(str);
        }
        ull qhash = 0;
        for (ll i = 1; i <= a; ++i) qhash = qhash * base2 + th[i];

        if (binary_search(s + 1, s + 1 + cnt, qhash)) cout << 1 << endl;
        else cout << 0 << endl;
    }
    return 0;
}
```