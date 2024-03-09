
## ##⏹️枚举
	从前需要定义一些常量符号如(red是1,yellow是2)
	需要独立的 const int 来定义,事实上可以用枚举
	当我们去表示颜色的时候,就可以直接使用red和yellow这些名字且其都带着整型值
```c
enum 枚举类型名字 { 名字1,名字2,名字3} ;
enum colors {red,yellow,green} ;//依次自动赋值0,1,2...
enum colors {red=2,yellow=4,green=6} ;//可以手动赋值
```
## ##⏹️自动计数的枚举
	最后一个会自动赋值,恰好就是枚举元素的总数,
	这样需要遍历所有的枚举量或者需要建立一个用枚举量做下标的数组就很方便了 
```c
#include<stdio.h>

enum COLOR {red,yellow,green,colorcount} ;
//colorcount自动赋值3,恰好就是总共的颜色数量

int main()
{
	int color = -1; //定义一个空字符
	char *COLORNAME[colorcount] = {"red","yellow","green"}
	//定义了个指针数组一个单位一个字符串,共三个单位
	char *colorname = NULL; //定义一个空指针用来存字符串

	printf("输入颜色代码:");
	scanf("%d",&color);

	if(color >= 0 && color <= colorcount)
	{
		colorname= COLORNAME[color];//指针赋值
	}
	else colorname = "unknown";//指向字符串unknown

	printf("你喜欢的颜色是%s\n",colorname);//字符串指针输出
}
```


## ##⏹️结构体变量的定义

	表示一个数据需要一个变量,而变量都有类型
	当表示"时间"的数据的时候,需要很多变量如年月日,对应了三个数据
	此时就需要结构体
	这个结构体可以声明在main外部变成全局变量
	如果声明在函数内部就是局部变量了
```c
struct student 
{//这个point可以不写,这是结构体名称
	char name[20];
	int age;
}stu1,stu2; 

int main()
{
	struct student stu1={iota,19},stu2={kappa,18};
	stu1=stu2; 
	stu1.age = stu2.age;//整型直接运算
	strcpy(stu1.name,stu2.name);//字符串需要函数运算
}
```
	结构体赋值:同类型时可以进行直接赋值(此时就是整个结构里的多个数据进行了赋值)
	结构体数组成员的指针运算要注意字符串需要函数而非直接运算,当然整型就可以

## ##⏹️结构体初始化

```c
#include<stdio.h>
#include<string.h>

struct empolyee//把多个相关的数据打包在一起 
{
	char name[20];
	int age;
	int height;
	int id;
	int salary; //排列成员 
}; //这里没有变量的定义 


struct boss
{
	char name[20];
	int age;
}	b1,b2;//在这里就是定义了全局变量了 



int main() 
{
	struct empolyee e1,e2,e3={"kappa",18,100,528,8000}; 
	//在这里就是定义了局部变量了//e3是一次性初始化了 
	struct boss b1={"odddouglas",19}; //变量初始化
	printf("【empolyee】\n\n") ;
	printf("姓名  %s\n年龄  %d\n体重  %d\nID   %d\n工资   %d\n",e3.name, e3.age, e3.height, e3.id, e3.salary); 
	//变量名后面加点好即可得到相关字段数据 
	e2.age=20;//整型可以直接赋值
	strcpy(e2.name,"iota");//字符需要调用string库来进行copy 
	printf("\n【boss】\n\n") ;
	b2=b1;//结构体复制（同类型的才可以） 
	printf("年龄  %d\n姓名  %s",b2.age,b2.name); 
	 
}

```

## ##⏹️结构数组
```c
struct DATE 
{
	int year;
	int month;
	int day;
};
struct DATE dates[100];
struct DATE dates[]={{2003,6,13},{2003,11,28}};\
for(i=0;i<2;i++)
printf("%d年%d%月d日",date[i].year,date[i].month,date[i].day);
```

## ##⏹️结构中的结构
```c
struct DATE{int year;int month;int day;};
struct TIME{int hour;int minute;int second;};
struct DATEandTIME
{
	struct DATE date;
	struct TIME time;
}a,*p;
printf("%d",a.date.year); //结构嵌套 
p = &a;
printf("%d",p->time.hour); //结构嵌套
printf("%d",a.time.hour);//上下等价
```


## ##⏹️结构体输入函数之结构变量指针
```c
var.成员
ptr->成员
(*ptr).成员
```
	这里展示一个指针的等价关系
```c
#include<stdio.h>

struct letsgo
{
	int i;
	char j;
	double k;
	char m[20];
	//没看到有定义整型数组的 
} a1,a2;


int main()
{
	struct letsgo a1={1,'1',1,"111"};
	struct letsgo *pa1=&a1;//定义一个结构体指针变量指向结构体 
	
	printf("%d,%c,%.0lf,%s\n",(*pa1).i,(*pa1).j,(*pa1).k,(*pa1).m);
	printf("%d,%c,%.0lf,%s\n",pa1->i,pa1->j,pa1->k,pa1->m); 
	//上下两句是等价的 
	
}
```
	展示如何传入指针后在函数内部达到输入的效果
```c
struct score {int x,int y};
struct score* getStruct(struct score *p);
void output(struct score a);
void print(const struct score *p);

int main(int argc， char const *argv[])
{
	struct point a ={0,0};
	getStruct(&a);
	output(a); //此时传入的指针已经达到输入并传出的效果了
	output(*getStruct(&a));//因为函数getStruct的返回值是一个指针
}

struct score* getStruct(struct score *p)
{//定义一个结构指针类型的函数,参数也是个结构指针
	scanf("%d",&p->x);
	scanf("%d",&p->y); 
	//此时传入的结构变量a的两个数据已经在函数内部实现输入了
	//函数外部的a已经被改变了
	printf("%d,%d",p->x,p->y);
	return p;//这里返回一下指针是为了展示那个output参数里读函数返回的指针
}

void output(struct score p)
{
	printf("%d,%d",p.x,p.y);
}
```

## ##⏹️结构体输入函数之结构拷贝取等

```c
#include <stdio.h>
struct point 
{
int x;int y; 
};

void getStruct(struct point p);
void output(struct point p);

void main( )
{
	struct point a = {0,0}; //a的两个数据x,y分别都是0
	getStruct(a); //调用函数输入结构体变量的两个数据
	outPut(a); //调用函数输出结构体变量的两个数据以检验
}

void getStruct(struct point p) 
//传入了结构体变量,但注意这里不同于数组,数组会退化成指针,但这里不是指针
{
	scanf("%d",&p.x);
	scanf("%d",&p.y);
	printf("%d, %d", p.x, p.y);//这里可以提前打印出来
}//传入的这个结构变量只是克隆,故此处的格式化输入,不管输入啥
//都并没有改变原先那个a的值,a的两个数据仍是0
void output(struct point p) 
{
	printf("%d, %d", p.x, p.y); //因此这里传入的a还是0,故输出的还是0
}

```

	我们可以在这个输入函数中,创建一个临时的结构变量,然后把这个结构变量返回
```c
#include <stdio.h>
struct point 
{
	int x;
	int y;
};
struct point getStruct(void); //返回值是结构变量的函数
void output(struct point);

int main()
{
	struct point a = {0,0}; //初始化一个结构变量
	a = getStruct(); //这个getStruct的功能就是输入,这里是一个结构取等
	output(a);//这个output的功能单纯输出
}

struct point getStruct(void)
{
	struct point p; //在这个函数里实现结构变量两个数据的输入
	scanf("%d",&p.x);
	scanf("%d",&p.y);
	printf("%d,%d\n",p.x,p.y);
	return p;//这里把结构变量传回即可,此时传回来了两个数据
}

void output(struct point a)
{
	printf("%d,%d",a.x,a.y);
}
```


----