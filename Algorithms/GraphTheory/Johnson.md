模板题目：[P5905 【模板】Johnson 全源最短路](https://www.luogu.com.cn/problem/P5905)

题解：[[洛谷日报#242]Johnson 全源最短路径算法学习笔记](https://www.luogu.com.cn/blog/StudyingFather/johnson-algorithm)

板子：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 3e3 + 100;
const int MAXM = 6e3 + 100 + MAXN;
const int INF = 1e9;


int main() {
    int n, m;
    cin >> n >> m;
    for (int i = 1, u, v, w; i <= m; i++) {
        cin >> u >> v >> w;
        addedge(u, v, w);
    }
    for (int i = 1; i <= n; i++)
        addedge(0, i, 0);
    if (!SPFA::SPFA(n, 0)) {
        puts("-1");
        return 0;
    }
    for (int u = 1; u <= n; u++)
        for (int i = head[u]; i; i = e[i].nxt)
            e[i].val += SPFA::dis[u] - SPFA::dis[e[i].to];
    for (int i = 1; i <= n; i++) {
        Dijkstra::Dijkstra(n, i);
        LL ret = 0, ans = 0;
        for (int j = 1; j <= n; j++) {
            if (Dijkstra::dis[j] == INF)
                ans = INF;
            else
                ans = (Dijkstra::dis[j] + SPFA::dis[j] - SPFA::dis[i]);
            ret += j * ans;
        }
        cout << ret << endl;
    }
    return 0;
}
```