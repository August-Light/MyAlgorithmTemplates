模板题目：[P8436 【模板】边双连通分量](https://www.luogu.com.cn/problem/P8436)

板子：

```cpp
namespace Tarjan {
    int dfn[MAXN], low[MAXN], dfn_cnt;
    stack<int> st;
    vector<vector<int> > EDCCs;

    void Tarjan(int u, int lst) {
        low[u] = dfn[u] = ++dfn_cnt;
        st.push(u);

        for (int i = head[u]; i; i = e[i].nxt) {
            if (i == (lst ^ 1)) // 反边
                continue;
            int v = e[i].to;
            if (!dfn[v]) {
                Tarjan(v, i);
                low[u] = min(low[u], low[v]);
            } else
                low[u] = min(low[u], dfn[v]);
        }

        if (dfn[u] == low[u]) {
            vector<int> tmp;
            int v; do {
                v = st.top(); st.pop();
                tmp.push_back(v);
            } while (v != u);
            EDCCs.push_back(tmp);
        }
    }
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const int MAXN = 5e5 + 100;
const int MAXM = 2e6 * 2 + 100;



int main() {
    int n, m; cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        int u, v; cin >> u >> v;
        addedge(u, v);
        addedge(v, u);
    }

    for (int i = 1; i <= n; i++)
        if (!Tarjan::dfn[i])
            Tarjan::Tarjan(i, -1);

    cout << Tarjan::EDCCs.size() << endl;
    for (auto EDCC : Tarjan::EDCCs) {
        cout << EDCC.size() << ' ';
        for (auto i : EDCC)
            cout << i << ' ';
        cout << endl;
    }

    return 0;
}

```