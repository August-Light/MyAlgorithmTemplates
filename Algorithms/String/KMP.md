模板题目：[P3375 【模板】KMP字符串匹配](https://www.luogu.com.cn/problem/P3375)

题解：[最浅显易懂的 KMP 算法讲解](https://www.bilibili.com/video/BV1AY4y157yL)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned int uint;


const int MAXN = 1e6 + 100;

int n, m;
char S[MAXN], T[MAXN];
int nxt[MAXN]; // border 长度

int main() {
    scanf("%s%s", S+1, T+1);
    n = strlen(S+1); m = strlen(T+1);

    for (int i = 2; i <= m; i++) {
        int p = nxt[i-1];
        while (p != 0 && T[p + 1] != T[i])
            p = nxt[p];
        if (T[p + 1] == T[i])
            nxt[i] = p + 1;
    }

    int p = 0;
    for (int i = 1; i <= n; i++) {
        if (T[p + 1] == S[i])
            p++;
        else {
            while (p != 0 && T[p + 1] != S[i])
                p = nxt[p];
            if (T[p + 1] == S[i])
                p++;
        }
        if (p == m)
            write(i-m+1, '\n');
    }

    for (int i = 1; i <= m; i++)
        write(nxt[i], ' ');
    return 0;
}
```