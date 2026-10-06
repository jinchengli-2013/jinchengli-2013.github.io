1. 已知函数 $f(x)=\ln x-\frac{1}{2}ax^2+x(a\in \R)$ 

   (1). 当 $a=0$ 时, 求曲线 $y=f(x)$ 在点 $(1, f(1))$ 处的切线方程

   (2). 令 $g(x)=f(x)-ax+1$ 求函数 $g(x)$ 的极值.

**【解答】**

(1).

当 $a=0$ 时， $f(x)=\ln x+x$

那么 
$$
f'(x)=\frac{1}{x}+1
$$
令 $x=1$, 得到切线方程的斜率 $k$.
$$
k=f'(1)=2
$$
切点为
$$
(1, \ln 1+1)
$$
就是
$$
(1, 1)
$$
使用直线方程的点斜式， 得到
$$
(y-1)=2(x-1) \Leftrightarrow y-1=2x-2 \Leftrightarrow y=2x-1
$$
答案就是
$$
\boxed{y=2x-1}
$$
(2).

求出 $g(x)$ 的表达式
$$
\because g(x)=f(x)-ax+1 \\
\therefore g(x)=\ln x-\frac{1}{2}ax^2+x-ax+1=-\frac{1}{2}ax^2+(1-a)x+1+\ln x \\
$$
求出 $g(x)$ 的定义域:

$\ln x$ 的定义域为 $x>0$, 那么 $g(x)$ 的定义域就为 $x>0$.

直接求导
$$
g'(x)=-ax+1-a+\frac{1}{x}
$$
 令 $g'(x)=0$
$$
-ax+1-a+\frac{1}{x}=0 \\
-ax^2+x-ax+1=0 \\
ax^2-x+ax-1=0 \\
ax^2+(a-1)x-1=0 \\
(ax-1)(x+1)=0 \\
x_1=\frac{1}{a}, x_2=-1(不符合定义域, 舍去) \\
$$
得到驻点
$$
x=\frac{1}{a}(a>0)
$$
分类讨论:

当 $a\leq0$ 时，不存在驻点，所以函数没有极值.

当 $a>0$ 时， 驻点为 $x=\frac{1}{a}$, 极值为
$$
\boxed{f(\frac{1}{a})=\frac{1}{2a}-\ln a}
$$


 作者: 李金城

主页: [jinchengli-2013](https://github.com/jinchengli-2013)

mail: [传送门](jinchengli_2013@163.com)





