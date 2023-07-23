# 利用Taichi加速Python
???+ note "引言"
    Taichi 是一个嵌入在 Python 中的领域特定语言（DSL）。 Taichi 的主要功能之一是加速计算密集的 Python 程序，帮助这些程序 实现可以媲美 C/C++ 甚至 CUDA 的性能。 这使得 Taichi 在科学计算领域处于更有利的地位。

如何体现Taichi的优越性呢。我们下面用python设计一个求解素数个数的程序，

```python
import time
ti.init(arch = ti.gpu)

def is_prime(n:int):
    result =  True
    for i in range(2,int(n**0.5)+1):
        if n % i == 0:
            result = False
            break
    return result

def cout(n:int)->int:
    s = 0
    for i in range(2,n):
        if is_prime(i):
            s+=1
    return s

if __name__ == "__main__":
    a = time.time()
    print(cout(10000000))
    b = time.time()
    print(f"{b-a}秒")

```
???+ note "运行结果"
    ```cpp
    664579
    65.65770673751831秒
    ```

接下来我们在代码中引入Taichi来加速
首先导入Taichi并初始化
```python
import taichi as ti
ti.init(arch = ti.gpu)
```
然后在我们在`is_prime()`和`cout()`函数前面分别加上`@ti.func`,`@ti.kernel`来进行修饰，这段代码的含义也就是指定在gpu中运行，从而极大提高效率。
>Taichi 的编译器将 @ti.kernel 装饰的 Python 代码编译到不同的设备上，例如 CPU 和 GPU，以实现高性能计算。--来源于Taichi官网

```python
import taichi as ti
import time
ti.init(arch = ti.gpu)
@ti.func
def is_prime(n:int):
    result =  True
    for i in range(2,int(n**0.5)+1):
        if n % i == 0:
            result = False
            break
    return result
@ti.kernel
def cout(n:int)->int:
    s = 0
    for i in range(2,n):
        if is_prime(i):
            s+=1
    return s

if __name__ == "__main__":
    a = time.time()
    print(cout(10000000))
    b = time.time()
    print(f"{b-a}秒")
```
???+ note "运行结果"
    ```bash
    [Taichi] version 1.1.3, llvm 10.0.0, commit 1262a70a, win, python 3.10.7
    [Taichi] Starting on arch=cuda
    664579
    0.26840996742248535秒
    ```

可以发现，在通过taichi的加速后，计算10000000之内素数的时间从65秒缩短到了0.2秒，效率提高了三百多倍，而且当数据量更大的话，效率可以继续提高.