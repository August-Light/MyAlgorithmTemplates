模板题目：

[P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)

[P3385 【模板】负环](https://www.luogu.com.cn/problem/P3385)

题解：深进P154

板子：

```cpp
Graph g;
namespace SPFA {
    int dis[MAXN], cnt[MAXN];
    bool vis[MAXN];
    queue<int> q;
    bool SPFA(int n, int s) {
        memset(vis, 0, sizeof(vis));
        memset(cnt, 0, sizeof(cnt));
        for (int i = 1; i <= n; i++)
            dis[i] = INF;
        dis[s] = 0; vis[s] = 1; q.push(s);
        while (!q.empty()) {
            int u = q.front(); q.pop();
            vis[u] = 0;
            for (int i = g.head[u]; i; i = g.e[i].nxt) {
                Edge edge = g.e[i];
                if (dis[edge.to] > (LL)dis[u] + edge.val) { // relax
                    dis[edge.to] = dis[u] + edge.val;
                    if (!vis[edge.to]) {
                        q.push(edge.to);
                        vis[edge.to] = 1;
                        cnt[edge.to]++;
                        if (cnt[edge.to] > n)
                            return 0;
                    }
                }
            }
        }
        return 1;
    }
}
```

使用：

P3371

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int INF = INT_MAX;
const int MAXN = 1e4 + 100;
const int MAXM = 5e5 + 100;


int main() {
    int n, m, s;
    cin >> n >> m >> s;
    for (int i = 1, u, v, w; i <= m; i++) {
        cin >> u >> v >> w;
        g.addedge(u, v, w);
    }
    SPFA::SPFA(n, s);
    for (int i = 1; i <= n; i++)
        cout << SPFA::dis[i] << " ";
    return 0;
}
```

P3385

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 2e3 + 100;
const int MAXM = 6e3 + 100;
const int INF = INT_MAX;


int main() {
    int t;
    cin >> t;
    while (t--) {
        int n, m;
        g.init();
        cin >> n >> m;
        for (int i = 1, u, v, w; i <= m; i++) {
            cin >> u >> v >> w;
            g.addedge(u, v, w);
            if (w >= 0)
                g.addedge(v, u, w);
        }
        if (!SPFA::SPFA(n, 1))
            puts("YES");
        else
            puts("NO");
    }
    return 0;
}
```