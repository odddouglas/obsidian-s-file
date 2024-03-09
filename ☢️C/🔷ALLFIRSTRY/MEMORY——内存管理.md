
	局部变量——>栈区
	全局变量——>静态区
	void*空指针可替

## ##⏹️动态存储分配函数malloc
	调用malloc的时候应该利用sizeof计算存储单元内存大小,不要直接写数值
	此外每次申请最好if检查是否申请成功,不成功会返回NULL(0),成功会返回首地址
```c
void *malloc(unsigned size)
```

## ##⏹️动态存储分配函数realloc
	这个内存分配会自动全部初始化为0
	申请不成功会返回NULL,成功会返回首地址
	一定要注意不要数组越界尤其是赋值操作
```c
void *calloc(unsigned n,unsigned size)
```

## ##⏹️动态存储释放函数free
	当一个动态存储空间不再使用,需要立即释放
	如果是ptr是空指针(NULL),则free什么也不做
```c
void free(void *ptr)
```

## ##⏹️动态存储调用函数realloc
	ptr是先前开辟的内存块的指针（也就是malloc或calloc之前申请的那块内存空间，即需要调整大小的内存空间）(觉得太大或者太小)
	size指的是新的字节数，注意不是增加的字节数，而是新开辟的那块内存空间的字节数，返回值为调整之后的内存的起始地址。
```c
void *realloc(void *ptr, unsigned size)
```
## ##⏹️初始化函数memset
	將某一塊内存中的全部設置為指定的值
	s指向要填充的内存地址。即从哪开始改
	c是要被設置的值。即改成什么
	n是要被設置該值的字元數。即总共要改多少
	返回類型是一個指向存儲區s的指標
```c
void *memset(void *s, int c, size_t n); 

```
## ##⏹️关于内存释放以及内存初始化
```c

#include<stdio.h>
//malloc虽然是申请到内存，但是内存里面可能还有一些数据没被清理
//free 可以做到释放内存但是无法做到完全清除内存，只是告诉操作系统该内存地址可以被重新调用了，我已经用完了 
int main()
{
	int *p1=(int*)malloc(sizeof(int));//申请四个字节 
	*p1=0; //这个就是内存初始化 
	printf("%p,%d\n",p1,*p1);
	
	
	int *p2=(int*)malloc(sizeof(int)*10);//申请四个字节，总共申请十次 
	int i;
	for(i=0;i<10;i++) 
	{
		p2[i]=0; //指针和数组之间就是有着某种联系 
	}
	printf("%p,%d\n",p2,*p2);
	
	int *p3=(int*)malloc(sizeof(int)*10);
	memset(p3,0,sizeof(int)*10);
	printf("%p,%d\n",p3,*p3);
	// 第一个参数是要清理的内存首地址，第二个暂时填0，第三个填要清理的字节数 
	
}
```

## ##⏹️关于自定义函数的内存回收的验证
```c
#include<stdio.h>
int* getP() //无参 
{
	int a=1; //该局部变量在执行完getP之后内存会被回收
	//下次再执行其他函数的时候就会使用原先a所处的内存地址，a的值就会被覆盖 
	return &a; //将a的地址返回给getP函数 
}

void show()
{
	int b=0; //单纯给b这个局部变量赋值 ，这个局部变量和a的位置是一样的  
}

int main()
{
	int *p=getP(); //指针p指向getP该指针函数的返回值 
	printf("%d,%d\n",p,*p);//此时是第一次运行参数时的a=1的值
	show(); //运行一次第二个无参函数//此时调用函数所用到的地址已经回收
	printf("%d,%d\n",p,*p);//此时是第一次运行参数时的b=0的值
	//地址相同，但发现不是同一个数据了
	//数据再调用show的时候就被覆盖掉了 
	 
	return 0;
 } 
```


