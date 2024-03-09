​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​

## 簡單運算

	拉丁字母、阿拉伯數位和 +-*/= 運算符均可以直接輸入獲得，
	命令\cdot表示乘法的圓點，
	命令\neq表示不等號，
	命令\equiv表示恆等於，
	命令\quad表示空格

$$ x+2-3*4/6=4/y + x\cdot y $$
```tex
 $$ x+2-3*4/6=4/y + x\cdot y $$
```

$$ 0 \neq 1 \quad x \equiv x \quad 1 = 9 \bmod 2 $$
```tex
$$ 0 \neq 1 \quad x \equiv x \quad 1 = 9 \bmod 2 $$
```

## 上下標

	_表示下標、^表示上標，
	但上下標內容不止一個字元時，需用大括弧括起來。 單引號『表示求導

$$ a_{ij}^{2} + b^3_{2}=x^{t} + y' + x''_{12} $$
```tex
$$ a_{ij}^{2} + b^3_{2}=x^{t} + y' + x''_{12} $$
```


## 取整
	向上取整：

$$\lceil{x}\rceil $$
```latex
$$\lceil{x}\rceil $$
```

	向下取整：

$$\lfloor{x}\rfloor$$
```latex
$$\lfloor{x}\rfloor$$
```


## 分式

	分式可以用来输入，例如，效果为 `\dfrac{}{}` 。
	为了在行间、分子、分母或者指数上输入较小的分式，可以改用 `\frac{}{}`

$$\dfrac{a}{b}$$
```tex
$$\dfrac{a}{b}$$
```
$$a^\frac{1}{n}$$
```tex
$$a^\frac{1}{n}$$
```

## 括号

括号可以直接用输入，但是需要注意的是，有时候括号内的内容高度较大，需要改用`\left(..\right)`

$$\left(1+\dfrac{1}{n}\right)^n$$
```tex
$$\left(1+\dfrac{1}{n}\right)^n$$
```

在中间需要隔开时，可以用`\middle|`

$$\left(..\middle|..\right)^n$$

另外，输入大括号{}时需要用，其中起到了转义作用。`\{..\}` 

## 加粗

对于加粗的公式，建议使用宏包，并且用命令来加粗，这可以保留公式的斜体`bm`

$$\bm{1}$$
## 根號

	\sqrt表示平方根，
	\sqrt[n]表示n次方根，

$$\sqrt{x} + \sqrt{x^{2}+\sqrt{y}} = \sqrt[3]{k_{i}} - \frac{x}{m}$$
```tex

$$\sqrt{x} + \sqrt{x^{2}+\sqrt{y}} = \sqrt[3]{k_{i}} - \frac{x}{m}$$
```

## 上下取整
中括号 `[ x ] \left[ x \right][x]`

```latex
\left[ x \right]
```

向下取整（取整）符号 `⌊ x ⌋ \lfloor x \rfloor⌊x⌋`

```latex
\lfloor x \rfloor
```

向上取整（取顶）符号 `⌈ x ⌉ \lceil x \rceil⌈x⌉`

```latex
\lceil x \rceil
```

## 上下標記

	\overline，
	 \underline 分別在表達式上、下方畫出水平線

$$\overline{x+y} \qquad \underline{a+b}$$
```tex
$$\overline{x+y} \qquad \underline{a+b}$$
```


	\overbrace， 
	\underbrace 分別在運算式上、下方給出一個水準的大括弧

$$\overbrace{1+2+\cdots+n}^{n个} \qquad \underbrace{a+b+\cdots+z}_{26}$$
```tex
$$\overbrace{1+2+\cdots+n}^{n个} \qquad \underbrace{a+b+\cdots+z}_{26}$$
```


## 向量

	\vec表示向量，
	\overrightarrow表示箭頭向右的向量，
	\overleftarrow表示箭頭向左的向量

$$\vec{a} + \overrightarrow{AB} + \overleftarrow{DE}$$
```tex
$$\vec{a} + \overrightarrow{AB} + \overleftarrow{DE}$$
```


## 積分、極限、求和、乘積

	\int表示積分，\lim表示極限， \sum表示求和，\prod表示乘積，^、_表示上、下限

$$  \lim_{x \to \infty} x^2_{22} - \int_{1}^{5}x\mathrm{d}x + \sum_{n=1}^{20} n^{2} = \prod_{j=1}^{3} y_{j}  + \lim_{x \to -2} \frac{x-2}{x} $$
```tex
$$  \lim_{x \to \infty} x^2_{22} - \int_{1}^{5}x\mathrm{d}x + \sum_{n=1}^{20} n^{2} = \prod_{j=1}^{3} y_{j}  + \lim_{x \to -2} \frac{x-2}{x} $$
```

## 三圓點

	\ldots點位於基線上，\cdots點設置為居中，\vdots使其垂直，\ddots對角線排列

$$ x_{1},x_{2},\ldots,x_{5}  \quad x_{1} + x_{2} + \cdots + x_{n} $$
```tex
$$ x_{1},x_{2},\ldots,x_{5}  \quad x_{1} + x_{2} + \cdots + x_{n} $$
```


## 重音符號

$$ \hat{x} $$

```tex
$$ \hat{x} $$

```
$$ \bar{x} $$

```tex
$$ \bar{x} $$
```

$$ \tilde{x} $$
```tex
$$ \tilde{x} $$
```

## 矩陣

		其採用矩陣環境實現矩陣排列，
		常用的矩陣環境有matrix、bmatrix、vmatrix、pmatrix，
		其區別為在於外面的括弧不同：

	&用於分隔列，\用於分隔行

$$\begin{bmatrix}
1 & 2 & \cdots \\
67 & 95 & \cdots \\
\vdots  & \vdots & \ddots \\
\end{bmatrix}$$
```tex
$$\begin{bmatrix}
1 & 2 & \cdots \\
67 & 95 & \cdots \\
\vdots  & \vdots & \ddots \\
\end{bmatrix}$$
```

## 希臘字母

	希臘字母無法直接通過美式鍵盤輸入獲得。 在LaTeX中通過反斜杠\加上其字母讀音實現，將讀音首字母大寫即可輸入其大寫形式，
	
$$ \alpha^{2} + \beta = \Theta  $$
```tex
$$ \alpha^{2} + \beta = \Theta  $$
```

$$ \alpha
\beta
\gamma
\delta
\epsilon
\theta
\vartheta
\iota
\kappa
\lambda
\varepsilon
\mu
\zeta
\eta
\nu
\xi
\upsilon
\pi
\varpi
\rho
\phi
\varphi
\chi
\varrho
\psi
\sigma
\omega
\varsigma
\tau

\Sigma
\Lambda
\Gamma
\Psi
\Delta
\Xi
\Upsilon
\Omega
\Theta
\Pi
\Phi $$
```tex
$$ 
\alpha
\beta
\gamma
\delta
\epsilon
\theta
\vartheta
\iota
\kappa
\lambda
\varepsilon
\mu
\zeta
\eta
\nu
\xi
\upsilon
\pi
\varpi
\rho
\phi
\varphi
\chi
\varrho
\psi
\sigma
\omega
\varsigma
\tau

\Sigma
\Lambda
\Gamma
\Psi
\Delta
\Xi
\Upsilon
\Omega
\Theta
\Pi
\Phi 

$$
```

## 多行公式

## 公式組合

	通過cases環境實現公式的組合，&分隔公式和條件，
	還可以通過\limits來讓x→0位於lim的正下方而非預設在lim符號的右下方顯示

$$D(x) = \begin{cases}
\lim\limits_{x \to 0} \frac{a^x}{b+c}, & x<3 \\
\pi, & x=3 \\
\int_a^{3b}x_{ij}+e^2 \mathrm{d}x,& x>3 \\
\end{cases}$$
```tex
$$D(x) = \begin{cases}
\lim\limits_{x \to 0} \frac{a^x}{b+c}, & x<3 \\
\pi, & x=3 \\
\int_a^{3b}x_{ij}+e^2 \mathrm{d}x,& x>3 \\
\end{cases}$$
```


## 拆分單個公式

	通過split環境實現公式拆分

$$\begin{split}
\cos 2x &= \cos^2x - \sin^2x \\
&=2\cos^2x-1
\end{split}$$
```tex
$$\begin{split}
\cos 2x &= \cos^2x - \sin^2x \\
&=2\cos^2x-1
\end{split}$$
```
