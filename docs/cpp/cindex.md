# c++

## 读入，输出优化
在c++中，我们可以使用 `cin` 来进行输入，这比scanf()要容易不少，但是cin的缺点是输入效率太低，在算法竞赛中，处理大规模的数据，例如输入字母矩阵，用cin可能会导致`TLE`错误。
这时候可以使用以下代码进行优化：
>在默认的情况下 `std::cin` 绑定的是 `std::cout`，每次执行 `<<` 操作符的时候都要调用 `flush()` 来清理 `stream buffer`，这样会增加 IO 负担。可以通过 `std::cin.tie(0)`（0 表示 NULL）来解除 `std::cin` 与 `std::cout` 的绑定，进一步加快执行效率。

```cpp
std::ios::sync_with_stdio(false);
std::cin.tie(0);
// 如果编译开启了 C++11 或更高版本，建议使用 std::cin.tie(nullptr);
```

## `char` or `string`
`char` 和`string` 均可以用来存放字符串，但是相比之下，我更喜欢用string，在c中，char的效率通常会高一点，但是具体使用的时候，string更加易于使用与高效。
`string`拥有不少实用的方法
```cpp
str.append() //末尾增加
str.length() //获取str的长度
str.empty() //判断字符串是否为空
```
与`char`相比,`string`更像是一个动态的`char`数组


## 万能头文件
在c++中如果不太记得代码中使用的来源于那个库，可以直接使用下面的头文件。
```cpp
#include <bits/stdc++.h>
```
??? info "头文件内容"
    ```cpp   
    #ifndef _GLIBCXX_NO_ASSERT
    #include <cassert>
    #endif
    #include <cctype>
    #include <cerrno>
    #include <cfloat>
    #include <ciso646>
    #include <climits>
    #include <clocale>
    #include <cmath>
    #include <csetjmp>
    #include <csignal>
    #include <cstdarg>
    #include <cstddef>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <ctime>
    
    #if __cplusplus >= 201103L
    #include <ccomplex>
    #include <cfenv>
    #include <cinttypes>
    #include <cstdalign>
    #include <cstdbool>
    #include <cstdint>
    #include <ctgmath>
    #include <cwchar>
    #include <cwctype>
    #endif
    
    // C++
    #include <algorithm>
    #include <bitset>
    #include <complex>
    #include <deque>
    #include <exception>
    #include <fstream>
    #include <functional>
    #include <iomanip>
    #include <ios>
    #include <iosfwd>
    #include <iostream>
    #include <istream>
    #include <iterator>
    #include <limits>
    #include <list>
    #include <locale>
    #include <map>
    #include <memory>
    #include <new>
    #include <numeric>
    #include <ostream>
    #include <queue>
    #include <set>
    #include <sstream>
    #include <stack>
    #include <stdexcept>
    #include <streambuf>
    #include <string>
    #include <typeinfo>
    #include <utility>
    #include <valarray>
    #include <vector>
    
    #if __cplusplus >= 201103L
    #include <array>
    #include <atomic>
    #include <chrono>
    #include <condition_variable>
    #include <forward_list>
    #include <future>
    #include <initializer_list>
    #include <mutex>
    #include <random>
    #include <ratio>
    #include <regex>
    #include <scoped_allocator>
    #include <system_error>
    #include <thread>
    #include <tuple>
    #include <typeindex>
    #include <type_traits>
    #include <unordered_map>
    #include <unordered_set>
    #endif

    ```
???+ info "注意"
    万能头文件的缺点是包含的库太多，有时候没有用到的库在执行时也会被加载，这会导致代码运行效率低，占用内存大，因此在做算法题时候 可能会导[`TLE`]错误和超内存
