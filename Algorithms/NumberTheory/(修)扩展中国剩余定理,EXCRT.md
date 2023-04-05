模板题目：[P4777 【模板】扩展中国剩余定理（EXCRT）](https://www.luogu.com.cn/problem/P4777)

题解：[扩展中国剩余定理](https://www.luogu.com.cn/blog/blue/kuo-zhan-zhong-guo-sheng-yu-ding-li)

板子：

```cpp
i128 excrt() {
    i128 d, p1, p2, lambda1, lambda2;
    b[0] = mod(b[0], a[0]);
    for (int i = 0; i < n - 1; i++)
    {
        d = __gcd(a[i], a[i + 1]);
        p1 = a[i] / d;
        p2 = a[i + 1] / d;
        exgcd(p1, p2, lambda1, lambda2);
        b[i + 1] = b[i] + (b[i + 1] - b[i]) / d * lambda1 * a[i];
        a[i + 1] = lcm(a[i], a[i + 1]);
        b[i + 1] = mod(b[i + 1], a[i + 1]);
    }
    return b[n - 1];
}
```

使用：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
typedef __int128 i128;
i128 read();
void write(i128 x, char c = '\0');
const int MAXN = 1e5+5;
int n;
i128 a[MAXN],b[MAXN];
i128 mod(i128 a, i128 b);
i128 lcm(i128 a, i128 b);
void exgcd(i128 a, i128 b, i128 &x, i128 &y);
i128 excrt();
int main() {
    cin>>n;
    for (int i = 0; i < n; i++) {
        a[i]=read();
        b[i]=read();
    }
    write(excrt());
    return 0;
}
```