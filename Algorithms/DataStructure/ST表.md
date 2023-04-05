模板题目：[P3865 【模板】ST 表](https://www.luogu.com.cn/problem/P3865)

板子：

```cpp
struct ST {
    using func_type = function<int(const int &, const int &)>;
    int f[MAXT][MAXN];
    void init(int a[], int n, func_type op) {
        for (int i = 1; i <= n; i++)
            f[0][i] = a[i];
        for (int i = 1; (1 << i) <= n; i++)
            for (int p = 1; p <= n; p++) {
                int tmp = p + (1 << (i - 1));
                if (tmp + (1 << (i - 1)) - 1 > n)
                    break;
                f[i][p] = op(f[i - 1][p], f[i - 1][tmp]);
            }
    }
    int query(int l, int r, func_type op) {
        int len = r - l + 1, log2len = log2(len);
        return op(f[log2len][l], f[log2len][r - (1 << log2len) + 1]);
    }
};
```

使用：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
int read();
void write(int x);
const int MAXN = 1e5 + 5;
const int MAXT = 25;

ST st;
int a[MAXN];
int n, m, l, r;
int Max(int a, int b) {
    return max(a, b);
}
int main() {
    n = read(); m = read();
    for (int i = 1; i <= n; i++)
        a[i] = read();
    st.init(a, n, Max);
    while (m--) {
        l = read(); r = read();
        write(st.query(l, r, Max)); putchar('\n');
    }
    return 0;
}
```