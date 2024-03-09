## *多文件编译*

在前面我们已经学习了如何编写头文件与源文件，但这还只是停留在单一文件的编译方法上。多数大型的工程的头文件和源文件非常多，我们也不可能把所有的代码都写在同一个文件里，这样也不方便代码的阅读与维护，通常都会根据不同的功能将代码分别书写到多个源文件与头文件中。例如我们可以编写一个简单的日历程序：

#### < ioput.h > 
```c

#include <stdio.h>
//读取用户输入的年份
int input_year(void);
//读取用户输入的月份
int input_month(void);
//显示日历
void output_days(int year, int month, int week, int is_leap_year);


```
#### < ioput.c > 
```c

#include "ioput.h"
//读取用户输入的年份
int input_year(void)
{
	int year;
	printf("Enter the year:");
	scanf("%d", &year);
	return year;
}
//读取用户输入的月份
int input_month(void)
{
	int month;
	printf("Enter the month:");
	scanf("%d", &month);
	return month;
}
//显示日历
void output_days(int year, int month, int week, int is_leap_year)
{
	//月份与星期的名称及每个月的天数
	char* month_name[12] = { "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December" };
	char* week_name[7] = { "Su", "Mo", "Tu", "We", "Th", "Fr", "Sa" };
	int days[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	//闰年二月29天
	if (is_leap_year)
	{
		days[1] = 29;
	}
	printf("\n");
	//显示年、月和星期
	printf("     %s %d\n", month_name[month], year);
	for (int j = 0; j < 7; j++)
	{
		printf("%2s ", week_name[j]);
	}
	printf("\n");
	//显示每月1日前的空白
	for (int i = 0; i < week % 7; i++)
	{
		printf("   ");
	}
	//循环显示日期
	for (int i = 1; i <= days[month]; i++)
	{
		printf("%2d ", i);
		//显示7个数后换行
		if ((i + week) % 7 == 0)
		{
			printf("\n");
		}
	}
	printf("\n\n");
}


```
#### < calc.h >
```c
#include "ioput.h"
//蔡勒公式计算星期，只适合于1582年10月15日之后的日期
int calc_week(int year, int month, int day);
//计算闰年
int calc_leap_year(int year);
//日历核心函数
void calc_core(void);


```
#### < calc.c > 
```c
#include "calc.h"
//蔡勒公式计算星期，只适合于1582年10月15日之后的日期
int calc_week(int year, int month, int day)
{
	if (month <= 2)
	{
		month += 12;
		year--;
	}
	int century = year / 100;
	year %= 100;
	int days = (year + year / 4 + century / 4 - 2 * century + 26 * (month + 1) / 10 + day - 1) % 7;
	while (days < 0)
	{
		days += 7;
	}
	return days;
}
//计算闰年
int calc_leap_year(int year)
{
	if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0))
	{
		return 1;
	}
	return 0;
}
//日历核心函数
void calc_core(void)
{
	do
	{
		int year = input_year();
		if (year <= 1582)
		{
			break;
		}
		int month = input_month();
		if (month <= 0 || month >= 13)
		{
			break;
		}
		int is_leap_year = calc_leap_year(year);
		int week = calc_week(year, month, 1);
		month--;
		output_days(year, month, week, is_leap_year);
	}
	while (1);
}

```
#### < main.c > 
```c
#include "calc.h"
int main(int argc, char *argv[])
{
	calc_core();
	return 0;
}
```

接受用户的输入和输出为一对头文件与源文件ioput.h和ioput.c，
而计算闰年和计算某日期是星期几则在另一对头文件当中calc.h和calc.c，
最后主函数main所在的源文件main.c中包含了这几个相关的头文件，并将这几个源文件一同编译：

```cpp
gcc -o calc main.c ioput.c calc.c
```

gcc将这3个源文件一同编译成一个可执行文件。我们来看看这个文件的运行结果。

```text
Enter the year: 2017
Enter the month: 11

     November 2017
Su Mo Tu We Th Fr Sa 
          1  2  3  4 
 5  6  7  8  9 10 11 
12 13 14 15 16 17 18 
19 20 21 22 23 24 25 
26 27 28 29 30 
```

