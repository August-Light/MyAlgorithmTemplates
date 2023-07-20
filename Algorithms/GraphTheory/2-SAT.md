模板题目：[P4782 【模板】2-SAT 问题](https://www.luogu.com.cn/problem/P4782)

题解：深进 P200

```cpp
#include <bits/stdc++.h>
#define debug(x) fprintf(stderr, ""#x"\t= %d\n", x); fflush(stderr)
#define bar() fprintf(stderr, "---------\n"); fflush(stderr)
using namespace std;
typedef long long LL;


const int MAXN = 2e6 + 100;
const int MAXM = 2e6 + 100;




using Tarjan::bel;

int pt[MAXN >> 1][2];

int main() {
    int n = read(), m = read();
    for (int i = 1, t = 0; i <= n; i++)
        pt[i][0] = ++t, pt[i][1] = ++t;
    for (int i = 1; i <= m; i++) {
        int x = read(), a = read(), y = read(), b = read();
        addedge(pt[x][!a], pt[y][b]);
        addedge(pt[y][!b], pt[x][a]);
    }
    for (int i = 1; i <= (n << 1); i++)
        if (!Tarjan::dfn[i])
            Tarjan::Tarjan(i);
    for (int i = 1; i <= n; i++) {
        if (bel[pt[i][0]] == bel[pt[i][1]]) {
            puts("IMPOSSIBLE");
            return 0;
        }
    }
    puts("POSSIBLE");
    for (int i = 1; i <= n; i++) {
        int d = !(bel[pt[i][0]] < bel[pt[i][1]]);
        printf("%d%c", d, " \n"[i == n]);
    }
    return 0;
}
```