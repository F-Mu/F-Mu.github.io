---
title: Codeforces 1221D - Make The Fence Great Again
tags: [DP]
date: 2019-11-25 19:33:05
categories: DP
mathjax: true
---

**题意**

对于$n$个栅栏，对于每个$i$，有高度$a[i]$，对于任意$2<=i<=n$,有$a[i]\not=a[i-1]$，则称该组栅栏为好栅栏，每个栅栏可花费$b[i]$提升$1$个高度（可无限提升）。给一组栅栏，问最少花费多少可以将这组栅栏变为好栅栏。

<!--more-->

**分析**

对于第$i$个栅栏，他要保证不与第$i-1$和$i+1$个栅栏相同，最多提升$2$，如果提升$2$与第$i-1$或$i+1$相同，则可选择提升$0$或$1$,同理如果此时与另一侧栅栏相同，则可提升$0$或$1$使该栅栏与两侧栅栏不同。题意给出其实提醒了$DP$（说$a[i]\not= a[i-1]$）。我们设置$DP[i][j]$表示对于第$i$个栅栏，提升$j$后，使得前$i$个栅栏为好栅栏，$0<=j<=2$。
$(1)$对于$a[i]=a[i-1]$的情况
如果第$i$个栅栏提升$0$，则第$i-1$个栅栏需提升$1$或者$2$，那么有
$$
dp[i][0] = min(dp[i - 1][1], dp[i - 1][2]
$$
如果第$i$个栅栏提升$1$，则第$i-1$个栅栏需提升$0$或者$2$，那么有
$$
dp[i][1] = min(dp[i - 1][0], dp[i - 1][2]) + b[i]
$$
如果第$i$个栅栏提升$2$，则第$i-1$个栅栏需提升$0$或者$1$，那么有
$$
dp[i][2] = min(dp[i - 1][0], dp[i - 1][1]) + b[i] * 2
$$
$(2)$对于$a[i]=a[i-1]+1$的情况
如果第$i$个栅栏提升$0$，则第$i-1$个栅栏需提升$0$或者$2$，那么有
$$
dp[i][0] = min(dp[i - 1][0], dp[i - 1][2])
$$
如果第$i$个栅栏提升$1$，则第$i-1$个栅栏需提升$0$或者$1$，那么有
$$
dp[i][1] = min(dp[i - 1][0], dp[i - 1][1]) + b[i]
$$
如果第$i$个栅栏提升$2$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1],dp[i-1][2])) + b[i] * 2
$$
$(3)$对于$a[i]=a[i-1]+2$的情况
如果第$i$个栅栏提升$0$，则第$i-1$个栅栏需提升$0$或者$1$，那么有
$$
dp[i][0] = min(dp[i - 1][0], dp[i - 1][1])
$$
如果第$i$个栅栏提升$1$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i]
$$
如果第$i$个栅栏提升$2$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i] * 2
$$
$(4)$对于$a[i]=a[i-1]-1$的情况
如果第$i$个栅栏提升$0$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]))
$$
如果第$i$个栅栏提升$1$，则第$i-1$个栅栏需提升$1$或者$2$，那么有
$$
dp[i][1] = min(dp[i - 1][1], dp[i - 1][2]) + b[i]
$$
如果第$i$个栅栏提升$2$，则第$i-1$个栅栏需提升$0$或者$2$，那么有
$$
dp[i][2] = min(dp[i - 1][0], dp[i - 1][2]) + b[i] * 2
$$
$(5)$对于$a[i]=a[i-1]-2$的情况
如果第$i$个栅栏提升$0$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]))
$$
如果第$i$个栅栏提升$1$，则第$i-1$个栅栏需提升$0$或者$1$或者$2$，那么有
$$
dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i]
$$
如果第$i$个栅栏提升$2$，则第$i-1$个栅栏需提升$1$或者$2$，那么有
$$
dp[i][2] = min(dp[i - 1][1], dp[i - 1][2]) + b[i] * 2
$$
$(6)其他情况
第$i$个栅栏提升$0,1,2$，第$i-1$个栅栏可提升$0$或者$1$或者$2$，有
$$
dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]))
$$
$$
dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i]
$$
$$
dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i] * 2
$$
最后输出$min(dp[n][0], min(dp[n][1], dp[n][2]))$即可

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
const int maxn = (ll) 3e5 + 5;
const int mod = 1000000007;
const int inf = 0x3f3f3f3f;
int dp[maxn][3];
int a[maxn], b[maxn];

signed main() {
    start;
    int q;
    cin >> q;
    while (q--) {
        int n;
        cin >> n;
        for (int i = 1; i <= n; ++i) {
            cin >> a[i] >> b[i];
            dp[i][0] = dp[i][1] = (ll) (1e18) + 5;//千万不能用memset
        }
        /*初始化*/
        dp[1][0] = 0;
        dp[1][1] = b[1];
        dp[1][2] = b[1] * 2;
        for (int i = 2; i <= n; ++i) {
            if (a[i] == a[i - 1]) {
                dp[i][0] = min(dp[i - 1][1], dp[i - 1][2]);
                dp[i][1] = min(dp[i - 1][0], dp[i - 1][2]) + b[i];
                dp[i][2] = min(dp[i - 1][0], dp[i - 1][1]) + b[i] * 2;
            } else if (a[i] == a[i - 1] + 1) {
                dp[i][0] = min(dp[i - 1][0], dp[i - 1][2]);
                dp[i][1] = min(dp[i - 1][0], dp[i - 1][1]) + b[i];
                dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1],dp[i-1][2])) + b[i] * 2;
            } else if (a[i] == a[i - 1] + 2) {
                dp[i][0] = min(dp[i - 1][0], dp[i - 1][1]);
                dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i];
                dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i] * 2;
            } else if (a[i] == a[i - 1] - 1) {
                dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]));
                dp[i][1] = min(dp[i - 1][1], dp[i - 1][2]) + b[i];
                dp[i][2] = min(dp[i - 1][0], dp[i - 1][2]) + b[i] * 2;
            } else if (a[i] == a[i - 1] - 2) {
                dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]));
                dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i];
                dp[i][2] = min(dp[i - 1][1], dp[i - 1][2]) + b[i] * 2;
            } else {
                dp[i][0] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2]));
                dp[i][1] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i];
                dp[i][2] = min(dp[i - 1][0], min(dp[i - 1][1], dp[i - 1][2])) + b[i] * 2;
            }
        }
        cout << min(dp[n][0], min(dp[n][1], dp[n][2])) << '\n';
    }
    return 0;
}
```
（这题我竟然和一个吉尔吉斯斯坦的小姐姐代码撞了，被判重然后unrated，哭了）
