板子：

```cpp
int lcm(int a, int b) {
    return a/__gcd(a, b)*b;
}
```