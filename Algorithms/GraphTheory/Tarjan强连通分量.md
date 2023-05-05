模板题目：[P2863 [USACO06JAN]The Cow Prom S](https://www.luogu.com.cn/problem/P2863)

题解：深进 P194

板子：

```cpp
namespace Tarjan {
    int dfn[MAXN], low[MAXN], dfn_cnt;
    stack<int> st;
    bool in_stack[MAXN];
    vector<vector<int> > SCCs;

    void Tarjan(int u) {
        low[u] = dfn[u] = ++dfn_cnt;
        st.push(u); in_stack[u] = true;

        for (int i = head[u]; i; i = e[i].nxt) {
            int v = e[i].to;
            if (!dfn[v]) {
                Tarjan(v);
                low[u] = min(low[u], low[v]);
            } else if (in_stack[v])
                low[u] = min(low[u], dfn[v]);
        }

        if (dfn[u] == low[u]) {
            vector<int> tmp;
            int v; do {
                v = st.top(); st.pop(); in_stack[v] = false;
                tmp.push_back(v);
            } while (v != u);
            SCCs.push_back(tmp);
        }
    }
}
```

使用：

[完整代码](https://www.luogu.com.cn/paste/1js7ar59)