模板题目：

[P8435 【模板】点双连通分量](https://www.luogu.com.cn/problem/P8435)

[P3388 【模板】割点（割顶）](https://www.luogu.com.cn/problem/P3388)

板子：

```cpp
namespace Tarjan {
    int rt;
    int dfn[MAXN], low[MAXN], dfn_cnt;
    stack<int> st;
    vector<vector<int> > VDCCs;
    bool cutv[MAXN];

    void Tarjan(int u, int lst) {
        int son = 0;
        low[u] = dfn[u] = ++dfn_cnt;
        st.push(u);

        for (int i = head[u]; i; i = e[i].nxt) {
            if (i == (lst ^ 1)) // 反边
                continue;
            int v = e[i].to;
            if (!dfn[v]) {
                son++;
                Tarjan(v, i);
                low[u] = min(low[u], low[v]);
                if (low[v] >= dfn[u]) {
                    if (u != rt)
                        cutv[u] = 1;
                    vector<int> tmp;
                    tmp.push_back(u);
                    int v_; do {
                        v_ = st.top(); st.pop();
                        tmp.push_back(v_);
                    } while (v_ != v);
                    VDCCs.push_back(tmp);
                }
            } else
                low[u] = min(low[u], dfn[v]);
        }
        if (u == rt) {
            if (son >= 2)
                cutv[u] = 1;
            else if (son == 0) {
                vector<int> tmp;
                tmp.push_back(rt);
                VDCCs.push_back(tmp);
            }
        }
    }
}
```

使用：

P8435

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
        if (!Tarjan::dfn[i]) {
            Tarjan::rt = i;
            Tarjan::Tarjan(i, -1);
        }

    cout << Tarjan::VDCCs.size() << endl;
    for (auto VDCC : Tarjan::VDCCs) {
        cout << VDCC.size() << ' ';
        for (auto i : VDCC)
            cout << i << ' ';
        cout << endl;
    }

    return 0;
}
```

P3388

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
        if (!Tarjan::dfn[i]) {
            Tarjan::rt = i;
            Tarjan::Tarjan(i, -1);
        }

    int ans = 0;
    for (int i = 1; i <= n; i++)
        ans += Tarjan::cutv[i];
    printf("%d\n", ans);
    for (int i = 1; i <= n; i++)
        if (Tarjan::cutv[i])
            printf("%d ", i);

    return 0;
}
```