---
title: '[ICPC Japan 2018] Floating-Point Numbers'
tags: [二进制,模拟]
mathjax: true
date: 2020-01-27 13:36:57
categories: 二进制
---

**题意**

给出$64$位二进制浮点数的表示方式，其中前$12$是指数，后$52$位是小数部分，整数部分默认为$1$，给出整数$n$，和按该表示方法给出的浮点数$a$的小数部分（后$52$位），输出$a$在该表示方法下连续加和$n$次（即$n+1$个$a$依次相加）的$64$位二进制浮点数的表示。

<!-- more -->

**分析**

题面已经给出了该表示方法存在累积误差，因此无法使用$Java$或$Python$的大数来直接相乘。

因此采用模拟。

1. 将$a$使用一个$53$位整数（二进制）表示出来，注意默认$a$的小数点前有一个$1$，将这个$1$作为第一位。
2. 设置一个当前的加和$ans$，每一次循环中$ans$加上$a$。
3. 若$ans+a\geq 2^{53}$，即和超过了$53$位，这个时候将指数计数器$e$加一，$ans$右移一位，注意到此时$a$的最后一位对加和没有贡献了（题意给出加法是截断的），$a$也需要右移一位，若此时$a$已经变为了$0$，则$a$不在对加和产生贡献，跳出循环即可
4. 输出答案，注意到表示采用二进制，可以采用栈$+$除法输出，也可直接采用位运算。

若是一次次相加，看数据规模，必然会$TLE$。

观察第$2$步，可以产生一个优化想法，由于若不产生进位，两次循环之间的操作是没有变的，我们可以使用乘法代替，那么我们需要寻找一个临界点，两个临界点之间的操作可以用乘法代替，保证每次循环一定进位或跳出。

注意到进位条件是$ans+a\geq 2^{53}$，那么加法次数保证$ans+a<2^{53}$即可，该次数为$((1ll << 53) - 1 - ans) / a$，为了产生进位，每次循环加上$((1ll << 53) - 1 - ans) / a+1$个$a$即可，然后进行第$3$步即可

```cpp
#pragma GCC optimize(3, "Ofast", "inline")

#include <bits/stdc++.h>

#define start ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define ll long long
#define LL long long
#define int ll//方便
#define ls st<<1
#define rs st<<1|1
#define pii pair<int,int>
using namespace std;
const int maxn = (ll) 1e5 + 5;
const int mod = 1000000007;
const int inf = 0x3f3f3f3f;

signed main() {
    start;
    int n;
    while (cin >> n && n) {
        int a = 0;
        for (int i = 0; i < 52; ++i) {//一共输入后面的52位
            a <<= 1;
            char c;
            cin >> c;
            if (c == '1')
                a++;
        }
        a += 1ll << 52;//该方法计数，小数点前数字为1
        int e = 0, ans = a;//e表示指数部分，ans表示小数部分
        /*模拟加法*/
        while (n) {
            int t = ((1ll << 53) - 1 - ans) / a + 1;//前半部分代表刚好保证不进位需要加多少次，加1表示刚好进位需要加多少次
            if (t > n) {//如果加上n次仍然不进位，则数字可加上n次的a
                ans += n * a;
                break;
            } else {//将当前数字加上t次原数字
                ans += t * a;
                ++e;//进位，指数加一
                ans >>= 1;//由于指数加一，二进制表示的小数需要右移一位
                a >>= 1;//由于指数加一，原小数的最后一位对答案没有贡献，丢弃
                if (!a)//如果原小数对答案已经没有贡献，则跳出
                    break;
                n -= t;//加了t次
            }
        }
        for (int i = 11; i >= 0; --i)//输出12位指数
            cout << (((1ll << i) & e) ? 1 : 0);
        for (int i = 51; i >= 0; --i)//输出52位小数
            cout << (((1ll << i) & ans) ? 1 : 0);
        cout << '\n';
    }
    return 0;
}
```

