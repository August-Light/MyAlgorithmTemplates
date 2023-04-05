模板题目：[P6136 【模板】普通平衡树（数据加强版）](https://www.luogu.com.cn/problem/P6136)

题解：[FHQ-Treap](https://www.luogu.com.cn/blog/178294/fhq-treap)

板子：

```cpp
namespace fhqTreap {
    int val[MAXN], wei[MAXN], siz[MAXN], son[MAXN][2];
    int tot, root;
    void pushup(int u) {
        siz[u] = siz[son[u][0]] + siz[son[u][1]] + 1;
    }
    pii split(int u, int k) { // u is the root
        if (!u)
            return make_pair(0, 0);
        if (val[u] < k) {
            pii t = split(son[u][1], k); // right
            son[u][1] = t.first;
            pushup(u);
            return make_pair(u, t.second);
        } else {
            pii t = split(son[u][0], k); // left
            son[u][0] = t.second;
            pushup(u);
            return make_pair(t.first, u);
        }
    }
    int merge(int u, int v) {
        if (!u || !v)
            return u + v;
        if (wei[u] < wei[v]) {
            son[u][1] = merge(son[u][1], v);
            pushup(u);
            return u;
        } else {
            son[v][0] = merge(u, son[v][0]);
            pushup(v);
            return v;
        }
    }
    void insert(int k) {
        val[++tot] = k;
        wei[tot] = rand();
        siz[tot] = 1;
        pii t = split(root, k);
        root = merge(merge(t.first, tot), t.second);
    }
    void del(int k) {
        pii x, y;
        x = split(root, k);
        y = split(x.second, k + 1);
        y.first = merge(son[y.first][0], son[y.first][1]);
        root = merge(x.first, merge(y.first, y.second));
    }
    int queryRank(int k) {
        int ret;
        pii t = split(root, k);
        ret = siz[t.first] + 1;
        root = merge(t.first, t.second);
        return ret;
    }
    int queryVal(int k) {
        int pos = root;
        while (pos) {
            if (k == siz[son[pos][0]] + 1)
                return val[pos];
            if (k <= siz[son[pos][0]])
                pos = son[pos][0];
            else {
                k -= siz[son[pos][0]] + 1;
                pos = son[pos][1];
            }
        }
        return 0;
    }
    int queryPrev(int k) {
        return queryVal(queryRank(k) - 1);
    }
    int queryNext(int k) {
        return queryVal(queryRank(k + 1));
    }
}
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef pair<int, int> pii;
int read() {
    int ret = 0, sgn = 0, ch = getchar();
    while (!isdigit(ch)) sgn |= ch == '-', ch = getchar();
    while (isdigit(ch)) ret = ret * 10 + ch - '0', ch = getchar();
    return sgn ? -ret : ret;
}
void write(int x) {
    if (x < 0) putchar('-'), x = -x;
    if (x > 9) write(x / 10);
    putchar(x % 10 + '0');
}
const int MAXN = 1e6 + 1e5 + 5;

int n, m, last, ans;
int main() {
    srand(time(0));
    n = read(); m = read();
    for (int i = 1; i <= n; i++)
        fhqTreap::insert(read());
    while (m--) {
        int opt = read();
        int x = read() ^ last;
        if (opt == 1)
            fhqTreap::insert(x);
        else if (opt == 2)
            fhqTreap::del(x);
        else if (opt == 3)
            last = fhqTreap::queryRank(x), ans ^= last;
        else if (opt == 4)
            last = fhqTreap::queryVal(x), ans ^= last;
        else if (opt == 5)
            last = fhqTreap::queryPrev(x), ans ^= last;
        else if (opt == 6)
            last = fhqTreap::queryNext(x), ans ^= last;
    }
    write(ans);
    return 0;
}
```