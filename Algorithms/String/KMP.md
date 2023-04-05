模板题目：[P3375 【模板】KMP字符串匹配](https://www.luogu.com.cn/problem/P3375)

题解：[最浅显易懂的 KMP 算法讲解](https://www.bilibili.com/video/BV1AY4y157yL)

板子：

```cpp
namespace KMP {
    int n, m;
    int pi[MAXN];
    vector<int> v;
    void build_pi(char T[]) {
        for (int i = 2; i <= m; i++) {
            int p = pi[i - 1];
            while (p != 0 && T[p + 1] != T[i])
                p = pi[p];
            if (T[p + 1] == T[i])
                pi[i] = p + 1;
        }
    }
    void KMP(char S[], char T[]) {
        n = strlen(S + 1);
        m = strlen(T + 1);
        build_pi(T);
        int p = 0;
        for (int i = 1; i <= n; i++) {
            if (T[p + 1] == S[i])
                p++;
            else {
                while (p != 0 && T[p + 1] != S[i])
                    p = pi[p];
                if (T[p + 1] == S[i])
                    p++;
            }
            if (p == m)
                v.push_back(i - m + 1);
        }
    }
}
```

使用：

```cpp
#include <bits/stdc++.h>
#define endl '\n'
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 1e6 + 100;

int n, m;
char S[MAXN], T[MAXN];
int main() {
    scanf("%s%s", S + 1, T + 1);
    n = strlen(S + 1); m = strlen(T + 1);
    KMP::KMP(S, T);
    for (int i : KMP::v)
        printf("%d\n", i);
    for (int i = 1; i <= m; i++)
        printf("%d%c", KMP::pi[i], " \n"[i == n]);
    return 0;
}
```