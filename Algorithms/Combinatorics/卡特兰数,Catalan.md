题目：[P1044 [NOIP2003 普及组] 栈](https://www.luogu.com.cn/problem/P1044)

题解：深进P336

卡特兰数性质：

$$
C_n=\sum\limits_{i=0}^{n-1}C_i\times C_{n-i-1}
$$
$$
C_n=C_{n-1}\times\dfrac{4n-2}{n+1}
$$
$$
C_n=\dfrac{C_{2n}^n}{n+1}
$$

板子：

```cpp
int C[30];
void getCatalan(int n) {
    C[0] = 1;
    C[1] = 1;
    for (int i = 2; i <= n; i++)
        for (int j = 0; j < i; j++)
            C[i] += C[j] * C[i - j - 1];
}
```

使用：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
int C[30];
void getCatalan(int n);
int n;
int main() {
    cin>>n;
    getCatalan(n);
    cout<<C[n];
    return 0;
}
```