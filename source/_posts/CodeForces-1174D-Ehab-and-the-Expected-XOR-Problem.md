---
title: CodeForces 1174D Ehab and the Expected XOR Problem
date: 2019-11-23 17:26:18
tags: [二进制]
categories: 数论
mathjax: true
---

**题意**
给定两个数$n$和$x$，构造一个序列，设为$a[l]$($l$不确定）
$1$、$1\leq a[i]<2^{n}$
$2$、序列中没有子序列异或和为$0$或$x$
$3$、$l$应最长

<!-- more-->

**分析**
$1$、设前$i$个数字异或和为$sum_{i}$，则对于$[i,j]$的异或和为$sum_{i}⨁sum_{j-1}$，所以我们可以找出$sum$数组，满足

$$\forall i,j,sum_{i}\oplus sum_{j}\neq 0\&\&sum_{i}\oplus sum_{j}\neq x$$

$2$、异或性质有$y⨁z=x$，则有$y⨁x=z$，且对于任意一个数$y$，$z$是惟一的。且由于$y$和$z$在二进制下是不可能超过$2^{n}$的，因此，相对立的$y$和$z$在$2^{n}$内成对存在，设$i$从$1$开始遍历到$2^{n}$，我们与将$i$对立的数标记即可，我们可以得到$sum$数组
$3$、$a[i]=sum[i]⨁sum[i-1]$，输出数组即可
```cpp
#pragma GCC optimize(3, "Ofast", "inline")

#include <bits/stdc++.h>

#define start ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define ll long long
#define int ll
#define ls st<<1
#define rs st<<1|1
#define pii pair<int,int>
using namespace std;
const int maxn = (ll) 5e5 + 5;
const int mod = 1000000007;
const int mod2 = 1000000006;
const int inf = (ll) 1e9 + 5;
bool vis[maxn];

signed main() {
    start;
    int n, x;
    cin >> n >> x;
    vis[x] = vis[0] = true;
    vector<int> ans;
    ans.push_back(0);
    for (int i = (1 << n) - 1; i > 0; --i)
        if (!vis[i]) {
            ans.push_back(i);
            vis[i ^ x] = true;
        }
    cout << ans.size() - 1 << '\n';
    for (int i = 1; i < ans.size(); ++i)
        cout << (ans[i - 1] ^ ans[i]) << ' ';
    return 0;
}
```