---
title: CCPC 2020 秦皇岛 E - Exam Results
top: false
cover: false
toc: true
mathjax: true
date: 2020-10-20 18:50:02
password:
summary:
tags: [ccpc,尺取]
categories:
---

## 题目
<!--more-->
Professor Alex is preparing an exam for his students now.

There will be $n$ students participating in this exam. If student $i$ has a good mindset, he/she will perform well and get $a_i$ points. Otherwise, he/she will get $b_i$ points. So it is impossible to predict the exam results. Assume the highest score of these students is $x$ points. The students with a score of no less than $x⋅p\%$ will pass the exam.

Alex needs to know what is the maximum number of students who can pass the exam in all situations. Can you answer his question?

## 思路
（待完善）

## AC代码
```c
#define _CRT_SECURE_NO_WARNINGS
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
struct _IO { _IO() { ios::sync_with_stdio(0); cin.tie(0); } }_io;
typedef long long ll; typedef long double db;

const ll maxn = 2e5 + 5;
array<pair<ll, ll>, maxn> a;
array<ll, maxn> count1;
vector<pair<ll, ll>> b;

int main()
{
	ll T, test = 1;
	cin >> T;
	while (T--)
	{
		ll n, p;
		cin >> n >> p;

		b.clear();
		for (ll i = 0; i < n; ++i)
		{
			cin >> a[i].first >> a[i].second;
			if (a[i].first > a[i].second) swap(a[i].first, a[i].second);
			b.push_back({ a[i].first,i });
			b.push_back({ a[i].second,i });
		}
		sort(a.begin(), a.begin() + n);
		sort(b.begin(), b.end());

		memset(&count1, 0, 8 * n);

		ll cnt = 0, ans = 0, j = 0;
		for (ll i = 0; i < b.size(); ++i)
		{
			++count1[b[i].second];
			if (count1[b[i].second] == 1) ++cnt;
			if (b[i].first < a[n - 1].first) continue;

			while (j <= i && b[j].first * 100 < b[i].first * p)
			{
				--count1[b[j].second];
				if (!count1[b[j++].second]) --cnt;
			}
			ans = max(ans, cnt);
		}

		cout << "Case #" << test++ << ": " << ans << endl;
	}
	return 0;
}
```