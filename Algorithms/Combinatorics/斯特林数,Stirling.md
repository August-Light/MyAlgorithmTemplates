## 第二类斯特林数

题目：[P1287 盒子与球](https://www.luogu.com.cn/problem/P1287)

题解：深进P338

斯特林数性质：

$$
S(n,r)=\dfrac{1}{r!}\sum\limits_{k=0}^{r-1}(-1)^kC^k_r(r-k)^n
$$

板子：

```cpp
LL S[20][20];
void getStirling(int n) {
    S[1][1] = 1;
    for (int i = 2; i <= n; i++)
        for (int j = 1; j <= i; j++)
            S[i][j] = S[i - 1][j - 1] + j * S[i - 1][j];
}
```

使用：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
typedef unsigned long long uLL;
LL S[20][20];
void getStirling(int n);
LL n, r, ans = 1;
int main() {
    cin>>n>>r;
    getStirling(n);
    for (int i = 1; i <= r; i++)
        ans*=i;
    ans*=S[n][r];
    cout<<ans;
    return 0;
}
```