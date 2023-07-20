题目：[P4551 最长异或路径](https://www.luogu.com.cn/problem/P4551)

题解：深基P109

板子：

```cpp
struct Trie {
    int tot, trie[MAXN * LOGW][2];
    void insert(int val) {
        int x = 0;
        for (int i = (1 << (LOGW-1)); i; i >>= 1) {
            int a = bool(val & i);
            if (!trie[x][a])
                trie[x][a] = ++tot;
            x = trie[x][a];
        }
    }
    int find_max_xor(int val) {
        int ans = 0, x = 0;
        for (int i = (1 << (LOGW-1)); i; i >>= 1) {
            int a = bool(val & i);
            if (trie[x][!a]) {
                ans += i;
                x = trie[x][!a];
            } else
                x = trie[x][a];
        }
        return ans;
    }
};
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

const int MAXN = 1e5 + 100;
const int MAXM = MAXN << 1;



const int LOGW = 31;



Trie tr;

int s[MAXN];
void dfs(int u, int lst) {
    for (int i = head[u]; i; i = e[i].nxt) {
        if (i == (lst ^ 1))
            continue;
        int v = e[i].to;
        s[v] = s[u] ^ e[i].val;
        dfs(v, i);
    }
}

int main() {
    int n = read();
    for (int i = 1; i <= n-1; i++) {
        int u = read(), v = read(), w = read();
        addedge(u, v, w);
        addedge(v, u, w);
    }
    dfs(1, -1);
    for (int i = 1; i <= n; i++)
        tr.insert(s[i]);
    int ans = 0;
    for (int i = 1; i <= n; i++)
        ans = max(ans, tr.find_max_xor(s[i]));
    write(ans);
    return 0;
}
```