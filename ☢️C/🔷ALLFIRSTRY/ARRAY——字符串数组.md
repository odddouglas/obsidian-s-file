
## ##⏹️字符数数组的一些概念

- 严格意义上的字符数组
	加上了'\0'之后就可以变成c语言严格意义上的字符串
	才可以进行直接的字符运算
```c
char word[]={'H','e','l','l','o','!'}
char word[]={'H','e','l','l','o','!','\0'}
```
- 字符串以数组的形式存在,以数组或者指针的形式访问(多以指针形式访问)
	string.h中有很多处理字符串的函数

## ##⏹️引用字符串得两种方式
```c
char str[] = "Hello";  //开辟内存直接初始化数组  
char str[] = {1,2,3,4,5,0} //这个0表示结束,如果写在前面就结束在前面了
char *str = "Hello";   //定义指针指向某一个不可写空间内的连续字节
//只有当char*所指的字符数组有结尾字符0,才能说所指的是字符串(int*同理)
const char *str = "Hello"; //其实上下两个是等价的 这个const是常量标志
printf("%s\n",str);   
printf("%c",str[2]); 
printf("%p",str); 
//指针初始化的字符串>其字符数组首地址很小(不可写)
//数组初始化的字符串>其字符数组首地址很大(可写)
```
- 数组直观且能直接操作(构造)
	但作为本地变量空间自动会被回收
- 指针的字符串位置未知无法操作(处理)
	处理参数(将字符串传入函数)
	动态分配空间(该字符串内存空间由内存函数调用)

## ##⏹️数组长度作为参数传入函数
```c
sizeof(指针)=指针的字节数(4字节)
sizeof(数组名)=数组的字节数
```
- 数组名作为参数的时候会退化成指针
- 在计算数组长度的时候 必须在函数外部算好之后想办法传进去

## ##⏹️关于字符串连续打印
```c
printf("Hello"    //这里的两个字符串会以字符数组的形式连续存放在某处
	   "world"); //这里可以直接换行的且不会读取两对双引号之间的符号(空格)
printf("Hello/world"); //这里也是连续打印但注意反斜杠后的东西会严格打印
```

## ##⏹️字符串赋值
```c
char *t = "Hello,World!";
char *s;
s = t;
//这里并没有产生新的字符串
//只是指针s指向了t所指的字符串
//此后对s的任何操作就是对t做了
```

## ##⏹️字符串的输入输出
- scanf时在%和s之间的数字应该要比数组的大小小一,表示最多允许读入的字符数量
```c
char word[8];
scanf("%s",word); //这里的scanf读入一个单词(到空格tab回车为止)
scanf("%7s",word); //这里避免数组越界可以限制读入的单词字母个数(这里设7)
printf("%s",word)
```
一般來說，我們在列印的時候使預設左對齊的
（1）左對齊
如果在%後加一個“-”標誌就表示左對齊，例如
```c 
printf(“%-8s\n”,a);
```
這裡表示我們要列印的數位寬度為8，如果要列印的位數小於8，則在後面補足空格; 如果要列印的位數大於8，則列印所有的數位，不會截斷。
（2）右對齊
```c
printf(“%8s”,a);
```
如果在%和d之間直接加上數位寬度，就表示。
這裏我們表示數位寬度為8，如果要列印的位數小於8，左邊補足空格; 如果要列印的位數大於8，則列印所有的數位，不會截斷。

## ##⏹️空字符串
```c
char buffer[100] = ""; //这是一个空字符串,buffer[0]=='\0'
char buffer[] = ""; //注意这样写,空字符串的长度只有1
char *buffer;  //以为char*就是定义字符串类型，其实只是定义了一个指针
//且指针buffer尚未初始化为0，也尚未初始化指向哪里，要注意
```

## ##⏹️字符串数组(二维数组和指针字符数组)
- 存放字符串的数组
![[Pasted image 20221227152256.png]]
![[Pasted image 20221227152251.png]]
```c
char a[][10]={"Hello","World","isdjiasdjasdsa"};
//前面括号表示行，后面括号表示列，此时列意思是字符的长度、
//像第三个就超过了10，此时出现数组越界
//此时a[0]就是“Hello”,a[1]就是"World"
char *a[]={"Hello","World","isdjiasdjasdsa"};
//这里的就是放着指向字符串的指针
```




## ##⏹️数组作为参数退化问题以及传入数组长度
	- 这个遍历方法很高级
```c

void PRINT(int *num,int count)//传入数组指针,传入数组长度
{
	int i;
	for(i=0;i<count;i++)
	{
		printf("a[%d]=%d\n\n",i,*(num+i)); //读取首地址往后一位一位递增的值 
	}
}

int main()
{
	int a[10]={1,2,3,4,5,6,7,8,9,0};
	int b=sizeof(a)/sizeof(a[0]);//现在函数外部计算数组长度 
	PRINT(a,b); //传入数组首地址以及数组长度 
	return 0;
}
```

## ##⏹️字符串数组strlen计算函数以及其他计算方法
```c
int strlen (const char *s);
```

```c
#include<stdio.h> 
#include<string.h>
//一些数组长度计算的自定义函数 

int STRLEN1(char *num) //传入数组指针
{
	int i=0;
	while(num[i]!='\0')
	{
		i++;
	}
	return i;
}

int STRLEN2(char *num)//传入数组指针
{
	int i=0;
	while(*num!='\0')
	{
		i++;
		num++; //这里num是指针，指向第一个变成指向下一个 
	}
	return i;
}

int STRLEN3(char *num)//传入数组指针
{
	char *p=num; 	//赋值为首地址 
	while(*p!='\0')  //读值 
	{
		p++; 
	}
	return p-num; //这里是两个位置相减 //妙
}


int main()
{
	char *str="abc"; //char c[]="abc" //char c[]={'a','b','c'}
	
	printf("%d\n",STRLEN1(str));
	printf("%d\n",STRLEN2(str)); 
	printf("%d\n",STRLEN3(str));
	printf("%d\n",strlen(str));
	printf("%d\n",sizeof(str)-1);//这里算上了最后的字符0所以减去一
	return 0;
}
```
## ##⏹️字符型数组str(i)cmp比较函数
```c
int strcmp(const char *s1,const char *s2);
```
	- 0是内容一致，1是前者大于后者，-1是前者小于后者
```c
#include<stdio.h>
#include<string.h> //字符型数组的指针探讨 //以及一些处理函数 

int main()
{
	char *a0="ab";
	char *a1="abc";
	char a2[10]={0};//开辟一个新的内存 
	char *a3=a1;    //定义a3指向a1，此时a3和a1的位置是相等的
	char a4[]="ABC";
	if(a3==a1) printf("YES\n");else printf("NO\n");
	if(a2==a1) printf("YEs\n");else printf("NO\n");
	if(a2==a3) printf("YES\n");else printf("NO\n");
	//此处两个指针变量作比较时，查看的时地址是否相等
	printf("compare a1&a2: 	%d\n",strcmp(a1,a2)); 
	//这里才是真的在比较内容(ACII)
	printf("compare a0&a1: 	%d\n",strcmp(a0,a1)); 
	//0是内容一致，1是前者大于后者，-1是前者小于后者
	printf("compare a4&a1: 	%d\n",stricmp(a4,a1)); 
	//这里是忽略大小写的比较内容 (ignore case compare) 
	return 0;
}
```
	直观的查看这个功能的比较规则
```c
strcmp("sea","sea") == 0;
//sea == sea
strcmp("compute","compare") == 'u'-'a' > 0 
//compute > compare
strcmp("happy","z") == 'h'-'z' < 0
//happy < z
strcmp("sea","seat") == '\0'-'t' <0
//sea < seat
```
## ##⏹️字符型数组strcpy拷贝函数
```c
char* strcpy(char* dst,const char* src);
```
	- 后者拷贝到前者
```c
#include<stdio.h>
#include<string.h>
int main()
{
	char *a1="123";
	char a2[]="321";
	strcpy(a2,a1); //将a1拷贝到a2 //从右到左
	printf("a1=%s %d\n",a1,a1); 
	printf("a2=%s %d\n",a2,a2); 
	//a1=123 4214784
	//a2=123 6487704

	printf("\n");
	a2[0]='4';   
	//不能写a1[0]='4' 因为此时左边是常量 或者说根本没有数组a1出现 
	//能写a2[0]是因为确确实实是个数组,而a1只是个数组指针
	printf("a1=%s %d\n",a1,a1);
	printf("a2=%s %d\n",a2,a2); //此处a2的首字符改变了 
	//a1=123 4214784
	//a2=423 6487704
	
	return 0;
}
```


## ##⏹️字符型数组strtok分割函数
```c
char *strtok(char *str, const char *delimiters);
```
	- `str`：要分割的字符串，第一次调用时传入需要分割的字符串，后续调用传入 NULL。
	- `delimiters`：包含分隔符字符的字符串。`str` 将会被按照 `delimiters` 中的字符进行分割。

	注意: `strtok` 函数在第一次调用时需要传入待分割的字符串 `str`，后续调用传入 NULL。这是因为 `strtok` 使用静态变量来跟踪分割的位置，所以需要在后续调用中传入 NULL 以继续从上一次的位置继续分割。
	分割后的子字符串将会替换原始字符串中的分隔符，而且原始字符串将会被修改。如果需要保留原始字符串，可以在使用 `strtok` 之前创建一个副本。
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[] = "Hello World C Programming";
    char *token = strtok(str, " "); // 使用空格作为分隔符

    while (token != NULL) {
        printf("%s\n", token); // 打印分割出的子字符串
        token = strtok(NULL, " "); // 继续分割下一个子字符串
    }

    return 0;
}

```

----

## ##⏹️字符型数组memcpy区域拷贝函数
```c
void *memcpy(void *dst, void *src, unsigned n);
```
	- 从地址src复制n个字节到地址dst
```c
#include<stdio.h>
#include<string.h>
#include<stdio.h>
#include<string.h>
int main()
{
	char s1[]={"0123456789"};
	char s2[10];
	int s3[]={0,1,2,3,4,5,6,7,8,9};
	int s4[10];
	int i;
	memcpy(s2,s1,10*sizeof(char)); //这里是限定几个字节拷贝出来 
	memcpy(s4,s3,sizeof(s3));  
	//这里就是全部拷贝出来 //这里参照的不是数组长度而是数组总字节数 
	for(i=0;i<10;i++)
	{
		printf("s1=%c\n",s1[i]); //  
		printf("s2=%c\n",s2[i]); 
		printf("s3=%d\n",s3[i]); 
		printf("s4=%d\n",s4[i]); 
		printf("\n");
	}
	
	return 0;
}
```

## ##⏹️字符型数组strcat_追加拷贝函数
```c
char* strcat(char *src, char *dst)
```
	- 把后者拷贝并接到前者后面
	- 把dst的结尾`'\0'`去掉之后开始接
```c
#include<stdio.h>
#include<string.h>
int main()
{
	char s1[]="123";
	char s2[]="456";
	char s3[]="789";
	printf("%s\n",s1); 			  //123
	printf("%s\n",strcat(s1,s2)); //123456
	printf("%s\n",strcat(s1,s3)); //123456789

	return 0;
}

```

## ##⏹️字符型数组strstr比对字符串查询函数
```c
char *strstr( const char *str1, const char *str2 );
char *strcasestr( const char *str1, const char *str2 ); //忽略大小写
```
	- 在一个字符串中找另一个字符串(查找字符串)
	- 函数用于判断字符串str2是否是str1的子串。
	- 如果是，则该函数返回 str1字符串从str2第一次出现的位置开始到str1结尾的字符串；
	(即子字符串在原先字符串中第一次出现的位置)
	- 否则，返回NULL。
```c
#include<stdio.h>
#include<string.h>
int main()
{
	char arr1[] = "aodddef";
	char arr2[] = "oddd";
	char *ret = strstr(arr1, arr2);
	if (ret == NULL)
	{
		printf("不存在\n");
	}
	else
	{
		printf("%s\n", ret);
	}
	return 0;
}

```
## ##⏹️字符型数组strchr比对单字符查询函数
```c
char *strchr( const char *string, int c );  //从左开始找
char *strrchr( const char *string, int c ); //从右开始找
```
	- strchr函数功能为在一个串中从左往右查找给定字符的第一个匹配之处。
	- strrchr函数则是从右边往左边找
	- 即在参数 str 所指向的字符串中搜索第一次出现字符 c（一个无符号字符）的位置。
	- 返回一个指向该字符串中第一次出现的字符的指针，
	- 如果字符串中不包含该字符则返回NULL空指针。
	- 注意，该函数式大小写区分的函数，大小字母和小写字母被看成是不一样的
```c
#include <stdio.h>
#include <string.h>
int main ()
{
   const char str[] = "odddouglas.txt.cpp";
   const char ch = '.';
   char *ret;
  
   ret = strchr(str, ch); //返回字符串odddouglas.txt.cpp中'.'处的指针
   //(作为了字符串txt.cpp的首地址)
   printf("|%c|之后的字符串是|%s|\n",ch,++ret); 
   //加1就是那个字符'.'的后一位开始，不打印这个字符'.'
   ret = strchr(ret, ch); //返回字符串txt.cpp中字符'.'处的指针
   //(作为cpp的首地址)
   printf("|%c|之后的字符串是|%s|\n",ch,++ret);
   return 0;
}

```

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
int main ()
{
   const char str[] = "odddouglas.txt.cpp";
   const char ch = '.';
   char *src = strchr(str,ch);
   char tmp = *src; //这里暂时存一下'.'这个字符
   *src = '\0'; //这里把在str字符串中定位到的'.'换成了'\0'
   char *dst = (char *)malloc(strlen(str)+1);
   //这里申请一个有整个str字符串那么大的内存空间
   strcpy(dst,src);//拷贝src到dst
   printf("%s\n",dst);//从dst开始打印直到src处的'\0'
   //即打印了odddouglas
   *src = tmp;//把值换回来
```

## ##⏹️字符输入输出gets,puts,getchar,putchar
```c
char *gets(char *str);
int puts(int *s);
```
	gets()函数从流中读取字符串,直到出现换行符或读到文件尾为止
	gets可以接收空格；
	而scanf遇到空格、回车和Tab键都会认为输入结束，所有它不能接收空格。
	所以在输入的字符串中包含空格时，应该使用gets输入
	最后加上NULL（‘\0’）作为字符串结束。
	(最后“敲”的换行符会从缓冲区中取出来，然后丢弃，所以缓冲区中不会遗留换行符)
	所读取的字符串暂存在给定的参数str中。
	
	将一个字符串（以‘\0’结束的字符序列）输出到终端
	用puts函数输出的字符串中可以包含转义字符。
	在用puts输出时自动把字符串结束标志′\0′转换成′\n′，即输出完字符串后换行
```c
int getchar(void);
int putchar(int ch);
```
	getchar输入成功,
	返回值为用户输入的第一个字符的ASCII码
	若出错返回-1，且将用户输入的字符回显到屏幕。
	循环语句中如果用户在按回车键之前输入了不只一个字符
	其他字符会保留在键盘缓冲区中，等待后续调用读取。
	也就是说，后续的getchat()调用不会等待用户按键，而是直接读取缓冲区中的字符，
	直到缓冲区的字符读取完毕后，才等待用户按键。
	putchar输出成功，返回该字符的ASCII码值，否则返回EOF

