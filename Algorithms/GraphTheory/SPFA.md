模板题目：

[P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)

题解：深进 P154

板子：

```cpp
int dis[MAXN];
queue<int> q;
bool in_queue[MAXN];
int cnt_queue[MAXN];
bool SPFA(int n, int s) {
    for (int u = 1; u <= n; u++)
        dis[u] = INF;
    dis[s] = 0;
    q.push(s);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        in_queue[u] = false;
        for (int i = head[u]; i; i = e[i].nxt) {
            int v = e[i].to, w = e[i].val;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                if (!in_queue[v]) {
                    q.push(v);
                    in_queue[v] = true;
                    cnt_queue[v]++;
                    if (cnt_queue[v] == n+1)
                        return 0;
                }
            }
        }
    }
    return 1;
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned int uint;


const int MAXN = 1e4 + 100;
const int MAXM = 5e5 + 100;
const int INF = INT_MAX;



int main() {
    int n = read(), m = read(), s = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addedge(u, v, w);
    }
    if (SPFA(n, s))
        for (int u = 1; u <= n; u++)
            write(dis[u], ' ');
    return 0;
}
```