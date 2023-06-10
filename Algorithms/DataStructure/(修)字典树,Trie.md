模板题目：[P8306 【模板】字典树](https://www.luogu.com.cn/problem/P8306)

板子：

```cpp
namespace trie {
    const int SIZ = 3e6 + 5;
    int size, C[SIZ][MAXM], S[SIZ];
    int to_int(char c) {
        if (isdigit(c)) return c - '0' + 1;
        else if (islower(c)) return c - 'a' + 10 + 1;
        else if (isupper(c)) return c - 'A' + 10 + 26 + 1;
        return 0;
    }
    void init() {
        for (int i = 0; i <= size; i++)
            S[i] = 0;
        for (int i = 0; i <= size; i++)
            for (int j = 1; j <= 26 + 26 + 10; j++)
                C[i][j] = 0;
        size = 0;
    }
    void insert(char X[]) {
        int p = 0;
        S[0]++;
        for (int i = 0; X[i]!= '\0'; i++) {
            int w = to_int(X[i]);
            if (C[p][w])
                p = C[p][w];
            else
                p = C[p][w] = ++size;
            S[p]++;
        }
    }
    int query(char X[]) {
        int p = 0;
        for (int i = 0; X[i]!= '\0'; i++) {
            int w = to_int(X[i]);
            if (C[p][w])
                p = C[p][w];
            else
                p = C[p][w] = ++size;
        }
        return S[p];
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
const int MAXL = 3e6 + 5;
const int MAXM = 26 + 26 + 10 + 5;
int n, q;

char A[MAXL];
int main() {
    int t;
    cin >> t;
    while (t--) {
        trie::init();
        cin >> n >> q;
        for (int i = 0; i < n; i++) {
            cin >> A;
            trie::insert(A);
        }
        while (q--) {
            cin >> A;
            cout << trie::query(A) << '\n';
        }
    }
    return 0;
}
```