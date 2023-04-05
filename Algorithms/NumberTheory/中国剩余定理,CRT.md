模板题目：[P1495 【模板】中国剩余定理（CRT）/ 曹冲养猪](https://www.luogu.com.cn/problem/P1495)

题解：深进P306

板子：

```cpp
namespace CRT {
    LL c[MAXN], t[MAXN];
    LL CRT(int n, LL a[], LL b[]) { // x = a[i] (mod b[i])
        LL m = 1, ret = 0;
        for (int i = 1; i <= n; i++)
            m *= b[i];
        for (int i = 1; i <= n; i++) {
            c[i] = m / b[i];
            t[i] = inv(c[i], b[i]);
        }
        for (int i = 1; i <= n; i++)
            ret += a[i] * (t[i] < 0 ? t[i] + b[i] : t[i]) * c[i];
        ret %= m;
        return ret;
    }
}
```

使用：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
const int MAXN = 15;
void exgcd(LL a, LL b, LL &x, LL &y);
LL inv(LL a, LL p);

LL a[MAXN], b[MAXN];
int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++)
        cin >> a[i] >> b[i];
    cout << CRT::CRT(n, b, a);
    return 0;
}
```