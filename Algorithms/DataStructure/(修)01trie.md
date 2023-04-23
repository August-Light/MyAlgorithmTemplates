题目：[P4551 最长异或路径](https://www.luogu.com.cn/problem/P4551)

题解：深基P109

板子：

```cpp
namespace trie {
    int tot, trie[MAXN * 31][2];
    void insert(int val) {
        int x = 0;
        for (int i = (1 << 30); i; i >>= 1) {
            int a = bool(val & i);
            if (!trie[x][a])
                trie[x][a] = ++tot;
            x = trie[x][a];
        }
    }
    int findmaxxor(int val) {
        int ans = 0, x = 0;
        for (int i = (1 << 30); i; i >>= 1) {
            int a = bool(val & i);
            if (trie[x][!a]) {
                ans += i;
                x = trie[x][!a];
            } else
                x = trie[x][a];
        }
        return ans;
    }
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 1e5 + 5;
struct edge {
    int to, w;
};
int s[MAXN];
vector<edge> p[MAXN];
int n;
void addedge(int u, int v, int w) {
    p[u].push_back(edge({v, w}));
    p[v].push_back(edge({u, w}));
}
void dfs(int x, int fa) {
    for (int i = 0; i < p[x].size(); i++) {
        int nxt = p[x][i].to;
        if (nxt != fa) {
            s[nxt] = s[x] ^ p[x][i].w;
            dfs(nxt, x);
        }
    }
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        addedge(u, v, w);
    }
    dfs(1, -1);
    for (int i = 1; i <= n; i++)
        trie::insert(s[i]);
    int ans = 0;
    for (int i = 1; i <= n; i++)
        ans = max(ans, trie::findmaxxor(s[i]));
    cout << ans << '\n';
    return 0;
}
```