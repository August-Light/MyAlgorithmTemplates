题目：[P1896 [SCOI2005] 互不侵犯](https://www.luogu.com.cn/problem/P1896)

题解：深进P264

```cpp
#include <bits/stdc++.h>
#define popcount __builtin_popcount
using namespace std;
typedef long long LL;
const int N = 9;
LL f[N + 5][N * N + 5][(1 << N) + 5];
// f[i][j][u]
// 前 i 行，总共放了 j 个国王，第 i 行状态为 u 的方案数
int main() {
    int n, k;
    cin >> n >> k;
    int tot = (1 << n) - 1;
    f[0][0][0] = 1;
    for (int i = 1; i <= n; i++)             // 前 i 行
        for (int j = 0; j <= k; j++)         // 放了 j 个
            for (int u = 0; u <= tot; u++) { // 第 i 行的状态 u
                int L = popcount(u);         // u 有 L 个国王
                if (L > j) // 国王太多了
                    continue;
                if ((u >> 1) & u) // u 不合法
                    continue;
                for (int v = 0; v <= tot; v++) { // 第 i-1 行的状态 v
                    if ((v >> 1) & v) // v 不合法
                        continue;
                    if ((u & v) || ((u << 1) & v) || ((u >> 1) & v)) // u 与 v 之间不合法
                        continue;
                    f[i][j][u] += f[i - 1][j - L][v];
                }
            }
    LL ans = 0;
    for (int u = 0; u <= tot; u++)
        ans += f[n][k][u];
    cout << ans << endl;
    return 0;
}
```