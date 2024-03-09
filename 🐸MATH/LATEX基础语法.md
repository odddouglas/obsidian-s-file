## 前言：排版工具与书写工具的讨论

LaTeX是一种“非所见即所得”的排版系统，用户需要输入特定的代码，保存在后缀为.tex的文件中，通过编译得到所需的pdf文件，例如以下代码：

```tex
\documentclass{article}

\begin{document}

Hello, world!

\end{document}
```

最后输出的结果是一个pdf文件，内容是”Hello, world!“。

如何理解“非所见即所得”呢？在这里举个“所见即所得”的例子：Word。Word的界面就是一张A4纸，输入的时候是什么样子，最后呈现出来就是什么样子。这给了我们极高的**自由度**，也非常容易上手，但是有如下问题： - 对于对细节不敏感的用户，Word的排版常常会在细节存在问题，比如两段话之间行间距不同、字体不同、标题样式不同等； - 对于撰写论文的用户，Word的标题、章节、图表、参考文献等无法自动标号，也很难在正文中引用； - 对于有公式输入需求的用户，Word自带的公式不稳定，而公式插件效果常常不好。

相比之下，使用LaTeX进行排版，就像是在铺好的轨道上驾驶火车一样。使用LaTeX没有办法像Word一样非常自由，但是可以保证**规范性**，这使得LaTeX非常适合用于论文的排版。在学习的过程中，也将会感受到这一点。

无论是LaTeX还是Word，其归根结底都只是**排版工具**，用Word也可以排出LaTeX的效果，用LaTeX也可以排出Word的效果。另外，笔者最建议的**书写工具**是Markdown，其书写的过程中可以不在意排版，也支持使用LaTeX语法输入公式，与LaTeX之间的转换非常方便。

## 准备工作：安装LaTeX与配置环境

### 安装Tex Live

官方的地址是[http://mirror.ctan.org/systems/texlive/Images/texlive2021.iso](http://mirror.ctan.org/systems/texlive/Images/texlive2021.iso)，但是可能速度较慢，以下是一些国内的镜像地址：

-   清华大学：[https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso)；
-   北京交通大学：[https://mirror.bjtu.edu.cn/ctan/systems/texlive/Images/texlive2021.iso](https://mirror.bjtu.edu.cn/ctan/systems/texlive/Images/texlive2021.iso)；
-   上海交通大学：[https://mirrors.sjtug.sjtu.edu.cn/ctan/systems/texlive/Images/texlive2021.iso](https://mirrors.sjtug.sjtu.edu.cn/ctan/systems/texlive/Images/texlive2021.iso)；
-   中国科技大学：[https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso](https://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso)；
-   重庆大学：[https://mirrors.cqu.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso](https://mirrors.cqu.edu.cn/CTAN/systems/texlive/Images/texlive2021.iso)；
-   腾讯云：[https://mirrors.cloud.tencent.com/CTAN/systems/texlive/Images/texlive2021.iso](https://mirrors.cloud.tencent.com/CTAN/systems/texlive/Images/texlive2021.iso)。

其中的iso文件可以使用压缩软件解压，或者加载到光盘，接下来直接安装就行了。对于其他操作系统的用户（如MacOS），可以参考[TeX Live 下载及安装说明 | 始终 (liam.page)](https://liam.page/texlive/)中的方法。

### 选择TeX编辑器

对于新手，最推荐的编辑器是TeXworks，非常适合用来上手，也避免了配置环境带来的问题。如果想要提高效率的话，可以选用：

-   TeXstudio，安装地址为[TeXstudio - A LaTeX editor (sourceforge.net)](http://texstudio.sourceforge.net/)；
-   宇宙第一的Visual Studio Code，这是笔者最建议的TeX编辑器，不过需要手动配置LaTeX，较为麻烦；
-   另外，也有在线的编辑器，如[Overleaf, 在线LaTeX编辑器](https://cn.overleaf.com/)。

### 选择pdf阅读器和编辑器

LaTeX编译的结果是pdf文件，建议选用专业的pdf阅读器或pdf编辑器。特别是在阅读beamer类型的文件时，不同的阅读器效果差别极大。在这里推荐Acrobat：

-   Adobe Acrobat Reader，免费，可用于查看、签署、协作处理和批注 PDF 文件，安装地址为[Adobe Acrobat Reader (中国)](https://www.adobe.com/cn/acrobat/pdf-reader.html)；
-   Adobe Acrobat Pro，付费，可用于创建、保护、转换和编辑 PDF文件，安装地址为[Adobe Acrobat | Adobe Document Cloud](https://www.adobe.com/cn/acrobat.html)。

## 利用LaTeX编写文档


#### 整体架构
```latex
%只能用一个
\documentclass{article}  %论文
\documentclass{apmcmthesis} %亚太模板
\documentclass{...}
```

```latex
%空行是很重要的事情
%%正文区
\begin{document} %正文区文章环境开始
	
	\pagestyle{frontmatterstyle} %页眉页脚设置
	
\begin{abstract} 		%摘要环境开始
	
	This is my abstract. 	 %摘要内容
	\keywords{Keywords1\quad Keywords2\quad Keywords3} %关键字
	
\end{abstract}			%摘要环境结束


%目录
\newpage %生成新页
\tableofcontents %生成目录
\newpage 
\pagestyle{mainmatterstyle} %页面格式
\setcounter{page}{1} %将此页设置成第一页

%章节开始

.
.
.
.

\end{document} %这个end一定要写在论文的后面后面
```

### 文档类型

TeX有多种文档类型可选，笔者较常用的有如下几种类型：

-   对于英文，可以用、和；`book``article``beamer`
-   对于中文，可以用、和，这些类型自带了对中文的支持。`ctexbook``ctexart``ctexbeamer`

不同的文件类型，编写的过程中也会有一定的差异，如果直接修改文件类型的话，甚至会报错。以下统一选用。在编辑框第一行，输入如下内容来设置文件类型：`ctexart`

```tex
\documentclass{ctexart}
```

另外，一般也可以在处设置基本参数，笔者通常设置默认字体大小为12pt，纸张大小为A4，单面打印。需要将第一行的内容替换为：`\documentclass`

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
```

文件的正文部分需要放入document环境中，在document环境外的部分不会出现在文件中。

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}

\begin{document}

这里是正文. 

\end{document}
```

### 宏包

为了完成一些功能（如定理环境），还需要在导言区，也即document环境之前加载宏包。加载宏包的代码是。本份教程中，与数学公式与定理环境相关的宏包为、、，用于插入图片的宏包为，代码如下：`\usepackage{}``amsmath``amsthm``amssymb``graphicx`

```tex
\usepackage{amsmath, amsthm, amssymb, graphicx}
```

另外，在加载宏包时还可以设置基本参数，如使用超链接宏包，可以设置引用的颜色为黑色等，代码如下：`hyperref`

```tex
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}
```

### 标题

标题可以用设置，作者可以用设置，日期可以用设置，这些都需要放在导言区。为了在文档中显示标题信息，需要使用。例如：`\title{}``\author``\date{}``\maketitle`

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

这里是正文. 

\end{document}
```
### 多级标题以及换行
```latex
%多级标题
\section{a} %一级标题 
\hspace{2em}This is my passage. %换行加开头缩进

This is my passage. \par % 因为上一行是空行因此当前行是缩进的, 且当前行的\par会让下一行缩进 
This is my passage. \\ % 双斜杠\\不会让下一行缩进 但是空行会,因为空行的作用跟\par一样

```
### 正文

正文可以直接在document环境中书写，没有必要加入空格来缩进，因为文档默认会进行首行缩进。相邻的两行在编译时仍然会视为同一段。在LaTeX中，另起一段的方式是使用一行相隔，例如：

```tex
我是第一段. 

我是第二段.
```

这样编译出来就是两个段落。在正文部分，多余的空格、回车等等都会被自动忽略，这保证了全文排版不会突然多出一行或者多出一个空格。另外，另起一页的方式是：

```tex
\newpage
```

笔者在编写文档时，为了保证美观，通常将中文标点符号替换为英文标点符号（需要注意的是英文标点符号后面还有一个空格），这比较适合数学类型的文档。

### 特殊字体

| 字体     | 命令       |
| -------- | ---------- |
| 直立     | \\textup{} |
| 意大利   | \\textit{} |
| 倾斜     | \\textsl{} |
| 小型大写 | \\textsc{} |
| 加宽加粗 | \\textbf{} |
|          |            |

### 列表

LaTeX中的列表环境包含无序列表、有序列表和描述，以为例，用法如下：`itemize``enumerate``description`

```tex
\begin{enumerate}
    \item 这是第一点; 
    \item 这是第二点;
    \item 这是第三点. 
\end{enumerate}
```

```latex
\begin{itemize}

	\item Local optimization and overall optimization:
	\item Sensitivity:
	\item
	\item Trend:
	\item Comparison:

\end{itemize}
```

```latex
\begin{description}

	\item sdsdsd

\end{description} 
```

### 章节

对于文件类型，章节可以用和命令来标记，例如：`ctexart` `\section{}` `\subsection{}`

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

\section{一级标题}

\subsection{二级标题}

这里是正文. 

\subsection{二级标题}

这里是正文. 

\end{document}
```

### 目录

在有了章节的结构之后，使用命令就可以在指定位置生成目录。通常带有目录的文件需要编译两次，因为需要先在目录中生成.toc文件，再据此生成目录。`\tableofcontents`

```tex
\documentclass[12pt, a4paper, oneside]{ctexart}
\usepackage{amsmath, amsthm, amssymb, graphicx}
\usepackage[bookmarks=true, colorlinks, citecolor=blue, linkcolor=black]{hyperref}

% 导言区

\title{我的第一个\LaTeX 文档}
\author{Dylaaan}
\date{\today}

\begin{document}

\maketitle

\tableofcontents

\section{一级标题}

\subsection{二级标题}

这里是正文. 

\subsection{二级标题}

这里是正文. 

\end{document}
```

### 图片

插入图片需要使用宏包，建议使用如下方式：`

```latex
\usepackage{graphicx}
```

```latex

The primary notations used in this paper are listed in \textbf{Figure \ref{test1}}
%这个交叉引用类似如图所示,里面的bf是加粗

%引用图片
\begin{figure}[!htbp]
	\small
	\centering %图片居中
	\includegraphics[width=12cm]{1.jpg} %图片大小,图片名和格式
	\caption{‘‘my picture 1’’}\label{test1} %图片的名称和引用的名称
\end{figure}

The above discussion is illustrated in \textbf{Figure \ref{test3}} 
%这个交叉引用类似如图所示,里面的bf是加粗
```

```latex
%并排引入图片
\begin{figure}[htbp]
	\centering
	\begin{minipage}[t]{0.48\textwidth}
		\centering
		\includegraphics[width=6cm]{2.png}
		\caption{‘‘pngtest’’}\label{test2}
	\end{minipage}
	\begin{minipage}[t]{0.48\textwidth}
		\centering
		\includegraphics[width=6cm]{cat.pdf}
		\caption{‘‘pdftest’’}\label{test3}
	\end{minipage}
\end{figure}
```

```latex
%并排引入图片2
\begin{figure}[!ht]
	\centering
	\includegraphics[width=3cm]{cat.pdf} \quad \includegraphics[width=5cm]{1.jpg}
	\caption{cat}\label{cat1}
\end{figure}

```

% `[htbp]`的作用是自动选择插入图片的最优位置，
% `\centering`设置让图片居中，
% `[width=8cm]`设置了图片的宽度为8cm，中括号内还可以写`[height=8cm,width=8cm]` 来同时控制高和宽
% `\caption{}`用于设置图片的标题。

### 公式
```latex
$R_1=\frac{x}{x_{0}}=ae^{-kt}$ %行内公式

$$R_1=\frac{x}{x_{0}}=ae^{-kt}$$ %行间公式
```

```latex
\begin{equation} %单行公式自动编号
	1+1=2\quad
	1+2=3\quad
	1+3=4
\end{equation}

```

```latex
%split
\begin{equation}
	\begin{split}
		1 + 1 &= 2 \\ %%这个&符号用于对齐
		1 + 2 &= 3 \\
		1 + 3 &= 4
	\end{split}
\end{equation}
```

```latex
%aligned和split作用一样
\begin{equation}
	\begin{aligned}
		1 + 1 &= 2 \\
		1 + 2 &= 3 \\
		1 + 3 &= 4
	\end{aligned}
\end{equation}

```

% `&` 符号被用于标记等号对齐的位置，即等号对齐。
% `\\` 用于表示换行，将不同的等式放在不同的行上。```

```latex
%行文间直接使用 \[ 和 \] 进行公式编辑 
\[x=\left[x_0\quad	x_1\quad	x_2\quad x_3	\quad	...\quad x_n \right]^T\]
	
\[\theta=\left[\theta_0\quad	\theta_1\quad	\theta_2\quad \theta_3	\quad	...\quad \theta_n \right]^T\]
```
### 表格

LaTeX中表格的插入较为麻烦，可以直接使用[Create LaTeX tables online – TablesGenerator.com](https://www.tablesgenerator.com/#)来生成。建议使用如下方式：

```tex
\begin{table}[htbp]
    \centering
    \caption{表格标题}
    \begin{tabular}{ccc}
        1 & 2 & 3 \\
        4 & 5 & 6 \\
        7 & 8 & 9
    \end{tabular}
\end{table}
```

```latex
\begin{table}[htbp]
	\centering
	\caption{'my table'}
	\begin{tabular}{|c|c|}
		\hline
		col1 & col2 \\
		\hline
		data1 & data2 \\
		data3 & data4 \\
		\hline
	\end{tabular}
	\label{table}
\end{table}
```
### 三线表

```
\usepackage{booktabs} %三线表格制作
```

```latex
%感觉这个更漂亮一点
\begin{table}[htb]
	\centering
	\begin{tabular}{lcc}
		\toprule
		\textbf{Header 1} & \textbf{Header 2} & \textbf{Header 3} \\
		\midrule
		Row 1, Col 1 & Row 1, Col 2 & Row 1, Col 3 \\
		Row 2, Col 1 & Row 2, Col 2 & Row 2, Col 3 \\
		\bottomrule
	\end{tabular}
	\caption{Example of a Three-line Table}
	\label{tab:example}
\end{table}



```

```latex
\begin{table}[!htbp]
	\begin{center} %表格居中
		\caption{my table} %标题
		\begin{tabular}{cc} %第几个c表示第几列居中 如果多加一列,记得多加一个c
			\toprule
			\multicolumn{1}{m{3cm}}{\centering Symbol}
			&\multicolumn{1}{m{8cm}}{\centering Definition}\\
			%如果要多加一列
			%复制多一行 &\multicolumn{1}{m{8cm}}{\centering name}到 \\ 前面
			\midrule
			$A$&1\\
			$B$&2\\
			$C$&3\\
			\bottomrule
		\end{tabular}
	\end{center}
\end{table}
```

% `lcc` 指定了表格的列格式，其中 `l` 表示左对齐，`c` 表示居中对齐。
% `\toprule`、`\midrule` 和 `\bottomrule` 分别用于绘制表格的顶线、中线和底线，创建了漂亮的三线表格外观。
% `\textbf{}` 用于使表头加粗。
% `\caption{}` 用于添加表格标题。
% `\label{}` 用于添加表格标签，以便在文档中引用表格。

### 附录

```latex
%附录代码
\newpage %附录是要换新页
\begin{lstlisting}[language=matlab,caption={The matlab Source code of Algorithm}]
	matlab();
\end{lstlisting}

```
另外，也可以自定义的样式：`\item`

```tex
\begin{enumerate}
    \item[(1)] 这是第一点; 
    \item[(2)] 这是第二点;
    \item[(3)] 这是第三点. 
\end{enumerate}
```

### 参考文献
不使用BibTex
```latex
%
\newpage 
\begin{thebibliography}{99}  

\bibitem{ref1}郭莉莉,白国君,尹泽成,魏惠芳. “互联网+”背景下沈阳智慧交通系统发展对策建议[A]. 中共沈阳市委、沈阳市人民政府.第十七届沈阳科学学术年会论文集[C].中共沈阳市委、沈阳市人民政府:沈阳市科学技术协会,2020:4.
\bibitem{ref2}陈香敏,魏伟,吴莹. “文化+人工智能”视阈下文化创意产业融合发展实践及路径研究[A]. 中共沈阳市委、沈阳市人民政府.第十七届沈阳科学学术年会论文集[C].中共沈阳市委、沈阳市人民政府:沈阳市科学技术协会,2020:4.
\bibitem{ref3}田晓曦,刘振鹏,彭宝权. 地方高校开展教育人工智能深度融合的路径探究[A]. 中共沈阳市委、沈阳市人民政府.第十七届沈阳科学学术年会论文集[C].中共沈阳市委、沈阳市人民政府:沈阳市科学技术协会,2020:5.
\bibitem{ref4}柏卓君,潘勇,李仲余.彩色多普勒超声在早期胚胎停育诊断中的应用[J].影像研究与医学应用,2020,4(18):129-131.
\bibitem{ref5}杨芸.我院2018年人血白蛋白临床应用调查与分析[J].上海医药,2020,41(17):34-35+74.

\end{thebibliography}
```

```latex
%文中引用文献
\cite{ref1}
\cite{ref1, ref5,...}
```

这种BibTex方法需要建立参考文献数据库，引用的时候调用所需要的参考文献
BibTeX 是一种格式和一个程序，用于协调LaTeX的参考文献处理. **BibTeX** 使用数据库的的方式来管理参考文献. BibTeX 文件的后缀名为 .bib . 先来看一个例子

```latex
@article{name1,
author = {作者, 多个作者用 and 连接},
title = {标题},
journal = {期刊名},
volume = {卷20},
number = {页码},
year = {年份},
abstract = {摘要, 这个主要是引用的时候自己参考的, 这一行不是必须的}
}
@book{name2,
author ="作者",
year="年份2008",
title="书名",
publisher ="出版社名称"
}



- 第一行@article 告诉 BibTeX 这是一个文章类型的参考文献，还有其它格式, 例如 article, book, booklet, conference, inbook, incollection, inproceedings，manual, misc, mastersthesis, phdthesis, proceedings, techreport, unpublished 等等.
- 接下来的"name1"，就是你在正文中应用这个条目的名称.
- 其它就是参考文献里面的具体内容啦
```

在论文最末，`\end{document}`之前，插入如下两行命令：

```latex
\bibliographystyle{plain}
\bibliography{ref}
```

常见的预设样式的可选项有8种，分别是：
```
- plain，按字母的顺序排列，比较次序为作者、年度和标题；
- unsrt，样式同plain，只是按照引用的先后排序；
- alpha，用作者名首字母+年份后两位作标号，以字母顺序排序；
- abbrv，类似plain，将月份全拼改为缩写，更显紧凑；
- ieeetr，国际电气电子工程师协会期刊样式；
- acm，美国计算机学会期刊样式；
- siam，美国工业和应用数学学会期刊样式；
- apalike，美国心理学学会期刊样式；
```
引用文献：
```latex
\cite {引用文章名称}
```


关于引用还有一些差异
```latex
%文中引用

$^{[1]}$ % 这个是小上标

\cite{bib:one,bib:two,...} % 这个是正常标

```
### 定理环境

定理环境需要使用宏包，首先在导言区加入：`amsthm`

```text
\newtheorem{theorem}{定理}[section]
```

其中是环境的名称，设置了该环境显示的名称是“定理”，的作用是让环境在每个section中单独编号。在正文中，用如下方式来加入一条定理：`{theorem}``{定理}``[section]``theorem`

```tex
\begin{theorem}[定理名称]
    这里是定理的内容. 
\end{theorem}
```

其中不是必须的。另外，我们还可以建立新的环境，如果要让新的环境和环境一起计数的话，可以用如下方式：`[定理名称]``theorem`

```tex
\newtheorem{theorem}{定理}[section]
\newtheorem{definition}[theorem]{定义}
\newtheorem{lemma}[theorem]{引理}
\newtheorem{corollary}[theorem]{推论}
\newtheorem{example}[theorem]{例}
\newtheorem{proposition}[theorem]{命题}
```

另外，定理的证明可以直接用环境。`proof`

### 页面

最开始选择文件类型时，我们设置的页面大小是a4paper，除此之外，我们也可以修改页面大小为b5paper等等。

一般情况下，LaTeX默认的页边距很大，为了让每一页显示的内容更多一些，我们可以使用宏包，并在导言区加入以下代码：`geometry`

```tex
\usepackage{geometry}
\geometry{left=2.54cm, right=2.54cm, top=3.18cm, bottom=3.18cm}
```

另外，为了设置行间距，可以使用如下代码：

```tex
\linespread{1.5}
```

### 页码

默认的页码编码方式是阿拉伯数字，用户也可以自己设置为小写罗马数字：

```tex
\pagenumbering{roman}
```

另外，表示小写字母，表示大写字母，表示大写罗马数字，表示默认的阿拉伯数字。如果要设置页码的话，可以用如下代码来设置页码从0开始：`aiph``Aiph``Roman``arabic`

```tex
\setcounter{page}{0}
```
### 代码

```latex
\section{XXX--matlab sourcecode}

\begin{lstlisting}[language=matlab]
		function [dist,xiane,xinyuzhi] = variable(info_renwu,i)
		[a,b] = sort(data_distance);
\end{lstlisting}
		
```

```latex
\section{XXX--python sourcecode}

\begin{lstlisting}[language=python]
	function dis = distance(wei1,jing1,wei2,jing2)
	R = 6370;
	%dis = asin(sqrt(sin((wei1-wei2)/2)^2+cos(wei1)*cos(wei2)*sin((jing1-jing2)/2)^2));
	%dis = dis*2*R;
\end{lstlisting}
```


