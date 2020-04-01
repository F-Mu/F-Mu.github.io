---
title: CodeForces 1322B Present
tags: [二进制]
mathjax: true
date: 2020-03-18 18:15:14
categories: 二进制
---

**题意**

求对所有的二元组$(i,j)(i<j)$，$a_i+a_j$的异或和

<!--more-->

**分析**

考虑对于第$k$位的贡献，将所有数对$2^{k+1}$取模（因为大于等于$2^{k+1}$的部分对第$k$位不产生贡献）

对于任意$a_i$和$a_j$来说，和要在第$k$位为$1$（为$0$不产生贡献），即和的范围必须在$[2^k,2^{k+1}-1]$或$[2^k+2^{k+1},2^{k+2}-2]$中

如果产生的贡献为奇数，则答案中该位为$1$，否则为$0$

统计第$k$位时，则可以取模，排序，双指针（二分），分别统计两个区间内的数，合并

```cpp
#pragma GCC optimize(3, "Ofast", "inline")

#include <bits/stdc++.h>

#define start ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define ll long long
#define int ll
#define ls st<<1
#define rs st<<1|1
#define pii pair<int,int>
#define re(z, x, y) for(int z=x;z<=y;++z)
#define com bool operator<(const node &b)
using namespace std;
const int maxn = (ll) 4e5 + 5;
const int mod = 1000000007;
const int inf = 0x3f3f3f3f;
int a[maxn], b[maxn], n, ans;

bool cal(int L, int R) {
    int sum = 0;
    for (int i = n, l = 1, r = 1; i >= 1; --i) {
        while (l <= n && b[i] + b[l] < L)
            ++l;
        while (r <= n && b[i] + b[r] <= R)
            ++r;
        sum += r - l - (l <= i && i < r);
    }
    return (sum >> 1) & 1;
}

bool solve(int k) {
    re(i, 1, n)b[i] = a[i] & ((1 << (k + 1)) - 1);
    sort(b + 1, b + n + 1);
    return cal(1 << k, (1 << (k + 1)) - 1) ^ cal(3 << k, (1 << (k + 2)) - 2);
}

signed main() {
    start;
    cin >> n;
    re(i, 1, n)cin >> a[i];
    re(i, 0, 25)ans |= solve(i) << i;
    cout << ans;
    return 0;
}
```

