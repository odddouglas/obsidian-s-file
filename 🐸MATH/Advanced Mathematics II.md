# 空间向量  

#### 向量叉乘
*向量的叉乘，也称为向量积或叉积，是一种用来计算两个向量之间产生的新向量的运算。它只适用于三维向量。*

- 向量的叉乘的结果是一个垂直于原始两个向量的新向量。该新向量的大小表示原始两个向量所围成的平行四边形的面积，而方向则遵循右手法则。
- 假设有两个向量 $\mathbf{A} = \begin{bmatrix} a_1 \ a_2 \ a_3 \end{bmatrix}$ 和 $\mathbf{B} = \begin{bmatrix} b_1 \ b_2 \ b_3 \end{bmatrix}$，它们的叉乘结果记作 $\mathbf{A} \times \mathbf{B}$。则叉乘的计算公式如下：

$$
\mathbf{A} \times \mathbf{B} = \begin{bmatrix} a_2b_3 - a_3b_2 \\ a_3b_1 - a_1b_3 \\ a_1b_2 - a_2b_1 \end{bmatrix}
$$
$$\mathbf{A} \times \mathbf{B} = |\mathbf{A}| |\mathbf{B}| \sin(\theta) \mathbf{n}$$

- 其中 $|\mathbf{A}|$ 和 $|\mathbf{B}|$ 分别表示向量 $\mathbf{A}$ 和 $\mathbf{B}$ 的模长（长度），$\theta$ 是 $\mathbf{A}$ 和 $\mathbf{B}$ 之间的夹角，$\mathbf{n}$ 是一个单位向量，垂直于 $\mathbf{A}$ 和 $\mathbf{B}$ 所在的平面。

- 叉乘运算的结果是一个新的向量，它既具有大小也具有方向。向量的大小可以通过计算其模长来得到。向量的方向可以使用右手法则来确定：将右手的拇指指向 $\mathbf{A}$，食指指向 $\mathbf{B}$，则中指的方向就是叉乘结果的方向。
	需要注意的是，向量的叉乘是一个向量运算，结果是一个向量，而不是一个标量。叉乘只适用于三维向量，对于二维向量或其他维度的向量，不存在叉乘运算。


#### 共面
$$(\vec a×\vec b)·\vec c=0\quad 其实就是\vec a ,\vec b向量的法向量与\vec c平行$$

#### 平面

一个平面可以通过以下方法来确定：

1.  通过三个不共线的点确定平面：如果给定平面上的三个不共线的点，可以使用这三个点来确定一个平面。通过这三个点，可以构建两个向量，然后取它们的叉乘得到平面的法向量。最后，使用其中一个点和法向量即可表示平面的方程。
    
2.  通过点和法向量确定平面：如果给定平面上的一个点和平面的法向量，可以使用这个点和法向量来确定一个平面。使用给定的点作为平面上的一个点，然后使用法向量作为平面的法线方向，从而确定平面的方程。
    
3.  通过点和平行于已知平面的向量确定平面：如果给定平面上的一个点和与已知平面平行的向量，可以使用这个点和向量来确定一个平面。使用给定的点作为平面上的一个点，然后使用平行于已知平面的向量作为平面的法线方向，从而确定平面的方程。
    

这些方法可以用于确定三维空间中的平面。无论使用哪种方法，确定平面的关键是找到平面上的一个点和与平面相关的向量（如法向量或平行向量），然后利用这些信息来构建平面的方程。

$$
Ax + By + Cz + D = 0$$
	其中，A、B、C 是平面的法向量的分量, (x,y,z) 是平面上的任意一点的坐标，D 是平面的常数项。起到平移平面的作用。具体来说，D 决定了平面与原点之间的距离。

> 求解平面方程至少需要3个有关A B C D 的关系式联立, 一般利用平行(叉乘为0)或者垂直(点乘为0)的关系来进行运算

#### 曲线的切线方程,法平面方程
$$(切线方程):\frac{x-x\left(t_0\right)}{x^{\prime}\left(t_0\right)}=\frac{y-y\left(t_0\right)}{y^{\prime}\left(t_0\right)}=\frac{z-z\left(t_0\right)}{z^{\prime}\left(t_0\right)}$$
$$
(法平面方程):x^{\prime}\left(t_0\right)\left[x-x\left(t_0\right)\right]+y^{\prime}\left(t_0\right)\left[y-y\left(t_0\right)\right]+z^{\prime}\left(t_0\right)\left[z-z\left(t_0\right)\right]=0
$$


#### 曲面的法向量,切平面方程,法线方程

$$
(法向量):\left(\mathbf{F}_{x}^{\prime}(\mathbf{x}_{0},\mathbf{y}_{0},\mathbf{z}_{0}),\mathbf{F}^{\prime}_y(\mathbf{x}_{0},\mathbf{y}_{0},\mathbf{z}_{0}),\mathbf{F}^{\prime}_z(\mathbf{x}_{0},\mathbf{y}_{0},\mathbf{z}_{0})\right) 
$$
$$
(切平面方程):\begin{array}{l}{{{\bf F}_{~x}^{\prime}({\bf x}_{0},{\bf y}_{0},{\bf z}_{0})({\bf x}-{\bf x}_{0})+{\bf F}_{~y}^{\prime}({\bf x}_{0},{\bf y}_{0},{\bf z}_{0})({\bf y}-{\bf y}_{0})+}} {{{\bf F}_{~z}^{\prime}({\bf x}_{0},{\bf y}_{0},{\bf z}_{0})({\bf z}-{\bf z}_{0})=0}}\end{array} 
$$

$$
(法线方程):\frac{x-x_{0}}{\mathbf{F}^{\prime}_x(x_{0},y_{0},z_{0})}=\frac{y-y_{0}}{\mathbf{F}^{\prime}_y(x_{0},y_0,z_0)}=\frac{z-z_0}{\mathbf{F}^{\prime}_z(x_{0},y_{0},z_{0})} 
$$
#### 投影柱面
  
*投影柱面是指在三维空间中，通过将一个二维平面沿着一条直线进行无限延伸形成的曲面。这条直线称为柱面的轴线，而平面则称为柱面的母平面。投影柱面可以通过将母平面上的点与轴线之间的连线在空间中旋转而得到*

# 偏导(partial derivative)
*当处理多元函数时，偏导数用于衡量函数对于其中一个自变量的变化率，而其他自变量保持不变。偏导数用于衡量多元函数对其中一个自变量的变化率。可以帮助我们理解函数在特定点附近的局部行为。*

- 考虑一个具有多个自变量的函数，例如，$f(x, y)$，其中 $x$ 和 $y$ 是自变量。当我们计算函数 $f$ 关于其中一个自变量的变化率时，偏导数派上了用场。具体来说，如果我们计算 $f$ 关于 $x$ 的偏导数，记作 $\frac{\partial f}{\partial x}$，则我们将 $y$ 视为常数，仅考虑 $x$ 的变化对 $f$ 的影响。

- 数学上，偏导数的定义如下：

$$\frac{\partial f}{\partial x}=\lim _{h \rightarrow 0} \frac{f(x+h, y)-f(x, y)}{h}$$
	这个定义表示了当自变量 $x$ 增加一个无穷小的量 $h$ 时，函数 $f$ 相应地发生的变化。我们计算差商的极限来获得这个变化率。类似地，我们也可以计算 $f$ 关于 $y$ 的偏导数 $\frac{\partial f}{\partial y}$，在这种情况下，我们将 $x$ 视为常数。

- 需要注意的是，偏导数的计算是基于函数在特定点上的局部性质，它描述了函数在给定点附近的变化率。<u>对于不同的点，偏导数的值可能不同，因此可以将偏导数看作是函数在不同点的局部变化率。
</u>


#### 【 一阶偏导 】

> $已知W=f(x,y,z)=x^3y^2+3xy+3xz$, 求$\frac {\partial w} {\partial x}$,$\frac {\partial w} {\partial y}$,$\frac {\partial w} {\partial z}$/ 求$W$在$(1,2,3)$点的偏导

1. 求总函数Z对x的偏导,就仅仅把x当作未知数,其余未知数当作常数(如y,z),然后对x求导得
$$\frac {\partial w} {\partial x}=3x^2y^2+3y+3z$$
2. 如果是给定点,则将定点的值代入求导结果中即得偏导
$$\frac {\partial w} {\partial x}|_{x=1,y=2,z=3}=3×1^2×2^2+3×2+3×3=27$$


#### 【 二阶偏导 】

> 已知$Z=f(x,y)=x^3y^2+3xy$ ,求$\frac {\partial^2 z} {\partial x^2}$ ,$\frac {\partial^2 z} {\partial x \partial y}$,$\frac {\partial^2 z} {\partial y \partial x}$,$\frac {\partial^2 z} {\partial y^2}$

1. $$\frac {\partial^2 z} {\partial x^2}=\frac {\partial(\frac{\partial z}{\partial x})} {\partial x}(就是z先对x求偏导的结果再对x求一次偏导)$$$$\frac {\partial^2 z} {\partial y^2}=\frac {\partial(\frac{\partial z}{\partial y})} {\partial y}(就是z先对y求偏导的结果再对y求一次偏导)$$
2. $$\frac {\partial^2 z} {\partial x \partial y}=\frac {\partial(\frac{\partial z}{\partial x})} {\partial y}\quad=\quad\frac {\partial^2 z} {\partial y \partial x}=\frac {\partial(\frac{\partial z}{\partial y})} {\partial x}$$$$(z先对y求偏导的结果再对x求一次偏导)=(z先对x求偏导的结果再对y求一次偏导)$$
#### 【 链式法则 】
*链式法则（Chain Rule）是微积分中的一个重要规则，用于计算复合函数的导数。它描述了由一个函数复合另一个函数所引起的导数的变化关系。*

- 链式法则可以用于多元函数的情况，例如

```
y = f(u) u = g(x)
其中，y是关于x的复合函数，即y = f(g(x))。
```

- 如果函数y = f(u)和u = g(x)都可导，那么复合函数y = f(g(x))关于自变量x的导数可以表示为：	$$\frac{dy}{dx} = \frac{dy}{du} * \frac{du}{dx}$$
	即复合函数的导数等于外层函数对内层函数的导数乘以内层函数对自变量的导数。

▶️具体来说，链式法则的步骤如下：
	1)  计算外层函数关于内层函数的导数，即求dy/du。    
	2)  计算内层函数关于自变量的导数，即求du/dx。
	3)  将步骤1和步骤2的结果相乘，得到复合函数关于自变量的导数，即dy/dx。

- 要注意的是，链式法则可以推广到更多的嵌套函数的情况，只需将每个函数的导数乘积串联起来即可。同时，对于多元函数的情况，链式法则可以用于计算偏导数

#### 【 多元复合函数的偏导 】

*对于多元复合函数，我们可以使用链式法则来计算偏导数。链式法则可以帮助我们将一个复杂的函数分解成一系列简单的函数，然后计算每个函数的偏导数并将它们组合起来。*

假设我们有一个多元复合函数，例如：

	z = f(u, v) = f(u(x, y), v(x, y))

- 其中u = u(x, y)和v = v(x, y)是由x和y表示的中间变量。我们可以使用链式法则来计算z对x和z对y的偏导数。
- 偏导数的一般计算方法是先对函数进行求导，然后再将自变量的导数代入原函数中求值。对于多元复合函数，我们需要分别对每个中间变量求导数，然后将这些导数组合起来得到最终的偏导数。

- 例如，对于z对x的偏导数，我们可以按照以下步骤计算( 类似地，我们可以计算z对y的偏导数)
	1)  u(x, y)对x求偏导，得到u对x的偏导数。
	2)  f(u, v)对u求偏导，得到z对u的偏导数。
	3)  v(x, y)对x求偏导，得到v对x的偏导数。    
	4)  f(u, v)对v求偏导，得到z对v的偏导数。
	5)  最后将其链式起来,即可

$$\frac{\partial z}{\partial x}=\frac{\partial z}{\partial u}\frac{\partial u}{\partial x}+\frac{\partial z}{\partial v}\frac{\partial v}{\partial x}$$
>已知$F=2u+3v^2+4w^3$,其中$u=2x+y$, $v=3y+z$, $w=4z$ ,求$\frac{\partial F}{\partial x}$$\frac{\partial F}{\partial y}$$\frac{\partial F}{\partial z}$

1. $$\frac{\partial F}{\partial x}=\frac{\partial F}{\partial u}\frac{\partial u}{\partial x}+\frac{\partial F}{\partial v}\frac{\partial v}{\partial x}+\frac{\partial F}{\partial w}\frac{\partial w}{\partial x}$$
2. (注意求完偏导之后如果有剩下u,v,w,是可以化简到只剩下x,y,z的)
3. 同理$\frac{\partial F}{\partial y}$$\frac{\partial F}{\partial z}$也如此求解
4. 另外，为了规范，其实$\frac{\partial w}{\partial x}$应该写成$\frac{d w}{d x}$,因为$\partial$只能用在多元求导,因为$u=2x+y$, $v=3y+z$,均有两个未知数,但是 $w=4z$只有一个未知数,因此该函数只能是适用于一元求导符号$d$$$\frac{\partial F}{\partial x}=\frac{\partial F}{\partial u}\frac{\partial u}{\partial x}+\frac{\partial F}{\partial v}\frac{\partial v}{\partial x}+\frac{\partial F}{\partial w}\frac{d w}{d x}$$
5. 如果题干并无设函数u,v,而是将u(x,y)和v(x,y)直接写进f(u,v)中,那么就需要我们自己设

####  【 多元隐函数的偏导 】
*多元隐函数的偏导数是指在多元函数关系中，某个变量无法用显式的表达式表示，但可以通过隐函数的形式来表示。当我们需要计算这个隐函数的偏导数时，我们可以使用偏微分的方法来求解。*

- 考虑一个二元隐函数关系，例如 $F(x, y) = 0$，其中 $F$ 是一个多元函数。偏导数 $\frac{{\partial y}}{{\partial x}}$ 表示在 $F(x, y) = 0$ 的条件下，当 $x$ 发生微小变化时，$y$ 对应的变化率。

- 为了计算偏导数，我们可以使用隐函数定理。该定理陈述了在一定条件下，可以通过偏微分来计算隐函数的偏导数。具体步骤如下：

	1) 对隐函数关系 $F(x, y) = 0$ 求偏导数 $\frac{{\partial F}}{{\partial x}}$ 和 $\frac{{\partial F}}{{\partial y}}$。
	2)  解方程组 $\begin{cases} \frac{{\partial F}}{{\partial x}} = 0 \ \frac{{\partial F}}{{\partial y}} \cdot \frac{{\partial y}}{{\partial x}} = -1 \end{cases}$，得到 $\frac{{\partial y}}{{\partial x}}$ 的表达式。
	3) 根据得到的表达式计算 $\frac{{\partial y}}{{\partial x}}$ 的值。

这样，我们就可以通过隐函数定理来计算多元隐函数的偏导数。需要注意的是，这个方法只适用于满足隐函数定理条件的情况。此外，对于更高维度的多元隐函数，也可以类似地应用这个方法来计算偏导数。
> 已知$x^2+2y^2+3z^2+xy+xz+yz=0,求$$\frac{\partial z}{\partial x}$$\frac{\partial z}{\partial y}$ $\frac{\partial ^2z}{\partial x^2}$ 

1. 将$x^2+2y^2+3z^2+xy+xz+yz=0$写成$x^2+2y^2+3z^2+xy+xz+yz=F(x,y,z)$
2. 将$\frac{\partial F}{\partial z}$和$\frac{\partial F}{\partial x}$和$\frac{\partial F}{\partial y}$都算出来,最后再根据以下公式即可求解$\frac{\partial z}{\partial x}$和$\frac{\partial F}{\partial y}$(注意有负号)
$$\frac{\partial z}{\partial x}=-\frac{\frac{\partial F}{\partial x}}{\frac{\partial F}{\partial z}}\quad\frac{\partial z}{\partial y}=-\frac{\frac{\partial F}{\partial y}}{\frac{\partial F}{\partial z}}$$
3. 如果求二阶偏导,必须先求一阶偏导$\frac{\partial z}{\partial x}$的结果,
	1. 接着对于二阶偏导,如$\frac{\partial ^2z}{\partial x^2}$ 即z对x求偏导的 {结果} 再对x求偏导, 
	2. 要注意 z对x求偏导的 {结果} 中，根据隐函数可以知道z是一个包含x这个未知数的式子, 该 {结果} 如果再对x求偏导需要注意z不能被看成是常数(由于暂时不知道z(x,y)是如何包含x的,所以直接写$\frac{\partial z}{\partial x}$即可,
	3. 再整理之后,可以再带入$\frac{\partial z}{\partial x}$在之前算出来的结果即可求解$\frac{\partial ^2z}{\partial x^2}$

