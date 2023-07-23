# 傅里叶级数证明巴塞尔问题



设有函数$f(x)=x$，其定义域为$x \in (-\pi,\pi)$。这个函数的傅里叶级数是:

$$
f(x) = \sum_{n=1}^{\infty}\frac{2(-1)^{n+1}}{n} \sin(nx)
$$


根据帕塞瓦尔恒等式，我们有：

$$
{\pi^2 \over 3}  = {1 \over 2\pi} \int_{-\pi}^{\pi} f^2(x) \, dx = \sum_{n=1}^{\infty}{1 \over 2\pi} \int_{-\pi}^{\pi} (2 \frac{(-1)^{n+1}}{n}  \sin(nt) )^2 dt = 2 \sum_{n=1}^{\infty} {1 \over n^2}
$$

因此
:$${\pi^2 \over 6}  =  \sum_{n=1}^{\infty} {1 \over n^2}$$

证毕。
