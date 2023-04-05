模板题目：[P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

题解：深进P142

板子（倍增）：

在线，预处理 $O(n\log n)$，查询 $O(\log n)$

```cpp
int d[MAXN], anc[MAXN][MAXT + 5];
vector<int> G[MAXN];
void addedge(int u, int v) {
    G[u].push_back(v);
    G[v].push_back(u);
}
namespace lca {
    void dfs(int u, int fa) {
        for (int i = 0; i < G[u].size(); i++) {
            int v = G[u][i];
            if (v == fa)
                continue;
            d[v] = d[u] + 1;
            anc[v][0] = u;
            dfs(v, u);
        }
    }
    void init() {
        d[s] = 1;
        dfs(s, 0);
        for (int j = 1; j <= MAXT; j++)
            for (int i = 1; i <= n; i++)
                anc[i][j] = anc[anc[i][j - 1]][j - 1];
    }
    int query(int u, int v) {
        if (d[u] < d[v])
            swap(u, v);
        for (int i = MAXT; i >= 0; i--)
            if (d[anc[u][i]] >= d[v])
                u = anc[u][i];
        if (u == v)
            return u;
        for (int i = MAXT; i >= 0; i--)
            if (anc[u][i] != anc[v][i])
                u = anc[u][i], v = anc[v][i];
        return anc[u][0];
    }
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
int read();
void write(int x);
const int MAXN = 5e5 + 5;
const int MAXT = 18;
int n, m, s, x, y, a, b;

int main() {
    n = read();
    m = read();
    s = read();
    for (int i = 1; i < n; i++) {
        x = read();
        y = read();
        addedge(x, y);
    }
    lca::init();
    while (m--) {
        a = read();
        b = read();
        write(lca::query(a, b));
        putchar('\n');
    }
    return 0;
}
```