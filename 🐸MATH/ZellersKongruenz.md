## C语言——蔡勒公式的使用

##### 蔡勒公式简介：

蔡勒（Zeller）公式，是一个计算星期的公式，随便给一个日期，就能用这个公式推算出是星期几。  
**计算公式：**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019113020053977.png)  
**核心公式：**

```c
w=(y+[y/4]+[c/4]-2c+[26(m+1)/10]+d-1)%7
```

|符号|说明|
|---|---|
|w|星期，0-星期日，1-星期一，2-星期二，3-星期三，4-星期四，5-星期五，6-星期六|
|c|年份前两位|
|y|年（年份后两位|
|m|月（`在蔡勒公式中，某年的1、2月要看作上一年的13、14月来计算，比如2019年1月1日要看作2018年的13月1日来计算`）|
|d|日|
|[ ]|代表取整，即只要整数部分。|

==注意==：1.以上公式只适合于1582年10月15日之后的情形（当时的罗马教皇将恺撒大帝制订的儒略历修改成格里历，即今天使用的公历）  
2.由于编程中数据都使用的是`int`整数类型，所以 `[ ]`可以直接去掉，整除直接使用`/`就行。  
**1、输入一个日期，计算出是这一天是星期几？**

```c
#include <stdio.h>
int main() {
	int Date,year,m,d,w,y,c;
	printf("请输入一个日期(eg:20191130):");
	scanf("%d",&Date);
	year=Date/10000;
	m=Date%10000/100;
	d=Date%100;
	y=year%100;
	c=year/100;
	if(m==1||m==2){
		m+=12;
		y--;
	}
	w=(y+y/4+c/4-2*c+26*(m+1)/10+d-1)%7;
	printf("%d\n",w);
	switch(w){
	case 0:printf("星期日\n");break;
	
	case 1:printf("星期一\n");break;

	case 2:printf("星期二\n");break;

	case 3:printf("星期三\n");break;

	case 4:printf("星期四\n");break;

	case 5:printf("星期五\n");break;

	case 6:printf("星期六\n");break;	
	}
	return 0;
}

```

**2、打印出当月的日历，例如：2019年11月日历**

```c
#include <stdio.h>
int main() {

	int Date,year,m,d,y,c,days,i;
	int week_day1,num=0;
	printf("请输入一个日期(eg:201911):");
	scanf("%d",&Date);
	year=Date/100;
	m=Date%100;
	y=year%100;
	c=year/100;
	switch(m){
	case 4:
	case 6:
	case 9:
	case 11:
		days=30;
		break;
	case 2:
		if(year%4==0&&year%100!=0||year%400==0){
			days=29;
			break;
		}else{
			days=28;
			break;
		}
	default:{
			days=31;
			break;
			}
	}
	if(m==1||m==2){
		m+=12;
		y-=1;
	}
	week_day1=(y+y/4+c/4-2*c+26*(m+1)/10+1-1)%7;  //计算出每月的第一天是星期几
//	printf("%d\n",week_day1);
	printf("日 一 二 三 四 五 六\n");   //或将空格换成 \t
	for(i=1;i<=days;i++){
		++num;
		if(week_day1){     //将每月第一天对应星期之前空出的星期用空字符补齐
			printf("   ");  //此处也可以使用\t进行制表，但注意前后要结构统一。
			--week_day1;
			--i;
		}else{
			printf("%2d ",i);  //或将空格换成 \t
		}
		if(num%7==0)
			printf("\n");
	}
	printf("\n");
	return 0;
}
```

`其他都正常，为什么输入201903就会爆炸呢？？？`

#### 错误原因

经过努力终于找到原因了：  
原来三月的`week_day1`的值不正常，将其值打印出后发现他的值是 `-6~0` .  
解决办法就是再`week_day1`值计算出后加一个`if`判断，如下所示：

```
	week_day1=(y+y/4+c/4-2*c+26*(m+1)/10+1-1)%7;
	if(m==3)
		week_day1+=7;
	//printf("%d\n",week_day1);
```

结果图奉上：  
![](https://img-blog.csdnimg.cn/20191201151923757.png)  
![](https://img-blog.csdnimg.cn/20191201151947131.png)  