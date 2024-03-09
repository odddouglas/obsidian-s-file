[Documentation - MATLAB & Simulink - MathWorks 中国](https://ww2.mathworks.cn/help/index.html)

## 1.1 [MATLAB](https://so.csdn.net/so/search?q=MATLAB&spm=1001.2101.3001.7020) 产品描述

全世界数超过百万的工程师和科学家们使用 MATLAB 来分析和设计改变世界的系统和产品。MATLAB 应用于汽车安全系统、宇宙飞船、健康监测设备、智能电网及LTE蜂窝网络等领域。它被用于机器学习、信号处理、图像处理、计算机视觉、网络通信、数值计算、控制设计、机器人技术等等。

### 1.1.1 数学、图形与编程

基于矩阵的 MATLAB 语言是世界上最自然的表达数学计算的方法。内置的图形显示功能使我们更容易将数据可视化并从中得到新认识。一个强大的内置工具箱可以让您快速上手您的领域所必需的算法。MATLAB 各种功能需要我们去实践、探索和发现。这些 MATLAB 工具和功能都经过严格的测试，还可以协同工作。

### 1.1.2 规模、集成与部署

MATLAB 将您的想法呈现在屏幕上。您可以在更大的数据集合上运行分析，并扩展到数据集群和云平台。MATLAB 代码可以与其他语言集成，帮助您在网络、企业和生产系统中部署算法和应用程序。

### 1.1.3 关键特性

- 用于科学和工程计算的高级语言；
- 为迭代开发、设计和解决问题而优化的桌面环境；
- 用于可视化数据的图形和用于创建自定义图表的工具；
- 用于曲线配合、数据分类、信号分析、控制系统调优等任务的应用；
- 为广泛的工程和科学应用程序而附加的工具箱；
- 用于构建具有自定义用户接口的应用程序的工具；
- 用于 C/ C++、Java®、.NET、Python、SQL、Hadoop 和 Microsoft® Excel® 的接口；
- 可选择免版税部署的方式与最终用户共享 MATLAB 程序。

## 1.2 MATLAB 界面基础知识

当您启动 MATLAB 时，桌面显示为默认布局。![图1default layout](https://img-blog.csdnimg.cn/20190405181334788.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)默认界面包括以下面板：

- **Current Folder** ：访问您的文件；
- **Command Window** ：在命令行输入命令，由**提示符 >>** 指示 ；
- **Workspace** ：浏览您从文件中创建或导入的数据。

当你使用 MATLAB 时，您可以发出创建变量和调用函数的命令。例如，通过在命令行输入这个语句创建一个名为 **a** 的变量：

```
a = 1
```

MATLAB 将变量 **a** 添加到工作区并在命令窗口中显示结果。

```
a =
	1
```

创建更多的变量。

```
b = 2

b =
	2
c = a + b

c =
	3
d = cos(a)

d =
	0.5403 
```

当不指定输出变量时，MATLAB 使用变量 **ans** （answer的缩写）存储计算结果。

```
sin(a)

ans =
	0.8415 
```

如果以**英文分号**（**;**）结束语句，MATLAB 会执行计算，但会在命令窗口中隐藏对应的输出值。

```
e = a * b;
```

您可以通过按**向上箭头键**（**↑**）和**向下箭头键**（**↓**）来撤销之前的命令。在空命令行或键入命令的第一个字符后按箭头键。例如，要撤销命令 **b = 2**，先键入 **b**，然后按向上箭头键。

## 1.3 矩阵与数组

MATLAB 是 “矩阵实验室” 的缩写。虽然其他编程语言大多一次处理一个数，但 MATLAB 主要用于对整个矩阵和数组进行操作。

无论数据类型如何，所有 MATLAB 变量都是多维数组。矩阵是线性代数中常用的二维数组。

### 1.3.1 建立数组

若要在一行中创建包含四个元素的数组，请使用**英文逗号**（**,**）或**空格**分隔这些元素。

```
a = [1 2 3 4]

a = 1×4
	1 2 3 4 
```

这种类型的数组称为行向量。  
若要创建具有多行的矩阵，请用分号分隔行。

```
a = [1 2 3; 4 5 6; 7 8 10]

a = 3×3
	1 2 3
	4 5 6
	7 8 10 
```

创建矩阵的另一种方法是使用函数，如产生一组 1、0 或随机数。例如，创建一个由 0 组成的 5×1 列向量。

```
z = zeros(5,1)

z = 5×1
	0
	0
	0 
	0
	0
```

### 1.3.2 矩阵和数组运算

MATLAB 允许您使用一个算术运算符或函数处理矩阵中的所有值。

```
a + 10

ans = 3×3
	11 12 13
	14 15 16
	17 18 20
sin(a)

ans = 3×3
	0.8415    0.9093   0.1411
	-0.7568  -0.9589  -0.2794
	0.6570    0.9894  -0.5440 
```

若要转置矩阵，请使用**单引号**（**’**):

```
a'

ans = 3×3
	1 4 7
	2 5 8
	3 6 10 
```

您可以使用 * 运算符执行标准矩阵乘法，它计算行和列之间的内积。例如，如果一个矩阵乘以它的逆矩阵返回单位矩阵：

```
p = a*inv(a)
p = 3×3 
	1.0000        0   -0.0000
	     0   1.0000         0
	     0        0    1.0000 
```

注意，**p** 不是一个整数值矩阵。MATLAB 将数字存储为浮点值，算术运算对实际值与其浮点表示之间的细微差别很敏感。您可以使用 **format** 命令显示更高的十进制小数位数：

```
format long
p = a*inv(a)

p = 3×3
	1.000000000000000                 0 -0.000000000000000
	                0 1.000000000000000                  0
	                0                 0  0.999999999999998 
```

使用以下命令将显示更短的数据格式

```
format short 
```

数据格式只影响数字的显示，而不影响 MATLAB 对结果的运算或保存。

要执行元素乘而不是矩阵乘，请使用 **.*** 运算符：

```
p = a.*a

p = 3×3
	 1  4   9
	16 25  36
	49 64 100 
```

用于乘法、除法和幂运算的矩阵运算符都有一个对应的数组运算符，该数组运算符按元素顺序操作。例如，取 **a** 的每一个元素的三次方：

```
a.^3 

ans = 3×3
	  1    8    27
	 64  125   216
	343  512  1000 
```

### 1.3.3 数组的拼合

数组的拼合是将数组拼接起来以生成更大的数组的过程。实际上，您通过拼合第一个数组的各个元素来构建一个数组。拼合操作符是一对**方括号 [ ]** 。

```
A = [a,a]

A = 3×6
	1 2  3 1 2  3
	4 5  6 4 5  6
	7 8 10 7 8 10 
```

使用逗号连接相邻的数组称为水平连接。每个数组必须具有相同的行数。类似地，当数组具有相同数量的列数时，可以使用分号垂直连接。

```
A = [a; a]

A = 6×3
	1 2  3
	4 5  6
	7 8 10
	1 2  3
	4 5  6
	7 8 10 
```

### 1.3.4 复数的表示

复数有实部和虚部，其中虚部是 -1 的平方根。

```
sqrt(-1) 

ans = 0.0000 + 1.0000i 
```

要表示复数的虚部，可以用 i 或 j。

```
c = [3+4i, 4+3j; -i, 10j]

c = 2×2 complex
	3.0000 + 4.0000i      4.0000 + 3.0000i
	0.0000 - 1.0000i      0.0000 +10.0000i 
```

## 1.4 数组的索引

MATLAB 中的每个变量都是一个可以容纳许多数字的数组。当您想访问数组中选定的元素时，请使用索引。  
4×4 的方阵 **A**：

```
A = magic(4)

A = 4×4
	16  2  3 13
	 5 11 10  8
	 9  7  6 12
	 4 14 15  1 
```

有两种方法可以引用数组中的特定元素。最常见的方法是指定行和列下标，例如：

```
A(4,2)

ans = 14 
```

不太常见但有时有用的方法是使用一个下标，按顺序遍历每一列：

```
A(8)

ans = 14 
```

使用单个下标来引用数组中的特定元素称为线性索引。

如果试图引用赋值语句右侧数组外的元素，MATLAB会提示错误。

```
test = A(4,5) 
```

因为索引超过矩阵的维数。

然而，在赋值语句的左侧，您可以指定当前维度之外的元素。这样，数组的会增加到需要的行数或列数。

```
A(4,5) = 17 

A = 4×5
	16   2   3  13   0
	 5  11  10   8   0
	 9   7   6  12   0
	 4  14  15   1  17 
```

要引用数组的多个元素，可以使用冒号操作符，它允许指定形式为 **start:end**。例如，列出 A 的第 1 行和第 2 列的元素：

```
A(1:3,2)

ans = 3×1
	 2
	11
	 7 
```

仅冒号（没有起始值 start 或结束值 end）是指定该维度中的所有元素。例如，选择 **A** 的第三行中的所有列：

```
A(3,:)

ans = 1×5
	9 7 6 12 0 
```

冒号运算符还允许您使用更通用的形式 **start:step:end** 创建一个等间距的值向量。

```
B = 0:10:100

B = 1×11
	0 10 20 30 40 50 60 70 80 90 100 
```

如果省略中间步骤，如 **start:end**，MATLAB 使用默认的步骤值 1。

## 1.5 工作空间变量

工作区包含您在 MATLAB 中创建或者从数据块或其他程序导入到 MATLAB 中的变量。例如，这些语句在工作区中创建变量 A 和 B。

```
A = magic(4);
B = rand(3,5,2); 
```

您可以使用 **whos** 查看工作区的内容。

```
whos

  Name   Size   Bytes   Class   Attributes
  A      4x4    128     double
  B      3x5x2  240   double 
```

这些变量也出现在桌面上的 **Workspace** 窗格中。![图2.Workspace](https://img-blog.csdnimg.cn/2019040520495848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
退出 MATLAB 后，工作区变量不会持久存在。用 **save** 命令可以保存您的数据，以便稍后使用。

```
save myfile.mat 
```

保存保存当前工作文件夹中的工作区，保存在一个压缩的文件中，扩展名为 .mat，文件名为 **MAT-file**。

要清除工作区中的所有变量，请使用 **clear** 命令。

使用 **load** 命令将数据从 **MAT-file** 文件中恢复到工作区。

```
load myfile.mat 
```

## 1.6 文本和字符串

### 1.6.1 字符串数组中的文本

处理文本时，将字符序列括在双引号中。你可以将文本分配给变量。

```
t = "Hello, world"; 
```

如果文本包含双引号，请在变量的定义中使用两个双引号。

```
q = "Something ""quoted"" and something else."

q =
	"Something "quoted" and something else. "
```

**t** 和 **q** 是数组，就像所有 MATLAB 变量一样。它们的类或数据类型是 **string**。

```
 whos t
 
   Name   Size   Bytes   Class   Attributes
   t      1x1    174     string 
```

> 注意：在R2017a中引入了使用双引号创建字符串数组。如果使用较早的版本，请创建字符数组。有关详细信息，请参见“字符数组中的数据”一节。

若要将文本添加到字符串末尾，请使用加号操作符 **+**。

```
f = 71;
c = (f-32)/1.8;
tempText = "Temperature is " + c + "C"

tempText =
"Temperature is 21.6667C" 
```

与数字数组类似，字符串数组可以有多个元素。使用 **strlength** 函数查找数组中每个字符串的长度。

```
A = ["a","bb","ccc"; "dddd","eeeeee","fffffff"] 

A =
	2×3 string array
	"a"     "bb"      "ccc"
	"dddd"  "eeeeee"  "fffffff"
strlength(A)

ans =
	1 2 3
	4 6 7 
```

### 1.6.2 字符数组中的数据

有时字符表示与文本不对应的数据，如 DNA 序列。您可以将这种类型的数据存储在字符数组中，该数组具有数据类型 **char**。  
字符数组使用单引号。

```
seq = 'GCTAGAATCC';
whos seq

	Name  Size  Bytes  Class   Attributes
	seq   1x10  20     char 
```

数组的每个元素都包含一个字符。

```
seq(4)

ans =
	'A' 
```

用方括号连接字符数组，就像连接数字数组一样。

```
seq2 = [seq 'ATTAGAAACC']

seq2 =
	'GCTAGAATCCATTAGAAACC' 
```

字符数组在引入字符串数组之前编写的程序中很常见。所有接受字符串数据的 MATLAB 函数也接受 **char** 数据，反之亦然。

### 1.6.3 函数的调用

MATLAB 提供了大量执行计算任务的函数。  
函数相当于其他编程语言中的子程序或方法。

要调用函数，如 **max**，将其输入参数括在括号中：

```
A = [1 3 5];
max(A)

ans = 5 
```

如果有多个输入参数，请用逗号分隔：

```
B = [10 6 4];
max(A,B)

ans = 1×3
	10 6 5 
```

通过将函数赋值给一个变量，返回函数的输出：

```
maxA = max(A)

maxA = 5 
```

当有多个输出参数时，用方括号括起来：

```
[maxA,location] = max(A)

maxA = 5
location = 3 
```

将任何字符输入用单引号括起来：

```
disp('hello world')

hello world 
```

若要调用不需要任何输入且不返回任何输出的函数，只需键入函数名：

```
clc 
```

**clc** 函数用于清除命令窗口。

## 1.7 二维图和三维图

### 1.7.1 二维图

要创建二维曲线图，请使用 **plot** 函数。例如，绘制正弦函数的值从 0 到 2π ：

```
x = 0:pi/100:2*pi;
y = sin(x);
plot(x,y) 
```

![图3：y=sin(x) 曲线图](https://img-blog.csdnimg.cn/20190405212802426.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
您可以命名这些轴并添加标题。

```
xlabel('x')
ylabel('sin(x)')
title('Plot of the Sine Function') 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190407150952717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
通过向 **plot** 函数添加第三个输入参数，可以使用红色虚线绘制相同的变量。

```
plot(x,y,'r--')
```

![图5：plot 函数添加第三个输入参数后的图像](https://img-blog.csdnimg.cn/20190405213347863.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
**’r–’** 是一种指定的曲线类型。每种指定类型可以包含行颜色、样式和标记。标记是注释在每个绘制的数据点上的符号，例如一个 **+** ，一个 **o** 或者一个 ***** 等符号。例如，**‘g:*’** 说明需要显示一条带 * 标记的虚线。

请注意，您为第一张图形编写的标题和标签已不在当前 **figure** 窗口中。MATLAB 会在每次调用绘图函数、重置坐标轴和其他元素来准备新绘图时清除 **figure**。

若要向现有 **figure** 添加图形，请使用 **hold on**命令。在使用 **hold off** 命令或关闭窗口之前，所有绘图将显示在当前 **figure** 窗口中。

```
x = 0:pi/100:2*pi;
y = sin(x);
plot(x,y) 
hold on
y2 = cos(x);
plot(x,y2,':')
legend('sin','cos')
hold off 
```

![图6.使用添加图形命令](https://img-blog.csdnimg.cn/2019040521515861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)

### 1.7.2 三维图

三维图通常显示由两个变量组成的函数 z = f(x,y) 所绘制的曲面。

要计算 z 的值，首先使用 **meshgrid** 在函数的定义域上标记一系列 (x,y) 点。

```
[X,Y] = meshgrid(-2:.2:2);
Z = X .* exp(-X.^2 - Y.^2); 
```

然后，拟合出一个曲面图形。

```
surf(X,Y,Z) 
```

![图7.创建曲面图形](https://img-blog.csdnimg.cn/20190407113935794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
**surf** 函数和与之一起使用的 **mesh** 函数用于在三维空间显示曲面。  
**surf** 函数用于彩色显示出连接线和表面。  
**mesh** 函数用于产生表面的线框，并且只标记的点之间的连线线框着色。

### 1.7.3 子图

您可以使用 **subplot** 函数在同一个窗口的不同子区域中显示多个绘图。

**subplot** 函数的前两个参数表示每一行和每一列中的图的数量。第三个参数指对应的第几个图处于活动状态，即可编辑的状态。例如，在 figure 窗口内的 2×2 网格中创建四幅图。

```
t = 0:pi/10:2*pi;
[X,Y,Z] = cylinder(4*cos(t));
subplot(2,2,1); mesh(X); title('X');
subplot(2,2,2); mesh(Y); title('Y');
subplot(2,2,3); mesh(Z); title('Z');
subplot(2,2,4); mesh(X,Y,Z); title('X,Y,Z'); 
```

![图8.创建四幅图](https://img-blog.csdnimg.cn/20190407120419734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)

## 1.8 程序脚本

最简单的 MATLAB 程序类型称为脚本。脚本是一个包含多次使用 MATLAB 命令行和函数调用的文件。您可以通过在命令行中键入脚本的名称来运行脚本。

### 1.8.1 脚本

要创建脚本，请使用 **edit** 命令，

```
edit mysphere 
```

这个命令打开一个名为 **mysphere.m** 的空文件。输入一些代码，可以创建一个单位球面，半径为两个单位长度，然后绘制结果：

```
[x,y,z] = sphere;
r = 2;
surf(x*r,y*r,z*r)
axis equal 
```

接下来，添加计算球体表面积和体积的代码：

```
A = 4*pi*r^2;
V = (4/3)*pi*r^3; 
```

在您编写代码的时候，添加解释代码的注释是个很好的习惯。  
注释帮助其他人理解您的代码，并且可以帮助您在以后看到这些代码的时候能快速的回忆起来。在 MATLAB 中使用百分号 (%) 添加注释。

```
% 创建并绘制一个半径为 r 的球体。
[x,y,z] = sphere;      % 创建一个单位球体。
r = 2;
surf(x*r,y*r,z*r)      % 调整每个维度和图表。
axis equal             % 每个轴使用相同的比例。
% 求表面积和体积。
A = 4*pi*r^2;
V = (4/3)*pi*r^3; 
```

将这份文件保存在当前文件夹中。如果要运行脚本，请在命令行中键入它的名称：

```
mysphere
```

您还可以使用编辑器 Editor 中的 **run** 按钮来运行脚本。  
![图9. run 按钮](https://img-blog.csdnimg.cn/20190407151354305.png)

### 1.8.2 实时脚本

您可以在 **live scripts** 中使用格式化选项来增强代码，而不是用纯文本的方式编写代码和注释。实时脚本允许您查看代码和输出并与之交互，还可以包含格式化的文本、方程式和图像。

例如，通过选择 **Save As** 并将文件类型更改为 MATLAB 实时代码文件 **(*.mlx)**， **mysphere** 此时便转换为实时脚本。然后，用格式化的文本替换代码注释。例如：

- 将评论行转换为文本。选择以百分号开头的每一行，然后选择 **Text** 选项，删除百分号。  
    ![图10. Text选项](https://img-blog.csdnimg.cn/20190407124608797.png)
- 重写文本以替换代码行末尾的注释。如果要将 monospace 字体应用于文本中的函数名，请点击 **Live Editor** 选项卡上的 **Text** 选项中的按钮 **M** 。

![图12. 将 monospace 字体应用于函数名](https://img-blog.csdnimg.cn/20190407125943410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTY1MTEy,size_16,color_FFFFFF,t_70)  
若要添加方程式，请点击 **Insert** 选项卡上的 **Equation** 选项。

若要新建一个实时脚本，请使用 **edit** 命令，并在文件名中添加 **.mlx** 扩展名：

```
edit newfile.mxl 
```

### 1.8.3 循环语句和条件语句

您可以在任何一个脚本中定义代码区段用于编写循环语句或条件语句。循环语句使用关键字 **for** 或 **while** ，条件语句使用关键字 **if** 或 **switch**。

循环对于创建序列很有用。例如，创建一个名为 **fibseq** 的脚本，它使用 **for** 循环来计算斐波那契数列的第一个100个数字。在这个序列中，第一个数字是1，后面的每个数字是前两个数字的和，递推公式：  
F n = F n − 1 + F n − 2 Fn = Fn-1 + Fn-2Fn=Fn−1+Fn−2

```
N = 100;
f(1) = 1; 
f(2) = 1;
for n = 3:N
	f(n) = f(n-1) + f(n-2);
end
f(1:10) 
```

运行脚本时，**for** 语句定义一个名为 **n** 的计数器，该计数器从 3 开始。然后，循环语句重复地给 **f(n)** 赋值，每执行一次循环，**n** 增加 1，直到达到 100。脚本中的最后一个命令 **f(1:10)** 显示了 **f** 的前10个元素。

```
ans =
	1 1 2 3 5 8 13 21 34 55 
```

条件语句只在给定表达式为真时执行。例如，依据随机数的大小为变量赋值：**‘low’**，**‘medium’** 或者 **‘high’** 。在本案例中，随机数是 1 到 100 之间的整数。

```
num = randi(100)
if num < 34
	sz = 'low'
elseif num < 67
	sz = 'medium'
else
	sz = 'high'
end 
```

语句 **sz = ‘high’** 只在 **num** 大于或等于 67 时执行。

### 1.8.4 脚本位置

MATLAB 在某些地方查找脚本和其他文件的规则：如果要运行脚本，那么脚本文件必须位于当前文件夹或在搜索路径的某个文件夹中。

默认情况下，MATLAB 安装程序创建的 **MATLAB** 文件夹位于搜索路径上。如果希望将程序存储和运行在另一个文件夹中，请将其添加到搜索路径。选择当前文件夹浏览器中的文件夹，右键单击，然后选择 **Add to Path**。

## 1.9 帮助和说明

所有 MATLAB 函数都有支持文档，其中包括示例和函数的输入、输出和语法的调用。以下有几种方法可以从命令行访问这些信息：

- 使用 **doc** 命令在单独的窗口中打开函数文档。

```
    doc mean
```

- 在你写出函数用于输入参数的圆括号时，命令窗口会显示函数的提示(函数说明的语法部分的提示)。

```
    mean(  
```

- 使用 **help** 命令在命令窗口中查看函数说明的简略文本版本。

```
    help mean 
```

通过单击帮助图标来获取完整的产品文档。  
![帮助图标](https://img-blog.csdnimg.cn/20190407135943613.png)