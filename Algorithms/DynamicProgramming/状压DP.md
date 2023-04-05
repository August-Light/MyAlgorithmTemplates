题目：[P1896 [SCOI2005] 互不侵犯](https://www.luogu.com.cn/problem/P1896)

题解：深进P264

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
LL dp[9 + 5][100 + 5][1024 + 5];
int n, k;
LL ans;
bool judge(LL x, LL y) { // 两行是否合法
    if (x & y)
        return false;
    if ((x << 1) & y)
        return false;
    if ((x >> 1) & y)
        return false;
    return true;
}
int main() {
    cin >> n >> k;
    int tot = (1 << n) - 1;
    dp[0][0][0] = 1;
    for (int i = 1; i <= n; i++) // 前 i 行
        for (int j = 0; j <= k; j++) // 放了 j 个
            for (int k = 0; k <= tot; k++) { // 状态
                int L = __builtin_popcount(k); // 新放了 L 个国王
                if (L > j)
                    continue;
                if (k & (k >> 1)) // 去掉有相邻国王的状态
                    continue;
                for (int l = 0; l <= tot; l++) {
                    if ((l >> 1) & l)
                        continue;
                    if (!judge(k, l))
                        continue;
                    dp[i][j][k] += dp[i - 1][j - L][l];
                }
            }
    for (int i = 0; i <= tot; i++)
        ans += dp[n][k][i];
    cout << ans << endl;
    return 0;
}
```