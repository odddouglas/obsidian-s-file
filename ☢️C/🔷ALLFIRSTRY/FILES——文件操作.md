
## ##⏹️一些文件概念
	程序文件：
		源程序文件（后缀为.c）
		目标文件(window环境后缀是.obj) ,运行时的临时文件
		可执行程序(window环境后缀是.exe)
	数据文件: 
		文件的内容不一定是程序,而是程序运行时需要读写的数据
		例如程序运行中需要从中读取数据的文件,或者输出内容的文件
	文件标识: 文件路径+文件名主干+文件后缀 ex: c:\code\test.txt
## ##⏹️关于文件流stream的理解
	stream和FILE有密切的关系。
	可以把stream理解为一个抽象的数据流，而FILE指针是一个具体的结构体变量
	它包含了一些与stream相关的信息。
	您可以通过FILE指针来操作stream，例如读取或写入数据
	c语言程序,只要运行起来就默认打开了FILE*类型的3个流
	除了二进制的输入输出函数(fread和fwrite)
	其他函数都可以适用于所有流 , 不管是文件流还是屏幕流
```c
stdin-标准输入流(键盘)
stdout-标准输出流(屏幕)
stderr-标准错误流(屏幕)

EOF-end of file(实质上是-1)
fp-文件输出流(文件)
```
	流是一个连续的字节序列，可以来自于文件、键盘、屏幕等设备]
	流的好处是可以把不同的输入或输出源统一为文件的形式，方便程序员操作]
	简单地说，流就是数据从一个地方到另一个地方的通道。
	例如
	你可以把键盘上输入的字符看作是从键盘到程序的流，
	也可以把程序输出到屏幕上的字符看作是从程序到屏幕的流
## ##⏹️文件的打开和关闭以及结构体类型FILE
	数据流(stream):
		每个被使用的文件都在内存中开辟了一个相应的文件信息区,用来存放文件的相关信息
	FILE:(本质上是一个结构体变量,该结构体类型是系统声明的,取名FILE)
		每当打开一个文件时,系统会根据文件的情况自动创建FILE结构的变量
		并填充信息(如文件的名字,文件的状态以及文件当前的位置等等)
		注意,不同c的编译器的FILE结构体类型的内容不完全相同
```c
struct _iobuf 
{
	char *_ptr;
	int_cnt;
	char _base;
	int_flag;
	int_file;
	int_charbuf;
	int_bufsiz;
	char *_tmpfname;
};
typedef struct_iobuf FILE;
```
	使用: 一般通过一个指向FILE的结构体指针来维护FILE类型的变量
	ex:
		当您使用fopen函数打开一个文件时
		您实际上是创建了一个与该文件相关联的stream
		并返回了一个指向该stream的FILE指针。
```c
FILE* pf; //创建了一个文件指针变量,指向了FILE结构体
```
## ##⏹️关于文件的侦错函数perror
```c
void perror(const char *str);
```
	该函数需要传入一个自定义的字符串,当然最好是文件名字符串,这样就可以知道哪里出错了
	str是一个自定义的字符串，会先打印出来，后面再加上错误原因字符串
	错误原因字符串根据全局变量errno的值来决定。
	例如，如果您尝试打开一个不存在的文件
	perror函数会输出类似这样的信息：Error: : No such file or directory。

## ##⏹️文件的打开和关闭
```c
//打开文件
FILE *fopen(const char* filename,const char* mode);
```
	首先传入一个文件名字符串,接着传入文件的打开方式字符串(注意,都是字符串,所以都是双引号)
	第一个文件名字符串其实是相对路径的,也就是同一目录下才可以进行文件操作
	但是该字符串可以改成绝对路径
	如"c:\\code\\test.txt"(这里多加了个\是避免转义)
	此时返回得到一个指向文件结构体的指针变量,需要一个文件指针变量去接收
	如果打开失败,将会返回一个空指针
```c
//关闭文件
int fclose(FILE *stream)
```
	传入一个文件指针变量,指向要关闭的文件即可
	本质上就是关闭了与该文件关联的流，并解除与该文件的关联

| 文件打开方式指示符 | 目的                                    | 如果文件不存在   |
| ------------------ | --------------------------------------- | ---------------- |
| "r"(read)          | 为了输入数据,打开一个已经存在的文本文件 | 出错             |
| "w"(write)                | 为了输出数据,打开一个文本文件           | 建立一个新的文件 |
| "a"(append)          | 向文本文件尾添加数据                    | 建立一个新的文件 |
| "r+"               | 为了读和写,打开一个文本文件             | 出错             |
| "w+"               | 为了读和写,建立一个新的文件             | 建立一个新的文件 |
| "a+"               | 打开一个文件,在文件尾进行读写           | 建立一个新的文件 |

```c
#include<stido.h>

int main()
{
	//打开文件
	FILE *pf = fopen("test.dat","w");
	if(pf==NULL)
	{
		perror("fopen");
		return 1; //给系统返回1表示操作异常
	}
	fclose(pf);
	pf=NULL; //给这个指针赋值为空
}
```

## ##⏹️文件的顺序读写

| 函数名  | 功能                           | 文件打开方式 | 理解图 |
| ------- | ------------------------------ | ------------ | ------ |
| fgetc   | 字符输入函数(输入一个字符)     | 读(r)        | 流->字符       |
| fputc   | 字符输出函数(输出一个字符)     | 写(w)        |  字符->流      |
| fgets   | 文本行输入函数(输入一个字符串) | 读(r)        | 流->字符串       |
| fputs   | 文本行输出函数(输出一个字符串) | 写(w)        |  字符串->流      |
| fscanf  | 格式化输入函数                 | 读(r)        |  流->(ALL)      |
| fprintf | 格式化输出函数                 | 写(w)        |   (ALL)->流     |
| fread   | 二进制输入                     | 读(r)        |   流->数组     |
| fwrite  | 二进制输出                     | 写(w)        |  数组->流      |
|         |                                |              |        |
##### fgetc字符输出函数
```c
int fgetc(FILE *stream)
```
	stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)。
	从一个文件中读取一个字符，返回该字符的ASCII码; 如果遇到文件末尾或出错，返回EOF。
```c
fopen(fp,"r"); // 以读的形式打开文件

int ret=fgetc(fp); // 从(文件流)文件中读取第一个字符
int ret=fgetc(fp); // 从(文件流)文件中读取下一个字符

int ret=fgetc(stdin) // 从(标准输入流)键盘上读取一个字符
```
##### fputc字符输入函数
```c
int fputc(int c,FILE *stream);
```
	第一个参数整型 c 是要输入的字符的ASCII码
	第二个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	向一个文件中输出一个字符，返回该字符的ASCII码; 如果出错，返回EOF
```c
fopen(fp,"w"); // 以写的形式打开文件

fputc('b',fp); // 向(文件流)文件中输出 'b' 这个字符
fputc('i',fp); // 向(文件流)文件中输出 'i' 这个字符
fputc('t',fp); // 向(文件流)文件中输出 't' 这个字符

fputc('b',stdout); // 在(标准输出流)屏幕上输出 'b' 这个字符
```
##### fgets字符串输入函数
```c
char *fgets(char *s, int n, FILE *stream) 
```
	第一个参数 s 是字符数组的首地址
	第二个参数 n 是要读取的最大字符数（包括末尾的’\0’）
	第三个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	从一个文件中读取一行字符串，存入一个字符数组中。
	函数返回字符数组s的地址，如果遇到文件末尾或出错，返回NULL。
```c
fopen(fp,"r"); // 以读的形式打开文件
char arr[100]={0} // 存储字符串的数组

fgets(arr,4,pf) //从(文件流)文件中读取4个字符(自动包括\0),存储到arr数组
fgets(arr,4,stdin) // 从(标准输入流)键盘中读取4个字符(自动包括\0),存储到arr数组
```
##### fputs字符串输出函数
```c
int fputs(const char *s, FILE *stream)
```
	第一个参数 s 是要写入的字符串的首地址
	第二个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)。
	向一个文件中写入一行字符串，不包括末尾的’\n’。
	函数返回一个非负值，如果出错，返回EOF。
```c
fopen(fp,"w"); // 以写的形式打开文件

fputs("abcdef\n",pf); //向(文件流)文件中输出 'abcdef' 字符串
fputs("ghijklmn",pf); // 向(文件流)文件中输出 'ghijklmn' 字符串

fputs("abcdefghijklmn",stdout) // 在(标准输出流)屏幕上输出 'abcdefghijklmn' 
```
##### fscanf格式化输入函数
```c
int fscanf(FILE *stream, const char *format, … )
```
	从一个文件中按照格式化输入数据，类似于scanf函数。
	第一个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	第二个参数 format 是格式控制字符串，后面跟着若干个指针参数，用来接收输入的数据。
	函数返回成功匹配和赋值的项数，如果遇到文件末尾或出错，返回EOF。
```c
struct student
{
	char arr[10];
	int num;
	float score;
}
struct S stu={"abc", 10 , 5.5f};
//这里用一个结构体变量来展示格式化输入的万能
fscanf(stdin,"%c,%d,%f",&(s.arr),&(s.num),&(s.score));
//这个流变成标准输入流时,上下等价
scanf("%c,%d,%f",&(s.arr),&(s.num),&(s.score))

```
##### fprintf格式化输出函数
```c
int fprintf(FILE *stream, const char *format, ...)
```
	第一个参数是stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	第二个参数 format是格式控制字符串，
	它会把format指定的格式字符串和后面的可变参数列表按照格式输出到stream指定的流中
	向一个流中按照指定的格式输出数据,并返回输出的字符数。如果发生错误，它会返回负值

```c
fprintf(stdout, "%d + %d = %d\n", a, b, a + b); 
// 表示把a、b和a+b的值按照整数加法的形式输出到标准输出中。其实就是打印

struct student 
{
	char arr[10];
	int num;
	float score;
}
struct S stu={"abc", 10 , 5.5f};
//这里用一个结构体变量来展示格式化输出的万能
fprintf(stdout,"%c,%d,%f",(s.arr),(s.num),(s.score));
//这个流变成标准输出流时,上下等价
printf("%c,%d,%f",(s.arr),(s.num),(s.score))

```
##### fread二进制输入函数
```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
```
	第一个参数 ptr 是要写入数据块的首地址(一般都为数组名)
	第二个参数 size 是每个数据块的字节数(数组字节数)
	第三个参数 nmemb 是要写入数据块的个数(数组元素个数)
	第四个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	它会从stream指定的流中读取nmemb个大小为size字节的数据块
	并写存储到ptr所指的地址,并返回实际读取到的数据块数
	如果遇到文件结束或错误，它会返回小于nmemb或0
```c
fread(buffer, sizeof(int), 10, fp);
//表示从fp指向的文件中读取10个整数，并保存到buffer数组中。
```
##### fwrite二进制输出函数
```c
size_t fwrite(const void *ptr,size_t size,size_t nmemb,FILE *stream)
```
	第一个参数 ptr 是要写入数据块的首地址(一般都是数组名)
	第二个参数 size 是每个数据块的字节数(数组字节数)
	第三个参数 nmemb 是要写入数据块的个数(数组元素个数)
	第四个参数 stream 是一个文件结构体类型指针,指向流(文件,键盘,屏幕)
	它会把ptr指向的内存区域中nmemb个大小为size字节的数据块
	写入到stream指定的流中,并返回实际写入到的数据块数。
	向一个文件中写入指定数量和大小的数据块,如果发生错误，它会返回小于nmemb或0
```c
fwrite(buffer, sizeof(int), 10, fp);
// 表示把buffer数组中10个整数写入到fp指向的文件中
```
