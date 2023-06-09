来源：深基P205

```cpp
#include<vector>
```

```cpp
vector<int> v(N,i);
```

建立一个可变长度数组 `v`，内部元素类型为 `int`；
该可变数组最开始有 `N` 个元素，每个元素初始化为 `i`。

可以省略 `i`(默认值为 `0`)，也可以把 `(N,i)` 同时省略，此时这个数组的长度就是 `0`。
内部元素类型可以换成其他的类型，如 `double`。

```cpp
v.push_back(a);
```

将元素 `a` 插入到数组 `v` 的末尾，并增加数组长度。

```cpp
v.size();
```

返回数组 `v` 的长度。

```cpp
v.resize(n,m);
```

重新调整数组大小为 `n`，

如果 `n` 比原来的小，则删除多余的信息；

如果 `n` 比原来的大，则新增的部分都初始化为 `m` ，其中 `m` 是可以省略的。