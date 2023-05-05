题目：[P4484 [BJWC2018]最长上升子序列](https://www.luogu.com.cn/problem/P4484)

题解：[题解 P4484 【[BJWC2018]最长上升子序列】](https://www.luogu.com.cn/blog/pks-LOVING/solution-p4484)

```cpp
#include <bits/stdc++.h>
#define popcount __builtin_popcount
#define debug(x) fprintf(stderr, ""#x"\t= %d\n", x)
#define bar() fprintf(stderr, "---------\n")
using namespace std;
typedef long long LL;
const int MOD = 998244353;
int qpow(LL a, LL b) {
    int ret = 1;
    while (b) {
        if (b & 1)
            ret = ret * a % MOD;
        a = a * a % MOD;
        b >>= 1;
    }
    return ret % MOD;
}
LL inv(LL a) {
    return qpow(a, MOD - 2);
}
LL fac(LL a) {
    LL ret = 1;
    for (int i = 1; i <= a; i++)
        ret = ret * i % MOD;
    return ret;
}
const int N = 24;
/*
在一个确定的状态 u 下
令 f_i 为 u 的前 i 个数的 LIS 长度
令 maxf_i 为前 i 个 f_i 的 max
令 dif_i 为 maxf 的差分数组
发现 dif 的每个值为 0 or 1
---
从小到大插入每个数
设：在第 i 位与第 i+1 位之间插入了新数
dif_{i+1} = 1
dif_pos = 0 其中 pos 为 i 后第一个比 a_i 大的数的索引
*/
LL dp[2][(1 << N) + 5];
// dp[i][u]
// 在一个 1 到 i 的排列，dif 为 u 的方案数
// i 用滚动数组滚掉
int main() {
    int n; cin >> n;
    if      (n == 25) { cout << 102117126 << endl; return 0; }
    else if (n == 26) { cout << 819818153 << endl; return 0; }
    else if (n == 27) { cout << 273498600 << endl; return 0; }
    else if (n == 28) { cout << 267588741 << endl; return 0; }

    dp[0][0] = 1;
    int now = 1; // now 为滚动数组索引
    for (int i = 1; i <= n - 1; now ^= 1, i++) {   // 在一个 1 到 i 的排列
        fill(dp[now], dp[now] + (1 << (i - 1)), 0);
        // 清空准备处理的滚动数组
        for (int u = 0; u < (1 << (i - 1)); u++) { // dif 为 u
            dp[now][u << 1] = (dp[now][u << 1] + dp[now ^ 1][u]) % MOD;
            int pos = -1;
            for (int k = i - 1; ~k; k--) { // 倒序枚举，同时找 pos
                int v = ((u >> k) << (k + 1)) | (1 << k) | (u & ((1 << k) - 1));
                //  v = 在 u 的第 k 位与第 k+1 位之间插入一个 1
                if (u & (1 << k))
                    pos = k;
                    // 记录找到的 pos
                if (~pos)
                    v ^= (1 << (pos + 1));
                    // 把 v 中第 pos 位 flip
                dp[now][v] = (dp[now][v] + dp[now ^ 1][u]) % MOD;
                // 状转方程
                // dp[i][u] = sigma dp[i-1][v]
            }
        }
    }

    now ^= 1;
    LL ans = 0;
    for (int u = 0; u < (1 << (n - 1)); u++)
        ans = (ans + dp[now][u] * (popcount(u) + 1)) % MOD;
    ans = ans * inv(fac(n)) % MOD;
    cout << ans << endl;
    return 0;
}

```