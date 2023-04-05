模板题目：[B3647 【模板】Floyd 算法](https://www.luogu.com.cn/problem/B3647)

题解：深进P164

板子：

```cpp
void floyd(int n, int dis[MAXN][MAXN]) {
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                dis[i][j] = min((LL)dis[i][j], (LL)dis[i][k] + dis[k][j]);
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 105;

int n, m, dis[MAXN][MAXN];
int main() {
    cin >> n >> m;
    memset(dis, 0x3f, sizeof(dis));
    for (int i = 1; i <= n; i++)
        dis[i][i] = 0;
    for (int i = 1; i <= m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        dis[u][v] = w;
        dis[v][u] = w;
    }
    floyd(n, dis);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++)
            cout << dis[i][j] << ' ';
        cout << endl;
    }
    return 0;
}
```