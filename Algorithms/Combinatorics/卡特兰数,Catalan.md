题目：[P1044 [NOIP2003 普及组] 栈](https://www.luogu.com.cn/problem/P1044)

题解：深进 P336

卡特兰数性质：

$$C_n=\sum\limits_{i=0}^{n-1}C_i\times C_{n-i-1}$$

$$C_n=C_{n-1}\times\dfrac{4n-2}{n+1}$$

$$C_n=\dfrac{\binom {2n} n}{n+1}$$

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;

LL Catalan[18+5];

int main() {
    int n = read();
    Catalan[0] = Catalan[1] = 1;
    for (int i = 2; i <= n; i++)
        Catalan[i] = Catalan[i - 1] * (4 * i - 2) / (i + 1);
    write(Catalan[n]);
    return 0;
}
```