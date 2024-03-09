
----
## [ swap ] 变量交换
	利用指针进行两个变量的无中间量交换
```c 
void swap(int *pa,int *pb) 
{
	int t = *pa;
	*pa = *pb;
	*pb = t;
}
```
## [ traverse ] 数组遍历打印_p++的运用
	这里注意那个字符数组里的0它就是字符0,也就是'0''\0'
	倘若你的字符数组里的0放在很前面 那就是代表这个字符串结束的很前面
	这个字符0一定代表着字符串的结束,如果有’\0’字符，那么循环就会提前结束。
	第二个while循环，条件是非‘\0',因此字符数组(字符串)里要注意'\0'的位置
	p++的while运用同时可以实现指针移动到数组尾端

	在traverse_FOR函数中，你用sizeof(a)/sizeof(a[0])来计算数组的长度
	但是这样只能得到指针a的大小除以char类型的大小，而不是数组的实际元素个数。
	你可以用一个参数来传递数组的长度，或者用一个特殊字符来标记数组的结尾。
```c
#include<stdio.h>

void traverse_all(char a[],int len)
//该函数需要多传入一个参数来表示即将打印的数组的长度参数
//该函数优点就是可以打印出数组内的所有元素
{
	for(i=0;i<len;i++)
	{
		printf("%d\n",a[i]);
	}
}


void traverse_str(char *p) 
//该函数的缺点是字符串必须以0结尾才能使用，要不然难以判定结束
//所以该函数专门应用于字符串打印
{
	while(*p!='\0')//不用初值，也不用p++,直接来个while 
	{
		printf("%d\n",*p++);//取出p所指的数据，之后顺便将p移到下一个位置
		//*的优先级虽然高，但是没有++高； 
	}
}


int main()
{
	char a[]={1,2,3,4,5,6,7,8,9};char *p = &a[0];//char *p=a;
	int len=sizeof(a)/sizeof(a[0]);
	//如果是char a[]={0,1,2,3,4},此时'\0'在首位,
	//则while遍历不会显示出来 但是for遍历仍然正常显示
	traverse_FOR(a,len);
	traverse_WHILE(a);
	return 0; 
}
```

## [ store_str ] 使用getchar存入字符数组 
#一些待解决的questions
	存入数组后续可通过遍历打印
	暂时不知道如何封装成函数
```c
#include<stdio.h>
int main()
{
	char a[128];int i=0;
	
	/////////////////////////
	a[i]=getchar(); 
	while(a[i]!='\n')
	{
		i++;
		a[i]=getchar();
	}
	a[i]='\0';
	
	////////////////////////
	/*
	按理来说这样写也是没有问题的，但不知道为啥就是跑不出来，明明起初i是0
	好像是在循环里跑不出来了，一定要问个明白到时候

	while(a[i]!='\n')
	{
		a[i]=getchar();
		i++;
	}
	a[i]='\0';
	
	*/
	printf("%s",a);
 } 
```
## [ printstr ] 直接输出字符_打多少输多少 
```c
#include<stdio.h>
int main( )
{
    int ch;
    while( (ch=getchar())!= '\n' )  
    //从控制台流中读取字符，直到按回车键结束
    printf ("%c", ch);  //输出读取内容
}
```

```c
#include<stdio.h>
int main()
{
	FILE* fp=NULL;
	char arr[10]={0};
	fgets(arr,4,stdin); // 从(标准输入流)键盘中读取4个字符(自动包括\0),存储到arr数组
	printf("%s",arr);
}
```
## [ store_by_mem ] 动态分配内存来储存字符串
	//定义一个函数，用来从标准输入读取一段未知长度的字符串
	//并返回一个指向动态分配内存的指针
	大概思路就是用if判断语句来判定长度,用realloc来重新申请更多的内存
	记得再main函数里free掉申请的内存
	好像有现成的封装函数strdup和getline,以后再说hh
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char *stor_by_mem( ) //无参
{
	char *str; //定义一个字符指针
	int len = 10; //初始长度为10
	str = (char *)malloc(len * sizeof(char)); 
	//用malloc申请10个字符大小的内存
	if (str == NULL) 
	{ //检查是否申请成功
	    printf("Memory allocation failed.\n");
	    return NULL;
	}
	int i = 0; //记录输入字符个数
	int c; //用一个int变量来接收getchar()返回值
	while ((c = getchar()) != EOF && c != '\n') 
	{ //循环读取输入字符直到文件结束符或换行符
		str[i] = c; //把字符赋值给str[i]
		i++;
		if (i == len) 
	    { //如果输入字符个数达到当前长度
			len *= 2; //把长度扩大两倍
		    str = (char *)realloc(str, len * sizeof(char)); 
		    //用realloc重新调整内存大小
		    if (str == NULL) 
		    { //检查是否调整成功
		        printf("Memory allocation failed.\n");
		        return NULL;
		    }
	    }
  }
  str[i] = '\0'; //在末尾加上空字符
  return str; //返回指针
}

```

## [ get_mem] 自定函数内的变量通过内存保留
```c

#include<stdio.h>//方法一 
#include<stdlib.h>
int* get_mem()//这个函数的作用就是申请一个内存，他这里还往这个内存里放了数据 
{
	int *p=(int*)malloc(size(int));//申请一个堆内存 
	*p=100; //往申请好的地址内存放数据 
	return p; //将指向申请好的堆内存地址返回 
 } 
void main()
{
	int *p=getMemory(); //将该指针指向为申请好指针的内存地址 
	printf("%d\n",*p);//读取p的内存并输出 //想咋用咋用 
	
	free(p);//打印完(用完)就回收 //该方法的申请内存和释放内存由两个函数实现 
 } 
```

```c
#include<stdio.h> //该方法适合单线程 

int* get_mem() //还是个无参，这样在主函数内使用的时候就不用传入参数 
{
	static int a=100;
	//使用static变量来修饰函数内部局部变量，静态变量的值会被保留起来 
	
	return &a;//将a的内存地址返回 
}
void main()
{
	int *p=get_mem();
	printf("%d",*p); 
 } 
```

```c

#include<stdio.h>
#include<stdlib.h>
void get_mem(int *p)//该函数只负责赋值 
{
	*p=100; //注意是直接修改内存的，所以即使在函数内部也能实现永久赋值 
}

void main()
{
	int *p=(int*)malloc(sizeof(int));
	fun(p);
	printf("%d",*p);
	free(p);
 } 

```





## [ str_copy ] 字符拷贝到申请好的内存地址
	使用strchr寻找到指定字符后,利用strlen获取数组内存空间并用strcpy拷贝过去

[[ARRAY——字符串数组#字符型数组strcpy函数|strcpy]] & [[ARRAY——字符串数组#字符型数组strchr函数|strchr]] & [[ARRAY——字符串数组#字符串的长度计算|strlen]]
```c
int main()
{
	char str = "hello,world"
	char *src = strchr(str,',')//返回字符串".world"
	char *dst = (char *)malloc(strlen(src)+1);
	//申请一个跟数组src一样大的内存空间,首地址是dst
	strcpy(dst,++src);//src加一就是从'.'之后的"world"开始拷贝
	printf("%s\n",dst);//打印world
	free(dst);//把申请到的内存空间释放掉
}
```
