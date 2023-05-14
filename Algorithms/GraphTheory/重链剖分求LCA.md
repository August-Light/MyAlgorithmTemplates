模板题目：[P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

```cpp
namespace HLD { // 重链剖分
    int fa[MAXN], dep[MAXN], siz[MAXN], son[MAXN], top[MAXN];
    void dfs1(int u, int f) {
        siz[u] = 1;
        fa[u] = f;
        dep[u] = dep[f] + 1;
        for (int i = head[u]; i; i = e[i].nxt) {
            int v = e[i].to;
            if (v == f)
                continue;
            dfs1(v, u);
            siz[u] += siz[v];
            if (siz[v] > siz[son[u]])
                son[u] = v;
        }
    }
    void dfs2(int u, int tp) {
        top[u] = tp;
        if (!son[u])
            return;
        dfs2(son[u], tp);
        for (int i = head[u]; i; i = e[i].nxt) {
            int v = e[i].to;
            if (v == fa[u] || v == son[u])
                continue;
            dfs2(v, v);
        }
    }
    void init(int rt) {
        dfs1(rt, 0);
        dfs2(rt, rt);
    }
    int LCA(int u, int v) {
        while (top[u] != top[v]) {
            if (dep[top[u]] < dep[top[v]])
                swap(u, v);
            u = fa[top[u]];
        }
        if (dep[u] > dep[v])
            swap(u, v); // u is LCA
        return u;
    }
}
using HLD::LCA;
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int MAXN = 5e5 + 100;
const int MAXM = MAXN * 2;




int main() {
    int n = read(), m = read(), s = read();
    for (int i = 1; i <= n - 1; i++) {
        int u = read(), v = read();
        addedge(u, v);
        addedge(v, u);
    }
    HLD::init(s);
    while (m--) {
        int u = read(), v = read();
        write(LCA(u, v), '\n');
    }
    return 0;
}

```