
----

# [ POINTER ]
## 文件拓展名指针的运用

```c
#include<stdio.h>//不知道为啥跑不了 //因为while里面的条件写错了。。 
#include<string.h>


void ParseFileName(char *fullNAME,char *fileNAME,char *extNAME) 
{
	char* startP=fullNAME;//将三个指针都指向最开始 
	char* midP=fullNAME;
	char* endP=fullNAME;
	//上次就是while的循环条件写成了==,导致了死循环 所以要特别注意有没有写反
	while(*endP!='\0') //将指针endP指到'\0'  
	{
		endP++;
	} 
	while(*midP!='.')//将指针midP指到'.' 
	{
		midP++;
	}
	memcpy(fileNAME,startP,midP-startP);//拷贝文件名到fileNAME 
	memcpy(extNAME,midP,endP-midP);//拷贝扩展名到extNAME  //用strcpy也可以 
	
	//第一个是要拷贝到的内存地址
	//第二个是拷贝的起点
	//第三个是要拷贝的字节数(直接指针相减即可得到相隔元素) 
	
}

int main() 
{
	char fullNAME[]="oddouglas.txt";
	char fileNAME[20]={0};
	char extNAME[20]={0};
	ParseFileName(fullNAME,fileNAME,extNAME);
	//经过函数的指针操作之后 就可以直接把数组打出来了 
	printf("文件名%s,扩展名%s\n",fileNAME,extNAME);
	
	return 0;
}
```


# [ FILES ]
## 任意个整数求和(kb8-10)
	利用calloc动态申请可以存放n个int数据的内存单元,申请成功就返回了动态内存的首地址,
	将其存放给指针,通过移动指针完成存放数组和运算
	动态内存申请得到的是一个没有名字只有首地址的连续存储空间,
	这就相当于一个无名的一维数组了,但注意得到的首地址需要强制转换成需要存放其地址的指针类型
```c
#include<stdio.h>
#include<stdlib.h>
//page 211
int main()
{
	int n,sum=0,i,*p;
	printf("ENTER n:");scanf("%d",&n);
	//为数组p动态分配n个整数类型大小的空间 
	if((p=(int *)(calloc(n,sizeof(int))))==NULL) 
	{
	//该if语句里已经申请了空间即那个指针的赋值操作 
	//甚至进行了判断内存申请成功与否
	//不过devc里对括号的要求十分严格,特别是在做类型强制转换的时候 
		printf("Not able to allocate memory.\n");exit(1);
	}
 	printf("ENTER %d integers:",n);
 	for(i=0;i<n;i++)
 	{
 		scanf("%d",p+i);//逐个进行数组填充 
	 }
	for(i=0;i<n;i++)
	{
		sum = sum + *(p+i);//计算n个整数的和//指针始终未移动 
	 } 
	printf("The sum is %d\n",sum);
	free(p);//一定要释放掉p指针申请的空间
	
	return 0;
	 
 }
  
``` 



## 计算文档中的总字符个数,以及字母数字的个数
```c
FILE *fp; // 定义一个文件指针 fp，用于打开和关闭文件
int c1,c2,c3; char ch; 
C1=C2=c3=0;
if((fp=fopen("f1.txt","r"))==NULL) // 用只读模式 r 打开文件 f1.txt
//并返回一个文件指针给 fp。如果打开失败，返回 NULL。
{
	printf("%s open error!\n","f1.txt");exit(1);
}
while((ch=fgetc(fp))!=EOF) // 从 fp 指向的文件中循环依次读取一个字符
//并返回它的 ASCII 码。如果到达文件末尾，返回 EOF。
{
	if(ch>='A'&&ch<='z'  ch>= 'a'&&ch<='z')
	//判断 ch 是否为字母，如果是，则 c1 加一。
	c1++;
	else if(ch>='g'&&ch<='9')
	//判断 ch 是否为数字，如果是，则 c2 加一
	c2++;
	else 
	//否则，认为 ch 是其他字符，c3 加一
	c3++;
}
printf("letter=%d,digit=%d,other=%d\n",c1,c2,c3);
fclose(fp); // 关闭 fp 指向的文件
return 0;
```
# [ AKGORITHM ]
## 输入整数按位数隔开
```c
#include<Stdio.h>//输入一个整数 然后按位数一个一个隔开并输出 
 
int main()
{
	int num;int a[128];int temp;int i=0,j;
	scanf("%d",&num);
	while(num!=0)
	{
		temp=num%10;
		a[i]=temp,i++;
		num/=10;
	}
	for(j=i-1;j>=0;j--)
	{
		printf("%d ",a[j]);
	}
	
	return 0;
 } 
```

```c
//位权运算
int main()
{
	int tmp,num,WQ=1;//位权初始先赋值为1 
	scanf("%d",&num);
	tmp=num;//存一下 因为后面分两次while的循环条件来用
	while(tmp>=10)
	{
		WQ*=10;
		tmp/=10;
	 } 
	while(WQ>0)
	{
		printf("%d ",num/WQ); 
		//输出最高位 //123除以位权100 先输出最高位1
		num%=WQ;		
		//舍弃最高位 //接着123求余位权100 剩下23 也就是舍弃最高位1
		WQ/=10; 		
		//此时因为123变成23，位权也就从100变成了10
		//相当于有个进制退化，在十进制里除以10就好了
	}
	return 0; 
 } 
```

## 判断某年某月某日是星期几

```c
#include <stdio.h>

// 函数声明
int determineDayOfWeek();

int main()
{
    printf("请输入一个日期(eg:20191130):");
    int weekdayIndex = determineDayOfWeek();
    printf("%d\n", weekdayIndex);
    return 0;
}

// 封装的函数
int determineDayOfWeek()
{
    int date, year, month, day, week, yearH, yearL;
    scanf("%d", &date);
    year = date / 10000;
    month = date % 10000 / 100;
    day = date % 100;
    yearH = year % 100;
    yearL = year / 100;
    if (month == 1 || month == 2)
    {
        month += 12;
        yearH--;
    }
    week = (yearH + yearH / 4 + yearL / 4 - 2 * yearL + 26 * (month + 1) / 10 + day - 1) % 7;

    return week;
}

```
# [ STR_ARRAY ]
## 字符串压缩(kb8-8)

	输入一个长度小于80的字符串，按规则进行压缩，输出压缩后的字符串
	如果某个字符x连续出现n次,则将n个字符x替换成"nx"的形式;否则保持不变 
```c
#include<stdio.h>
#define MAXLINE 80 
//page205
int main()
{
	char line[MAXLINE];
	//写成char line[MAXLINE]={'0'}是不可以的 
	//如果我在初始化的时候把数组都填上字符0,此时统计重复字符的时候就会把0算上了 
	gets(line);
	zip(line);
	puts(line);
	
	return 0; 
 } 

void zip(char *p)
{
	char *q=p;int n;
	while(*q!='\0')
	{
		n=1;
		while(*p == *(p+n)) //连续统计重复字符 
		{
			n++;//并未移动指针,而只是让首地址和该处往后n位进行比较 
		}
		if(n>=10) //讨论两位数的时候如何将这一个两位数转换成两个字符
		{
			*q++ = (n/10) + '0'; //将个位数转义成字符,顺便移至下一位 
			*q++ = (n%10) + '0'; //将十位数转义成字符,顺便移至下一位 
		} 
		else if(n>=2) //要么执行if要么执行else if//可以不写n<10 
		{
			*q++ = n + '0';//将个位数转义成字符,顺便移至下一位 
		}
		*q++ = *(p+n-1) ;//将重复的字符放到转义之后的n的后面一位,顺便移至下一位
		//注意这里的q指针是用来填充字符用的,下次循环就接着覆盖 
		p += n; //p指针自增,即移到下一个新的字符p=p+n 
		//注意这里的p指针是用来定位第一个字符的,下次循环就是定义下一个新字符的第一位 
	}
	*q='\0';//循环结束的时候这个q指针就是指着最后一个 
}
```

## 寻找最小的字符串(kb8-9)

```c
#include<stdio.h>
#include<string.h>
//page210
int main()
{
	int i,n;
	char STR[80],MIN[80];
	scanf("%d",&n);
	scanf("%s",STR);
	strcpy(MIN,STR);//拷贝第一个STR到MIN里,用来比较 
	for(i=1;i<n;i++)
	{
		scanf("%s",STR);//依次吸收,不同于gets输入,这里空格可以结束一次输入 
		if(strcmp(STR,MIN)<0)
		{
			strcpy(MIN,STR);
		}
	 } 
	printf("%s",MIN); 
 } 
```
## 字符串反转 
	使用指针反转字符串
	- 定义一个字符指针或字符数组，用来存储输入的字符串。
	- 定义两个变量，分别表示字符串的起始位置和结束位置。
	- 使用循环，从两端向中间遍历字符串，每次交换两个位置的字符。
	- 输出反转后的字符串。
```c
#include <stdio.h>
#include <string.h>
// 使用指针反转字符串
void reverseStringByPointer(char *str) 
{
    char *start = str; // 指向字符串起始位置
    char *end = str + strlen(str) - 1; // 指向字符串结束位置
    char temp; // 用来交换字符
    while (start < end) 
    { // 当起始位置小于结束位置时循环
        temp = *start; // 交换两个位置的字符
        *start = *end;
        *end = temp;
        start++; // 起始位置加一
        end--; // 结束位置减一
    }
}
// 使用数组下标反转字符串
void reverseStringByIndex(char str[]) 
{
    int start = 0; // 字符串起始下标
    int end = strlen(str) - 1; // 字符串结束下标
    char temp; // 用来交换字符
    while (start < end) 
    { // 当起始下标小于结束下标时循环
        temp = str[start]; // 交换两个位置的字符
        str[start] = str[end];
        str[end] = temp;
        start++; // 起始下标加一
        end--; // 结束下标减一
    }
}
int main() 
{
    char s1[] = "Hello"; // 定义一个字符数组，存储输入的字符串
    char *s2 = "World"; // 定义一个字符指针，存储输入的字符串

    printf("原始字符串为：%s\n", s1);
    reverseStringByIndex(s1); // 调用函数，使用数组下标反转字符串 
    printf("反转后的字符串为：%s\n", s1);

    printf("原始字符串为：%s\n", s2);
    reverseStringByPointer(s2); // 调用函数，使用指针反转字符串 
    printf("反转后的字符串为：%s\n", s2);

    return 0;
}
```
## 字符串组中查找某一字符串
	遍历查找,int find_blabla (char* a,char* b) 
	分别传入两个数组,一个指针数组,一个字符数组
	a是指针数组,每个元素就是指向某个字符串的首地址,相当于存储了很多字符串
	b是一个简单的字符数组,也就是一个字符串
	函数内进行比对,在a中找到b这样的字符串后
	返回寻找到的数组a里对应的下标,找不到就放回-1
使用**strstr**函数来在一个字符串中查找另一个字符串。[[ARRAY——字符串数组#字符型数组strstr函数|strstr函数介绍]]
```c
#include<string.h>
char *strstr(const char *haystack, const char *needle);
```

```c
int find_str(char* a[], char* b)
{
int i = 0; //用于遍历a
while (a[i] != NULL) //当a[i]不为空时循环
{ 
	if (strstr(a[i], b) != NULL) //如果在a[i]中找到了b
	{             
		return i; //返回下标i
	}
	i++; //否则增加i
}
return -1; //如果遍历完都没有找到，则返回-1
}
```




