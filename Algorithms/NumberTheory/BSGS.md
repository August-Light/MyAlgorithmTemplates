模板题目：[P3846 [TJOI2007] 可爱的质数/【模板】BSGS](https://www.luogu.com.cn/problem/P3846)

题解：[算法学习笔记(34): 大步小步算法](https://zhuanlan.zhihu.com/p/132603308)

板子：

```cpp
LL BSGS(LL a, LL b, LL m) {
    umap<LL, LL> hs; hs.clear();
    LL cur = 1, t = sqrt(m) + 1; // 取 sqrt(m) 会 WA
    for (LL B = 1; B <= t; B++) {
        (cur *= a) %= m;
        hs[b * cur % m] = B;
    }
    // cur == pow(a, t)
    LL cur2 = cur;
    for (LL A = 1; A <= t; A++) {
        auto it = hs.find(cur2);
        if (it != hs.end())
            return A * t - it->second;
        (cur2 *= cur) %= m;
    }
    return -1;
}
```

使用：

```cpp
#include <bits/stdc++.h>
#define umap unordered_map
using namespace std;
typedef long long LL;

int main() {
    int a, b, m;
    cin >> m >> a >> b;
    LL ans = BSGS(a, b, m);
    if (ans == -1)
        puts("no solution");
    else
        cout << ans << endl;
    return 0;
}
```