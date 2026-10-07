## 图形学与坐标变换

本文采用**左手坐标系**($\text{Left-handed coordinate system}$)

就是原点看向 $Z+$ 坐标轴

### 基本数学公式

设 $A$ 为 $m \times s$ 矩阵,$B$ 为 $s\times n$ 矩阵: 
$$
\begin{aligned}
A&=\begin{bmatrix}
a_{1,1}&a_{1,2}&a_{1,3}&...&a_{1,s} \\
a_{2,1}&a_{2,2}&a_{2,3}&...&a_{2,s} \\
...	\\
a_{m,1}&a_{m,2}&a_{m,3}&...&a_{m,s}
\end{bmatrix} \\
B&=\begin{bmatrix}
b_{1,1}&b_{1,2}&b_{1,3}&...&b_{1,n} \\
b_{2,1}&b_{2,2}&b_{2,3}&...&b_{2,n} \\
...	\\
b_{s,1}&b_{s,2}&b_{s,3}&...&b_{s,n}
\end{bmatrix}
\end{aligned}
$$


那么 $C=A \times B$ 为 $m\times n$ 矩阵,  其中
$$
c_{i,j}=\sum_{k=1}^{s}a_{i,k}b_{k,j}
$$
特殊情况: 图形数学中常用的**行向量**乘上**转移矩阵**.

本文仅使用 $4\times 4$ 的其次坐标, 那么矩阵乘法公式为
$$
\begin{aligned}
A&=
\begin{bmatrix}
p_1&p_2&p_3&p_4
\end{bmatrix} \\
B&=
\begin{bmatrix}
q_{1, 1}&q_{1, 2}&q_{1, 3}&q_{1,4} \\
q_{2, 1}&q_{2, 2}&q_{2,3}&q_{2,4} \\
q_{3, 1}&q_{3, 2}&q_{3, 3}&q_{3, 4} \\
q_{4, 1}&q_{4, 2}&q_{4, 3}&q_{4, 4}
\end{bmatrix} \\
C&=A\times B \\
&=
\begin{bmatrix}
p_1q_{1,1}+p_2q_{1,2}+p_3q_{1,3}+p_4q_{1,4}&p_1q_{2,1}+p_2q_{2,2}+p_3q_{2,3}+p_4q_{2,4}&
p_1q_{3,1}+p_2q_{3,2}+p_3q_{3,3}+p_4q_{3,4}&p_1q_{4,1}+p_2q_{4,2}+p_3q_{4,3}+p_4q_{4,4}
\end{bmatrix}
\end{aligned}
$$


简单来说, 就是**每一个数乘上每一行,然后求和**

---

对于三角形 $\triangle ABC$ , $A(x_1,y_1,z_1), B(x_2,y_2,z_2), C(x_3,y_3,z_3)$

三角形的重心为
$$
\begin{aligned}
G_x&=\frac{x_1+x_2+x_3}{3} \\
G_y&=\frac{y_1+y_2+y_3}{3} \\
G_z&=\frac{z_1+z_2+z_3}{3}
\end{aligned}
$$

---

### 世界坐标

设三角形 $\triangle ABC$ 的三个顶点为 $A(x_1, y_1, z_1),B(x_2,y_2,z_2),C(x_3,y_3,z_3)$ 这些坐标为**世界坐标**

我们设摄像机的坐标为 $C(x_c, y_c,z_c)$

 我们需要将世界坐标的原点平移到摄像机的坐标,也就是将摄像机的坐标当成新的原点.

根据**坐标轴平移公式**($\text{Coordinate axis translation formula}$), 点 $P(x_1, y_1, z_1)$ 以点 $Q(x, y, z)$ 为新原点的新坐标是
$$
\begin{cases}
x_{\text{new}}=x_1-x \\
y_{\text{new}}=y_1-y \\
z_{\text{new}}=z_1-z
\end{cases}
$$
现在我们需要使用矩阵表示, 通过矩阵乘法计算出新的坐标.

这里我们需要引入**齐次坐标**($\text{Homogeneous coordinates}$), 将 $A, B, C$ 三点的坐标改为
$$
A(x_1, y_1, z_1, 1) \\
B(x_2, y_2, z_2, 1) \\
C(x_3, y_3, z_3, 1)
$$
现在我们来推导**转移矩阵**($\text{Transition matrix}$)

我们现在要推导一个 $4\times 4$ 的矩阵 $V_{\text{world}}$ 使得
$$
[x_1, y_1,z_1,1]\times V_{\text{world}}=[x_1-x,y_1-y,z_1-z,1]
$$
 那么很显然, 我们只需要将每一行的 $x_1, y_1,z_1$ 所对应位置设置为 $1$, 第 $4$ 个位置设置成 $-x, -y, -z$ , 其他位置设置为 $0$ 即可

就是
$$
V_{\text{world}}=
\begin{bmatrix}
1&0&0&-x \\
0&1&0&-y \\
0&0&1&-z \\
0&0&0&1
\end{bmatrix}
$$

---

### 投影矩阵

我们需要进行一次坐标变换,将以摄像机为原点的坐标转换成一个立方体.

我们知道,人所看到的东西其实是一个**棱台**.

**太近的东西, 比如只距离你一个毫米的东西,你看不到,那么所能看到的最近的平面叫做==近裁剪面==**

近裁剪面到眼睛的距离使用 $n$ 来表示

**太远的东西,比如数十亿千米的东西你也看不到,那么同理,最远的平面叫做==远裁剪面==**.

远裁剪面到眼睛的距离使用 $f$ 来表示

**因为要显示到屏幕上面,就有==宽高比==**.

常见的宽高比为 $\text{aspect}=\frac{16}{9}$ .

**你的脖子不可以抬高看到你的背后,同时也不能低头看到背后,那么你的眼睛上下移动就有一个区间, 你的眼睛可以看到的度数区间,叫做 $\text{FOV}$ 角**

比如 $\mathcal{MineCraft}$ 中 $\text{Fov}=90^\degree$ 代表玩家可以向上 $45$ 向下 $45$.

#### 关键参数推导

在深度 $z$ 处

平面的半高为
$$
h(z)=z\times\tan(\text{aspect})
$$
 半宽为
$$
w(z)=z\times \tan(\text{aspect})
$$


---

#### 数学推导部分

我们考虑推导一个矩阵 $P$ 使得

$$
(x_1,y_1,z_1,1)\times P=(x_c,y_c,z_c,w_c)
$$
其中
$$
(x_c,y_c,z_c, w_c)
$$
为变换后的坐标.

然后我们将其进行**透视除法**.
$$
\Big(\frac{x_c}{w_c},\frac{y_c}{w_c},\frac{z_c}{w_c},1\Big)
$$
其中下面这个坐标是一个标准**立方体**.

而这个矩阵 $P$ 就是将空间的**视椎体(前文所说的==棱台==)**,映射成一个立方体.

**原来在棱台外面的点在正方体外面,在棱台里面的点在正方体的里面**, 是最重要的==性质==

但是透视除法之后的坐标满足一定的范围
$$
\frac{x_c}{w_c},\frac{y_c}{w_c}\in[-1,1] \\
\frac{z_c}{w_c}\in[0,1]
$$
矩阵的形式应该是这样子的
$$
P=
\begin{bmatrix}
a&0&0&0 \\
0&b&0&0 \\
0&0&c&d \\
0&0&1&0
\end{bmatrix}
$$

- $x_c$ 只和 $x_1$ 有关
- $y_c$ 只和 $y_1$ 有关
- $z_c$ 只和 $z_1$ 以及常数项有关
- $w_c=z_1$,  因为要使得近大远小,除以更大的 $w_c$

对点 $(x_1,y_1,z_1,1)$ 使用矩阵
$$
x_c=ax_1 \\
y_c=by_1 \\
z_c=cz_1+d \\
w_c=z_1
$$
透视除法后
$$
\Big(\frac{ax_1}{z_1},\frac{by_1}{z_1},\frac{cz_1+d}{z_1},1\Big)
$$
在深度 $z_1$ 处,右边界的 $x=w(z_1)=z_1\tan(\text{Fov}/2)\times\text{aspect}$ 应该映射到 $1$
$$
\frac{a\times z_1\tan(\text{Fov}/2)\times\text{aspect}}{z_1}=1 \\
a\tan(\text{Fov}/2)\times\text{aspect}=1 \\
\boxed{a=\frac{1}{\tan(\text{Fov}/2)\times\text{aspect}}}
$$
同理, 可以推得
$$
\frac{bz_1\tan(\text{Fov}/2)}{z_1}=1 \\
\boxed{b=\frac{1}{\tan(\text{Fov}/2)}}
$$
我们要求

$z=n$ 时, $\frac{cz+d}{z}=0$ 

$z=f$ 时, $\frac{cz+d}{z}=1$
$$
\frac{cz+d}{n}=c+\frac{d}{n}=0 \Rightarrow c=-\frac{d}{n}
$$

$$
c+\frac{d}{f}=1\Rightarrow -\frac{d}{n}+\frac{d}{f}=1 \\
\Rightarrow d\Big(\frac{1}{f}-\frac{1}{n}\Big) =1 \\
\Rightarrow d=-\frac{nf}{f-n}
$$

带回, 得
$$
c=\frac{f}{f-n}
$$
所以结论为
$$
\boxed{d=-\frac{nf}{f-n}},\boxed{c=\frac{f}{f-n}}
$$
带回 $a,b,c,d$ 
$$
\boxed{P=
\begin{bmatrix}
\frac{1}{\tan(\text{Fov}/2)\times\text{aspect}}&0&0&0 \\
0&\frac{1}{\tan(\text{Fov}/2)}&0&0 \\
0&0&\frac{f}{f-n}&d=-\frac{nf}{f-n} \\
0&0&1&0
\end{bmatrix}}
$$


