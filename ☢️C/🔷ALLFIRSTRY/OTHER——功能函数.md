
## ##⏹️数学库math.h
	平方根函数
```c
double sqrt(x) // 返回x的平方根
```
	绝对值函数
```c

int abs(int i)  //返回整型参数i的绝对值 

double cabs(struct complex znum)  //返回复数znum的绝对值  

double fabs(double x)  //返回双精度参数x的绝对值    

long labs(long n)  //返回长整型参数n的绝对值 
```
	幂函数
```c
double pow(x,y) // 表示x的y次方
```
	指数函数
```c
double exp(x) // 表示e的x次方
```
	对数函数
```c
double log(x) //表示以e为底的对数函数即lnx
double log10(x) //表示以10为底的对数函数即lgx
```

## ##⏹️字符函数ctype.h
ctype.h头文件中包含了许多有用的函数，用于判断字符的属性或对字符进行转换。
以下是一些常用的函数：

	-   isalpha(): 判断一个字符是否为字母。
	-   isalnum(): 判断一个字符是否为字母或数字。
	-   isspace(): 判断一个字符是否为空格字符（包括空格、制表符、换行符等）。
	-   islower(): 判断一个字符是否为小写字母。
	-   isupper(): 判断一个字符是否为大写字母。
	-   tolower(): 将一个字符转换为小写字母。
	-   toupper(): 将一个字符转换为大写字母。

除了上述函数外，ctype.h头文件中还包含了许多其他有用的函数，可根据具体需求选择使用。需要注意的是，在使用这些函数之前，需要先包含ctype.h头文件。
## ##⏹️最值定义limits.h/float.h

>`#include<limits.h>`    //整型数的最大最小值定义在该文件下
 `#include <float.h> `//浮点数数的最大最小值定义在该文件下

>`CHAR_MIN`和`CHAR_MAX`分別表示有符號小整型的最小值和最大值
>`UCHAR_MAX`表示無符號小整型的最大值;

>`SHRT_MIN`和`SHRT_MAX`分別表示有符號短整型的最小值和最大值
>`USHRT_MAX`表示無符號短整型的最大值;

>`INT_MIN`和`INT_MAX`分別表示有符號基本整型的最小值和最大值
>`UINT_MAX`表示無符號基本整型的最大值;

>`LONG_MIN`和`LONG_MAX`分別表示有符號長整型的最小值和最大值;
> `ULONG_MAX`表示無符號長整型的最大值.

>`FLT_MIN`和`FLT_MAX`分別表示單精度實數的最小絕對值和最大絕對值;
>`DBL_MIN`和`DBL_MAX`分別表示雙精度實數的最小絕對值和最大絕對值.

## ##⏹️随机函数rand

使用取模运算和加法运算来得到指定区间内的随机数
1.  取模运算将随机数的值限制在指定区间之内，例如`rand()%40`得到一个0到39之间的随机数。
2.  最后加法运算将这个随机数的值移到指定区间内，例如`rand()%40+60`得到一个60到99之间的随机数。
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
    srand(time(NULL));  // 初始化随机数种子
    int random_num = rand() % 100;  // 生成介于0和99之间的随机数
    int random_num=rand()%40+60// 生成介于60到99之间的随机数
    printf("The random number is %d\n", random_num);
    return 0;
}

```
	如果在调用rand()之前没有调用srand()设置种子，那么将使用默认的种子值。默认种子是1，因此如果多次调用rand()，它们将生成相同的序列。这种情况下，不同程序运行时产生的随机数序列也可能是相同的，因为不同程序在没有显式调用srand()的情况下，rand()使用的是同一默认种子。因此，在使用rand()之前应该使用srand()显式设置种子，以避免产生可预测的随机数序列。


## ##⏹️快排函数qsort()
	qsort()函数是C语言标准库中的一个快速排序函数，用于对数组进行排序。
	它的使用方法如下：
```c
void qsort(void *base, size_t nmemb, size_t size, int (*compar)(const void *, const void *))
```
	base是待排序数组的首地址，
	nmemb是待排序数组中元素的个数，
	size是待排序数组中每个元素的大小（以字节为单位），
	compar是一个指向函数的指针，用于确定排序的顺序。
	在这段代码中，首先定义了一个长度为10的整型数组num，然后通过for循环输入了10个整数。
	接着使用qsort()函数对num数组进行降序排列，并通过for循环输出了排列后的结果

----