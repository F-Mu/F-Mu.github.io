---
title: CodeForces 1187E Tree Painting
tags: [树,DP]
date: 2019-11-25 19:24:21
categories: DP
mathjax: true
---

题意：给定一棵$n$个点的树 初始全是白点
要求你做$n$步操作，每一次选定一个与一个黑点相隔一条边的白点，将它染成黑点，然后获得该白点被染色前所在的白色联通块大小的权值。
第一次操作可以任意选点。
求可获得的最大权值。

<!--more-->

分析：进行换根树形$DP$，对于某一个起点来说，答案是固定的
设以节点$i$为起点的答案为$ans(i)$，其子树大小（包括自己）为$siz[i]$
那么假设我们得到了节点$u$的答案，对于他的儿子$v$来说，需要加上从$v$蔓延到$u$的过程，即需要加上$n-siz[v]$，同时减去从$u$蔓延$v$的过程，即减去$siz[v]$，即有
$$
ans(v) = ans(u) + (n - siz[i]) - siz[i]
$$

所以先求出各个节点的$siz[i]$，从节点$1$开始进行$dfs$进行如上操作，而以节点$1$为起点的答案为$\sum\limits_{i=1}^{n}siz[i]$

``` cpp
#pragma GCC optimize(3, "Ofast", "inline")

#include <bits/stdc++.h>

#define start ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define ll long long
#define int ll
#define ls st<<1
#define rs st<<1|1
#define pii pair<int,int>
using namespace std;
const int maxn = (ll) 3e5 + 5;
const int mod = 1000000007;
const int inf = 0x3f3f3f3f;
int n;
vector<int> v[maxn];
int siz[maxn];
int tot = 0;

void dfs1(int u, int fa) {
    siz[u] = 1;
    for (auto &i:v[u]) {
        if (i == fa)
            continue;
        dfs1(i, u);
        siz[u] += siz[i];
    }
    tot += siz[u];
}

int ans;

void dfs2(int u, int fa, int sum) {
    ans = max(ans, sum);
    for (auto &i:v[u]) {
        if (i == fa)
            continue;
        int tmp = sum + (n - siz[i]) - siz[i];
        dfs2(i, u, tmp);
    }
}

signed main() {
    start;
    cin >> n;
    for (int i = 1; i < n; ++i) {
        int u, z;
        cin >> u >> z;
        v[u].push_back(z);
        v[z].push_back(u);
    }
    dfs1(1, 0);
    dfs2(1, 0, tot);
    cout << ans;
    return 0;
}
```