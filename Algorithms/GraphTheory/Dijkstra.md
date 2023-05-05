模板题目：

[P4779 【模板】单源最短路径（标准版）](https://www.luogu.com.cn/problem/P4779)

题解：深进 P151

使用场景：非负权图。

板子：

```cpp
namespace Dijkstra {
    int dis[MAXN];
    bool vis[MAXN];
    struct Node {
        int v, w;
        Node(int _v, int _w) : v(_v), w(_w) {}
        friend bool operator < (Node a, Node b) {
            return a.w > b.w;
        }
    };
    void Dijkstra(int n, int s) {
        priority_queue<Node> q;
        memset(vis, 0, sizeof(vis));
        for (int i = 1; i <= n; i++)
            dis[i] = INF;
        dis[s] = 0;
        q.push(Node(s, 0));
        while (!q.empty()) {
            int u = q.top().v; q.pop();
            if (vis[u])
                continue;
            vis[u] = 1;
            for (int i = head[u]; i; i = e[i].nxt) {
                Edge edge = e[i];
                if (dis[edge.to] > (LL)dis[u] + edge.val) { // relax
                    dis[edge.to] = dis[u] + edge.val;
                    q.push(Node(edge.to, dis[edge.to]));
                }
            }
        }
    }
}
```

使用：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
const int INF = INT_MAX;
const int MAXN = 1e5 + 100;
const int MAXM = 2e5 + 100;


int main() {
    int n, m, s;
    cin >> n >> m >> s;
    for (int i = 1, u, v, w; i <= m; i++) {
        cin >> u >> v >> w;
        addedge(u, v, w);
    }
    Dijkstra::Dijkstra(n, s);
    for (int i = 1; i <= n; i++)
        cout << Dijkstra::dis[i] << " ";
    return 0;
}
```