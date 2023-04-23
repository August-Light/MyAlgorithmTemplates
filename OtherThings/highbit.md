板子：

```cpp
LL highbit(LL x) {
    if (x == 0)
        return 0;
    return 1ll << int(log2(x));
}
```