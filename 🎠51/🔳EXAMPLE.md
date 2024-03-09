#### 延时函数 

- 如果要执行 `x` 毫秒延时，则将这条语句循环执行 `nms` 次
```c
static void DelayNms(int nms)
{
  unsigned int i, j;
  for (i = 0; i < nms; i++)
  {
    for (j = 0; j < 123; j++)
    {
    }
  }
}
```

# obsidian
#### 位运算以进行字节控制

- 左移效果的速度由延时函数控制，你可以根据需要调整延时时间以获得更快或更慢的左移效果。
- 对P2寄存器的值进行逻辑左移时，就是将P2寄存器中的值向左移动指定的位数, 左移一位意味着所有位向左移动一位，
- 最高位的1被移出，最低位补0。因此，LED灯的显示效果会逐渐向左移动。
- 要记住,右移是 ÷ 2 , 左移是 × 2 

#### 依次和巡回流水灯(按位操作)
```c
sbit LED1 = P1 ^ 0; //定义LED1引脚
sbit LED2 = P1 ^ 1; //定义LED2引脚
sbit LED3 = P1 ^ 2; //定义LED3引脚
sbit LED4 = P1 ^ 3; //定义LED4引脚

void main()
{
    unsigned int tag=1; //选择是依次流水灯还是巡回流水灯
	P1=0; //初始熄灭
    while (!tag)
    {
        LED1 = 1;      //打开LED1
        DelayNms(500); //延迟500ms
        LED1 = 0;      //关闭LED1
        
        LED2 = 1;      //打开LED2
        DelayNms(500); //延迟500ms
        LED2 = 0;      //关闭LED2
        
        LED3 = 1;      //打开LED3
        DelayNms(500); //延迟500ms
        LED3 = 0;      //关闭LED3
        
        LED4 = 1;      //打开LED4
        DelayNms(500); //延迟500ms
        LED4 = 0;      //关闭LED4
    }
	
	while(tag)
	{
		LED1=1; 
		DelayNms(500);
		LED2=1;
		DelayNms(500);
		LED3=1;
		DelayNms(500);
		LED4=1;
		DelayNms(500);
		LED4=0;
		DelayNms(500);
		LED3=0;
		DelayNms(500);
		LED2=0;
		DelayNms(500);
		LED1=0;
		DelayNms(500);
		
	}
	
}
```

#### 依次流水灯(总线操作)
- 这是一个依次流水灯, 也就是这个亮起熄灭, 下一个接着亮起熄灭,一次循环至最后一个
```
// 依次流水灯没有用位运算, 也就是没有用上那个自动左移, 用的是数组

P2 = 0xFE (二进制: 1111 1110)
P2 = 0xFD (二进制: 1111 1101)
P2 = 0xFB (二进制: 1111 1011)
P2 = 0xF7 (二进制: 1111 0111)
P2 = 0xEF (二进制: 1110 1111)
P2 = 0xDF (二进制: 1101 1111)
P2 = 0xBF (二进制: 1011 1111)
P2 = 0x7F (二进制: 0111 1111)

```

```c

#include <reg51.h>
 
unsigned char table[]={0xfe,0xfd,0xfb,0xf7,0xef,0xdf,0xbf,0x7f};//11111110,11111101,11111011.....

void main()
{
	unsigned char i;
	while(1)
	{
	    for(i=0;i<8;i++)
	    {
		    P2=table[i];
		    DelayNms(500);
	    }
    }
}
```

```
// 如果是接地的话, 此时1为点亮 
// 我们可以利用for循环,在for关键字括号内通过乘除实现移动

P2 = 0x01 (二进制: 0000 0001)
P2 = 0x02 (二进制: 0000 0010)
P2 = 0x04 (二进制: 0000 0100)
P2 = 0x08 (二进制: 0000 1000)
P2 = 0x10 (二进制: 0001 0000)
P2 = 0x20 (二进制: 0010 0000)
P2 = 0x40 (二进制: 0100 0000)
P2 = 0x80 (二进制: 1000 0000)
```

```c
void main()
{
    unsigned char i; //定义循环计数变量i
	
	for (P2 = 0x01; P2 <= 0x08; P2*=2) //奇怪,写P2>=1, 也能实现相同功能, 这样一直左移好像也行,会自动跳到下一个字节吗?
	{
        DelayNms(500); //延时500ms
    }
}      
```

#### 持续流水灯(总线操作)

- 这是一个持续流水灯,也就是流过的灯会持续亮起, 直到一次循环结束 也就是最后一个灯熄灭,  全部熄灭再次流水
```
//注意最后一位，可以设0为点亮（LED接电源）

P2 = 0x1E (二进制：0001 1110) 
P2 = 0x3C (二进制：0011 1100)
P2 = 0x78 (二进制：0111 1000)
P2 = 0xF0 (二进制：1111 0000)

```

```c
void main()
{
    unsigned char i; //定义循环计数变量i
    while (1)
    {
	    P2 = 0x1E; //点亮LED1, 其实也是回到原位,这样就是一直循环流水闪烁
	    for (i = 0; i < 4; i++)
	    {
	        DelayNms(500); //延时500ms
	        P2 = P2 << 1;//左移一位
	    }
    }
}      

```


#### 巡回流水灯(总线操作)

- 这是持续流水灯的进阶版, 逐个亮起并保持亮起状态,再从最先亮起的依次熄灭
```
//注意最后一位，可以设0为点亮（LED接电源）

P2 = 0x1E (二进制：0001 1110) 
P2 = 0x3C (二进制：0011 1100)
P2 = 0x78 (二进制：0111 1000)
P2 = 0xF0 (二进制：1111 0000)

//再从最先亮起的依次熄灭

P2 = 0xE1 (二进制：1110 0001) //其实 0000 0001 也无妨,左移的时候, 如果只有四个灯, 左边一半字节是不用关心的(因为只有四个灯)
P2 = 0xC3 (二进制：1100 0011)
P2 = 0x87 (二进制：1000 0111)
P2 = 0x0F (二进制：0000 1111)

```

```c
void main()
{
    unsigned char i; //定义循环计数变量i
    while (1)
    {
	    P2 = 0x1E; //点亮LED1, 其实也是回到原位,这样就是一直循环流水闪烁
	    for (i = 0; i < 4; i++)
	    {
	        DelayNms(500); //延时500ms
	        P2 = P2 << 1;//左移一位
	    }
	    
		P2 = 0xE1; // P2 = 1 也可以,效果一样
		for (i = 0; i < 4; i++)
		{
			DelayNms(500);
			P2 = (P2 << 1) +1; //左移加一
		}
    }
}      

```

#### 独立按键控制LED单次闪烁

- 取反操作 `~`  对于控制开关状态、标志位等非负整数类型的变量是非常有用的。
- 通过按键来操控LED灯, 按一下灯亮, 
```c
sbit KEY1 = P2 ^ 0; //定义按键KEY1引脚
sbit KEY2 = P2 ^ 1; //定义按键KEY2引脚
sbit KEY3 = P2 ^ 2; //定义按键KEY3引脚

sbit LED1 = P1 ^ 0; //定义LED1引脚
sbit LED2 = P1 ^ 1; //定义LED2引脚
sbit LED3 = P1 ^ 2; //定义LED3引脚


void main()
{
  while (1)
  {

//一次按键,此时灯亮并持续亮起,再一次按键将会熄灭

    if (0 == KEY1) //第一次检测到KEY1按键被按下
    {
      DelayNms(50);  //等待约50ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if (0 == KEY1) //第二次检测到KEY1按键被按下
      {
        LED1 = ~LED1; //对LED1状态取反，改变LED1的亮灭状态
        while (0 == KEY1); //等待按键弹起
      }
    }

//一次按键,此时灯亮,一直按一直亮, 按键弹起之后熄灭

    if (0 == KEY2) //第一次检测到KEY2按键被按下
    {
      DelayNms(50);  //等待约80ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if (0 == KEY2) //第二次检测到KEY2按键被按下
      {
        LED2 = ~LED2; //对LED2状态取反，改变LED2的亮灭状态
        while (0 == KEY2); //等待按键弹起
        DelayNms(500); 
	    LED2 = ~LED2; //等待之后, LED2亮灭状态再次取反
      }
    }
  }
}
```

#### 四位数码管(共阴极)动态显示
![[Pasted image 20230811222812.png]]
- 注意，这里用到了两个引脚进行控制，一个是段选控制（P2引脚），一个是位选控制（P0引脚）
- 当按下相应的按键时，数码管会显示对应的数字（0、1、2），并且在按键松开后，数码管的显示会消失。
- 按键的持续按下被用来触发不同的数字持续显示，然后通过循环来等待按键松开。
```c
sbit KEY1 = P2 ^ 0; //定义按键KEY1引脚
sbit KEY2 = P2 ^ 1; //定义按键KEY2引脚
sbit KEY3 = P2 ^ 2; //定义按键KEY3引脚

//P1是位选引脚.到时候直接总线操作即可(也就是字节操作)
//P3是段选引脚,到时候直接按位操作即可(即0和1高低电平切换)

sbit SegmentG1 = P3^0;  //定义数码管1段选引脚
sbit SegmentG2 = P3^1;  //定义数码管2段选引脚
sbit SegmentG3 = P3^2;  //定义数码管3段选引脚
sbit SegmentG4 = P3^3;  //定义数码管4段选引脚

const unsigned char arrNumber[] = {0x3F , 0x06 , 0x5B , 0x4F , 0x66 , 0x6D , 0x7D , 0x07 , 0x7F , 0x6F }; //共阴极
//初始化数码管显示数字0-9

void main()
{
  while (1)
  {
    if (0 == KEY1) //第一次	检测到KEY1按键被按下
    {
      DelayNms(50);  //等待约50ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if (0 == KEY1) //第二次检测到KEY1按键被按下
      {
		SegmentG1=0; //段选引脚直接[ 按位控制 ]开启数码管1
        P1=arrNumber[0];  //位选引脚直接[ 字节控制 ]数字字样
        while (0 == KEY1); //等待按键弹起
      }
	  DelayNms(50);
	  SegmentG1=1; //段选引脚关闭数码管1
    }
  }
}
```
 
#### 按键计数器(四位数)
- 将切换的间隔不断缩小，每位数码管的点亮与熄灭之间的时间间隔缩短至100ms，不用缩的太过小,差不多就行了,这样就有一个同时显示的效果
```c
#include <reg51.h>

sbit Key = P2^0; // 定义按键引脚
sbit SegmentG1 = P3^3; // 定义数码管1的段选引脚
sbit SegmentG2 = P3^2; // 定义数码管2的段选引脚
sbit SegmentG3 = P3^1; // 定义数码管3的段选引脚
sbit SegmentG4 = P3^0; // 定义数码管4的段选引脚

const unsigned char arrNumber[] = {0x3F, 0x06, 0x5B, 0x4F, 0x66, 0x6D, 0x7D, 0x07, 0x7F, 0x6F};


void displayNumber(unsigned int num) {
    SegmentG1 = 0;
    P1 = arrNumber[num % 10];
    DelayNms(100); // 微小延时以消除闪烁
    SegmentG1 = 1;

    SegmentG2 = 0;
    P1 = arrNumber[(num / 10) % 10];
    DelayNms(100);
    SegmentG2 = 1;

    SegmentG3 = 0;
    P1 = arrNumber[(num / 100) % 10];
    DelayNms(100);
    SegmentG3 = 1;

    SegmentG4 = 0;
    P1 = arrNumber[num / 1000];
    DelayNms(100);
    SegmentG4 = 1;
}

void main() {
    unsigned int count = 0; // 计数器

    Key = 1; // 初始化按键引脚为高电平

    while (1) {
        if (Key == 0) { // 按下按键（低电平）
            DelayNms(50); // 按键去抖动延时
            if (Key == 0) {
                count++; // 计数器加一
                while (Key == 0); // 等待按键弹起
                DelayNms(50);
            }
        }

        displayNumber(count ); // 显示计数器的四位数值
    }
}

```

#### 中断反复横跳

- 其实这个会出现一个又亮又暗的反复横跳, 肉眼上是一直亮着的,但是没有一直点着亮.
-  这段代码运行时，LED1会一直保持亮状态，因为在 `while(1)` 循环中，LED1被设置为1。当外部中断0触发时（外部中断0引脚由高电平变为低电平），中断服务函数 `External0_Handler` 被调用，LED1会被熄灭。然后程序会回到 `while(1)` 循环，继续将LED1保持亮，不断重复这个过程
```c

include<reg51.h>

sbit LED1 = P2^4;

void main()
{
	IT0 = 0; //触发方式是外部中断0为低电平
	EX0 = 1; //允许中断
	EA = 1; //总闸打开
	while(1)
	{
		LED1 = 1;
	}
}

void External0_Handler() interrupt 0
{
	LED1 = 0;
}
```

#### 定时器控制流水灯

```c
#include<reg51.h>

int cnt;

void main()
{
	int x;
	TMOD = 0x01; //定时器配置
	
	TH0 = 0x3c; //初值设定
	TL0 = 0xB0;
	
	ET0 = 1; //定时器0允许中断
	EA = 1; //总闸
	TR0 = 1; //定时器0启动

	for(x=0x80; x>=1; x=x/2)
	{
		P1 = x; //总线控制
		while(cnt<20);// 未到达20个50ms,P2不重新赋值,即不进行下一次循环
		cnt=0;
	}
}

void Time0() interrupt 1
{
	cnt++;
	TH0 = 0x3c;
	TL0 = 0xB0; //这个初值设定完, 是65536 - 15536 = 50000μs = 50ms
}
```

#### 外部中断控制按键计数
![[Pasted image 20230811231011.png]]
- P2控制数码管的位选，P0控制数码管的段选
- 通过外部中断来实现左边三位数递增和右边三位数递减显示的效果。外部中断INT0和INT1通过按键触发，使得数码管上的数字可以实时地变化。
```c
#include <reg51.h>

typedef unsigned int u16;
typedef unsigned char u8;

u8 ledData[] =
    {
        //共阴极
        0x3F, //"0"
        0x06, //"1"
        0x5B, //"2"
        0x4F, //"3"
        0x66, //"4"
        0x6D, //"5"
        0x7D, //"6"
        0x07, //"7"
        0x7F, //"8"
        0x6F, //"9"
        0x77, //"A"
        0x7C, //"B"
        0x39, //"C"
        0x5E, //"D"
        0x79, //"E"
        0x71, //"F"
        0x76, //"H"
        0x38, //"L"
        0x37, //"n"
        0x3E, //"u"
        0x73, //"P"
        0x5C, //"o"
        0x40, //"-"
        0x00, //熄灭
};
sbit k1 = P3 ^ 2;  //外部中断INT0引脚
sbit k2 = P3 ^ 3;  //外部中断INT1引脚
sbit LED = P1 ^ 0; //一个指示灯, P2引脚是其 [位选位], P0引脚是其 [段选位]

sbit Segment0 = P2 ^ 0; //数码管第零位
sbit Segment1 = P2 ^ 1; //数码管第一位
sbit Segment2 = P2 ^ 2; //数码管第二位
sbit Segment3 = P2 ^ 3; //数码管第三位
sbit Segment4 = P2 ^ 4; //数码管第四位
sbit Segment5 = P2 ^ 5; //数码管第五位

u16 cntIncr = 1;  //递增计数器
u16 cntDecr = 99; //递减计数器

void delay(u16 t)
{
  while (t--)
  {
  };
}

void INT0Init(void)
{
  IT0 = 1; //中断方式为下降沿触发r
  EX0 = 1; //允许外部中断
  EA = 1;  //总闸
}

void INT1Init(void)
{
  IT1 = 1; //中断方式为下降沿触发
  EX1 = 1; //允许外部中断
  EA = 1;  //总闸
}

void Ldisp(void)
{
  //个位数显示
  Segment5 = 0;
  P0 = ledData[cntDecr % 10];
  delay(100);
  Segment5 = 1; //消影

  //十位数显示
  Segment4 = 0;
  P0 = ledData[cntDecr / 10 % 10];
  delay(100);
  Segment4 = 1; //消影

  //百位数显示
  Segment3 = 0;
  P0 = ledData[cntDecr / 100];
  delay(100);
  Segment3 = 1; //消影
}

void Rdisp(void)
{
  //个位数显示
  Segment2 = 0;
  P0 = ledData[cntIncr % 10];
  delay(100);
  Segment2 = 1; //消影

  //十位数显示
  Segment1 = 0;
  P0 = ledData[cntIncr / 10 % 10];
  delay(100);
  Segment1 = 1; //消影

  //百位数显示
  Segment0 = 0;
  P0 = ledData[cntIncr / 100];
  delay(100);
  Segment0 = 1; //消影
}

void main()
{
  INT0Init();
  INT1Init();
  while (1)
  {
    while (k1 && k2)
    {
      Ldisp();
      Rdisp();
    }
  }
}

void Int0(void) interrupt 0
{
  if (!cntDecr)
    cntDecr = 999; //减到0重新回到999
  cntDecr--;

  LED = ~LED;
  while (!k1)
  {
    Ldisp();
    Rdisp();
  }
  delay(500);
  LED = ~LED;
}

void Int1(void) interrupt 2
{
  cntIncr++;
  LED = ~LED;
  while (!k2)
  {
    Ldisp();
    Rdisp();
  }
  if (cntIncr == 999)
    cntIncr = 0; //加到999重新回到0
  delay(500);
  LED = ~LED;
}

```



####  定时器控制计数器
![[Pasted image 20230905204615.png]]
- 启动定时器之后, 六位数码管前三位从1计数到999
```c
#include "reg51.h"

typedef unsigned int u16;
typedef unsigned char u8;

u8 ledData[] =
{
        //共阴极
        0x3F, //"0"
        0x06, //"1"
        0x5B, //"2"
        0x4F, //"3"
        0x66, //"4"
        0x6D, //"5"
        0x7D, //"6"
        0x07, //"7"
        0x7F, //"8"
        0x6F, //"9"
        0x77, //"A"
        0x7C, //"B"
        0x39, //"C"
        0x5E, //"D"
        0x79, //"E"
        0x71, //"F"
        0x76, //"H"
        0x38, //"L"
        0x37, //"n"
        0x3E, //"u"
        0x73, //"P"
        0x5C, //"o"
        0x40, //"-"
        0x00, //熄灭
};

sbit LED = P1 ^ 0; //一个指示灯, P2引脚是其 [位选位], P0引脚是其 [段选位]

sbit Segment0 = P2 ^ 0; //数码管第零位
sbit Segment1 = P2 ^ 1; //数码管第一位
sbit Segment2 = P2 ^ 2; //数码管第二位
sbit Segment3 = P2 ^ 3; //数码管第三位
sbit Segment4 = P2 ^ 4; //数码管第四位
sbit Segment5 = P2 ^ 5; //数码管第五位

u16 cntIncr = 1;  //递增计数器
u16 cntDecr = 99; //递减计数器

static void delay(int nms)
{
  while(nms--);
}

	void TIMEInit(void)
	{
	    TMOD = 0x01; // 0001 0001
	    // GATE=0即允许TRx启动计数器,
	    // C/T=0即模式选择成定时器,
	    // M1=1,M0=1即选择了"11" 的工作模式
	    TH0 = 0xFC; //高8位
	    TL0 = 0x18; //低8位
	    TH1 = 0xFC;
	    TL1 = 0x18; //这些都是初值，我也不知道咋算的
	    
	    ET0 = 1;    //允许定时器0和定时器1的中断允许标志位
	    ET1 = 1;
	    
	    EA = 1;  //总闸
	    
	    TR0 = 1; //启动定时0和定时器1
	    TR1 = 1;
	}

void Ldisp(void)
{
    //个位数显示
    Segment5 = 0;
    P0 = ledData[cntDecr % 10];
    delay(100);
    Segment5 = 1; //消影

    //十位数显示
    Segment4 = 0;
    P0 = ledData[cntDecr / 10 % 10];
    delay(100);
    Segment4 = 1; //消影

    //百位数显示
    Segment3 = 0;
    P0 = ledData[cntDecr / 100];
    delay(100);
    Segment3 = 1; //消影
	
	if(cntDecr==0) cntDecr=999;
}

void Rdisp(void)
{
    //个位数显示
    Segment2 = 0;
    P0 = ledData[cntIncr % 10];
    delay(100);
    Segment2 = 1; //消影

    //十位数显示
    Segment1 = 0;
    P0 = ledData[cntIncr / 10 % 10];
    delay(100);
    Segment1 = 1; //消影

    //百位数显示
    Segment0 = 0;
    P0 = ledData[cntIncr / 100];
    delay(100);
    Segment0 = 1; //消影
	
	if(cntIncr==999) cntIncr=0;
}

void main()
{
    TIMEInit(); //定时器启动
    while(1)
	{
		 Rdisp();
	}
}

void Time0(void) interrupt 1 //这个定时器用来闪烁LED灯
{
    static u16 tick0; //静态值,每次执行该函数时该值都不会变化
    TH0 = 0xFC;
    TL0 = 0x18;
    tick0++;
    if (tick0 == 1000)
    {
        LED = ~LED; 
        tick0 = 0;
    }
}

void Time1(void) interrupt 3 //这个定时器用来增加计数
{
    static u16 tick1;
    TH0 = 0xFC;
    TL0 = 0x18;
    tick1++;
    if (tick1 == 100) //调整tick1的值，会影响计数增加的速度
    {
        cntIncr++;
        tick1 = 0;
    }
}
```

#### 外部中断控制计时器
- KEY1可以做到暂停计时，再按一次就是开始计时，而KEY2可以重置计时
```c
#include "reg51.h"

typedef unsigned int u16;
typedef unsigned char u8;

u8 ledData[]={0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,0x7f,0x6f,0x77,0x7c,0x39,0x5e,0x79,0x71};//共阴不带小数点0-F段码为

sbit LED = P1 ^ 0; //一个指示灯

sbit KEY1 = P3 ^ 2; //一个按钮用于下降沿触发外部中断0，停止定时器
sbit KEY2 = P3 ^ 3; //一个按钮用于下降沿触发外部中断1, 重置定时器

sbit Segment0 = P2 ^ 0; //数码管第零位, P2引脚是其 [位选位], P0引脚是其 [段选位]
sbit Segment1 = P2 ^ 1; //数码管第一位
sbit Segment2 = P2 ^ 2; //数码管第二位
sbit Segment3 = P2 ^ 3; //数码管第三位
sbit Segment4 = P2 ^ 4; //数码管第四位
sbit Segment5 = P2 ^ 5; //数码管第五位

u16 cntIncr = 1;  //递增计数器
u16 cntDecr = 99; //递减计数器

static void delay(int nms)
{
  while(nms--);
}

void INTInit(void)
{
	IT0 = 0; //或者IT0 = 1 ; 设置外部中断的触发方式是低电平还是高电平
	IT1 = 0;
	
	EX0 = 1; //开 [外部中断0] 的中断允许位
	EX1 = 1;
	
	EA = 1; //开 [总中断] 开关
	
}

void TIMEInit(void)
{
	TMOD = 0x01; // 0001 0001
	// GATE=0即允许TRx启动计数器,
	// C/T=0即模式选择成定时器,
	// M1=1,M0=1即选择了"11" 的工作模式
	TH0 = 0xFC; //高8位
	TL0 = 0x18; //低8位
	ET0 = 1;    //允许定时器0中断允许标志位
	EA = 1;  //总闸
	TR0 = 1; //启动定时0
}

void Rdisp(void)
{
    //个位数显示
    Segment2 = 0;
    P0 = ledData[cntIncr % 10];
    delay(100);
    Segment2 = 1; //消影

    //十位数显示
    Segment1 = 0;
    P0 = ledData[cntIncr / 10 % 10];
    delay(100);
    Segment1 = 1; //消影

    //百位数显示
    Segment0 = 0;
    P0 = ledData[cntIncr / 100];
    delay(100);
    Segment0 = 1; //消影
	
	if(cntIncr==999) cntIncr=0;
}

void main()
{
	INTInit(); //初始化中断
	TIMEInit(); //初始化定时器
	while(1)
	{
		 Rdisp();
	}
}

void Time(void) interrupt 1 //这个定时器用来增加计数
{
    static u16 tick1=0;
	TH0 = 0xFC; //高8位
	TL0 = 0x18; //低8位
    tick1++;
    if (tick1 == 10) //调整tick1的值，会影响计数增加的速度
    {
        cntIncr++;
        tick1 = 0;
    }
}

void Int1(void) interrupt 0
{
	
	LED=~LED; //确定按键被按下, 执行触发事件
	if(0==LED) TR0 = 0;
	if(1==LED) TR0 = 1;
	while( !KEY1 ); //等待弹起

}

void Int2(void) interrupt 2 //重置定时器
{
	
	cntIncr=0;
	while( !KEY2 ); //等待弹起
}

```

#### 简易倒计时
从设定值开始倒计时，到0重置，循环往复
![[Pasted image 20230905164036.png]]
```c
#include <reg51.h>

static short iCnt = 60;
static short jCnt = 60000; //单位是1ms
sbit SEL3 = P2^0;
sbit SEL4 = P2^1;
sbit LED0 = P1^0;

static unsigned char arrnum[] = {0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,0x7f,0x6f,0x40};

static void DelayNms(int nums)
{
	while(nums--);
}

static void InitInterrupt0()
{
	IT0=0;
	EX0=1;
	ET0=1;
	EA=1;
}

static void InitTimer0()
{
	TMOD = 0x01; 
	TH0  = 0xFC;  
	TL0  = 0x18;  
	TR0  = 1;     
}

void main()
{
	P0=0x00;
	InitInterrupt0();
	InitTimer0();
	while(1)
	{
		if(iCnt>=0&&iCnt<=9)
		{
			P0=arrnum[iCnt];
			SEL4=1;
		}
		else if(iCnt>=10&&iCnt<=60)
		{
			P0=arrnum[iCnt/10];
			SEL3=0;
			DelayNms(5);
			SEL3=1;

			P0=arrnum[iCnt%10];
			SEL4=0;
			DelayNms(5 );
			SEL4=1;
		}


		if(iCnt==0)
		{
			LED0=0;
		}
		else
		{
			LED0=1;
		}
	}
}



void Timer0_Handler() interrupt 1
{
	TH0=0xFC;
	TL0=0x18;
	jCnt--;
	if(jCnt%1000==0) //秒的整数倍
	{
		iCnt--; //秒数减一
	}
}

void Interrupt0() interrupt 0
{
	iCnt=60;
}
```

#### 复杂倒计时
![[Pasted image 20230905193253.png]]
四个按键分别控制倒计时开始，暂停，时间加和减。剩下30S时蜂鸣器响，倒计时结束蜂鸣器响。
```c
#include <reg51.h>

 
unsigned char min=10;
unsigned char sec=0;

sbit KEY1=P3^0;
sbit KEY2=P3^1;
sbit KEY3=P3^2;
sbit KEY4=P3^3;

sbit LSA=P2^0;    
sbit LSB=P2^1;
sbit LSC=P2^2;
sbit LSD=P2^3;
sbit LSE=P2^4;

sbit bee=P2^6;
sbit led=P2^7;

unsigned char table[]={0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,0x7f,0x6f,0x40};//CC
 
void delay(unsigned int z)
{
	while(z--);
}
 
void T0_INIT()
{
	TMOD=0x11;//定时器方式为方式一
	EA=1;//开总中断
	ET0=1;//开T0中断
	TR0=1;//定时器0计时
	TH0=0x3c; //设置初值，定时时间为50ms
	TL0=0xb0;
}
 
void Timer()interrupt 1
{
	static int i=0;
	TL0 = 0xb0;		
	TH0 = 0x3c;	
 
	i++;
	if(i>19)
	{
		i=0;
		if(min>0||sec>0)
		{	
			if(sec==0)
			{
				sec=59;
				sec++;
			}
			if(sec>0)
			{
				sec--;
			}
			if(sec==59)
			{
				min--;
			}
		}
	}
}
	
void Show_time()
{
	P0=0x00;
	P0=table[min/10%10];
	LSA=0;delay(100);LSA=1;		//数码管位选
	
	P0=table[min%10];
	LSB=0;delay(100);LSB=1;
	
	P0=table[10];
	LSC=0;delay(100);LSC=1;
	
	P0=table[sec/10%10];
	LSD=0;delay(100);LSD=1;
	
	P0=table[sec%10];
	LSE=0;delay(100);LSE=1;
	P0=0x00;
	
}
 
void check_Sec_ADD()
{
	if(KEY4==0)
	{
		delay(20); //消抖
		while(!KEY4); //等待弹起

		Show_time();
		sec++; //实现函数的功能_添加秒数
		if(sec>59)
		{
			min++;
			sec=0;
		}
	}
}
 
 
void check_Sec_SUB()
{
	if(KEY3==0)
	{
		delay(20); //消抖
		while(!KEY3);	//判断按键是否松手
		
		Show_time();
		if(min>0||sec>0)
		{
			sec--; //实现函数的功能_减少秒数
			if(sec==0)
			{
				min--;
				sec=59;
			}
		}	
	}
}
 
 
void check_Pause() //进入该函数,将会判断是否停止定时器
{
	if(KEY2==0)
	{
		delay(10); //消抖
		if(KEY2==0)
		{
			TR0=0; //计时器停止
		}
	}
	if(min==0&&sec==0) //当倒计时结束时
	{
		TR0=0; //计时器停止
		bee=~bee; //蜂鸣器鸣叫
		led=0; //led灯亮起
	}
}
 
void check_Bee()
{
	if(min==0&&sec==30) //还剩三十秒的时候,蜂鸣器作响
	{
		bee=~bee;
	}
}
 
main()
{
	T0_INIT();
	while(1)
	{
 
		Show_time();
		
		if(KEY1==0) //一按下KEY1就进入这个语句段
		{
			if(min>0||sec>0) //还有时间,允许取消暂停定时器
			{
				TR0=1;
			}
			delay(5);
 
			while(KEY1)//松手之后(即0变成1),按键弹起,此时进入该循环
			{
				Show_time();
				check_Pause();
				check_Bee();
				check_Sec_ADD(); 
				check_Sec_SUB();
			}
		}
		check_Pause();
		check_Bee();
		check_Sec_SUB();
		check_Sec_ADD(); 
    }			
}
```

#### 长按短按控制LED灯亮灭

- P1.0控制LED灯（0为亮，1为灭），P2.0为按键KEY（按下为0，不按下为1），
- 实现按键 [短按0.2s] 后开灯，[长按2s] 后关灯。

```c
#include <reg51.h>
#define uint unsigned int
#define uchar unsigned char

sbit LED=P1^0;
sbit KEY=3^0;

static uint iCnt=0;
 
void Timer0Init()  //定时器0初始化
{
    TMOD |= 0x01; //模式1
    TMOD &= 0x0F;
    TH0 = 0xFC;
    TL0 = 0x18;  //定时1ms
    EA=1;
    ET0=1;
    
    TR0=1; //T0定时器启动
}
 
void delay (uint iCnt) //延时函数
{
    while(iCnt--);
}
 
void main()
{
    while(1) //一直检测按键是否按下  按下则进入中断
    {
        delay(1000);
        if(KEY==0)
        {
            delay(1000); //按键消抖
            if(KEY==0)
                Timer0Init(); //定时器0初始化 申请中断
        }
        else
        {
            TR0=0;    //无按键按下时，关闭定时器  且将iCnt置零
            iCnt=0;
        }
    }
}
 
void Timer0() interrupt 1  //定时器
{
    TH0 = 0xFC;
    TL0 = 0x18;  //定时1ms
    iCnt++;
    if(iCnt>=20&&iCnt<=2000)   //超过0.02s且不超过2s为开灯   iCnt++至200时 为0.2s 
        LED=0;
    if(iCnt>=2000)   //超过2s关灯  iCnt++至2000时 为2s 
        LED=1;
}

```



#### PWM控制LED亮度原理
- 展示三个不同亮度的LED
```c
#include<reg51.h>
#define uint unsigned int
#define uchar unsigned char

sbit led0 = P1^7;

void delay (uint iCnt) //延时函数
{
    while(iCnt--);
}

void main()
{
	// 0%
	uint i = 1000;
	while(i--)
	{
		led0 = 0;
		delay(100);
	}

	// 50%
	i=1000;
	while(i--)
	{
		led0 = 0;
		delay(50);
		led0 = 1;
		delay(50);
	}

	// 60%
	i=1000;
	while(i--)
	{
		led0 = 0;
		delay(40);
		led0 = 1;
		delay(60);
	}

	// 100%
	i=1000;
	while(i--)
	{
		led0 = 1;
		delay(100);
	}
}
```

#### PWM控制呼吸灯原理
- 设计一个呼吸灯程序，通过逐渐改变PWM的占空比实现LED1灯呼吸效果
1.  **LED亮度控制：** 根据 `s_iCnt1` 和 `s_iDuty` 的比较，控制LED的开关状态。当 `s_iCnt1` 小于等于 `s_iDuty` 时，LED处于开启状态，否则LED处于关闭状态。这样，LED的亮度随着占空比的变化而改变，实现了呼吸灯的效果。
2. **呼吸效果：** 使用 `s_iFlag` 变量来控制占空比的变化方向。当 `s_iFlag` 为0时，占空比减小，LED的亮度逐渐降低，呈现出灯光逐渐熄灭的效果。当 `s_iFlag` 为1时，占空比增加，LED的亮度逐渐增加，呈现出灯光逐渐亮起的效果。
```c
#include <reg52.h>

sbit LED1 = P2^4; //定义LED1

static void InitInterrupt()
{
  ET0 = 1;   //打开 定时器0 中断允许
  EA  = 1;   //打开 总中断允许
}


static void InitTimer0()
{
  TMOD = 0x02;  //设置 定时器0 工作方式2(8位自动重装)
  TH0  = 0x9C;  //设置 定时器0 重装值
  TL0  = 0x9C;  //设置 定时器0 计数初值，定时时间100μs
  TR0  = 1;     //打开 定时器0
}


void main()
{
  InitInterrupt();  //配置中断
  InitTimer0();     //配置定时器0
  while (1)
  {

  }
}

void Timer0_Handler () interrupt 1
{
  static unsigned int s_iCnt1 = 0;        //定义静态变量s_iCnt1作为10ms计数变量，用于控制PWM周期
  static unsigned int s_iCnt2 = 0;        //定义静态变量s_iCnt1作为20ms计数变量，用于控制呼吸快慢
  static unsigned int s_iDuty = 100;      //定义静态变量s_iDuty作为占空比,单位是百分比
  static unsigned char s_iFlag = 0;       //定义静态变量s_iFlag作为占空比减小或增加的标志，为0时占空比减小，为1时增加
  
  s_iCnt1++;
  if(s_iCnt1 >= 100)       //PWM周期控制，周期为100*100us=10ms
  {
    s_iCnt1 = 0;           //清零10ms计数变量
  }
  
  s_iCnt2++;
  if(s_iCnt2 >= 200)       //调节占空比，周期为200*100μs=20ms，用于控制呼吸快慢
  {
    s_iCnt2 = 0;           //清零20ms计数变量

    //设置占空比调节标志
    if(s_iDuty >= 100 && 1 == s_iFlag)     //若占空比增大到100且当前标志为增加占空比
    {
      s_iFlag = 0;                         //设置标志为减小占空比
    } 
    else if(0 == s_iDuty && 0 == s_iFlag)  //若占空比减小到0且当前标志为减小占空比
    {
      s_iFlag = 1;                         //设置标志为增加占空比
    }

//s_iflag = (s_iDuty >= 100 && s_iflag == 1) ? 0 : ((s_iDuty == 0 && s_iflag == 0) ? 1 : s_iflag);

    //根据标志来判断是减小还是增加占空比
    if(0 == s_iFlag) 	     //若s_iFlag为0
    {
      s_iDuty--;           //减小占空比
    }
    else if(1 == s_iFlag)  //若s_iFlag为1
    {
      s_iDuty++;           //增加占空比
    }
    // s_iDuty += (s_iFlag) ? 1 : -1;

  }

  if(s_iCnt1 <= s_iDuty)   //控制LED的打开与关闭
  {
    LED1 = 1;              //P2.4引脚输出高电平，关闭LED1
  }
  else
  { 
    LED1 = 0;              //P2.4引脚输出低电平，打开LED1
  }
  // LED1 = (s_iCnt1<=s_iDuty) ? 1 : 0;
}

```
#### PWM按键控制呼吸灯速度
```c
#include <reg52.h>

sbit LED1 = P2 ^ 4;

sbit KEY1 = P3 ^ 2; //减少总呼吸T
sbit KEY2 = P3 ^ 3; //增加总呼吸T
sbit KEY3 = P3 ^ 4; //重置T;

sbit SEL0 = P2 ^ 3; // 数码管定义4个位选 ,显示周期总数
sbit SEL1 = P2 ^ 2;
sbit SEL2 = P2 ^ 1;
sbit SEL3 = P2 ^ 0;

unsigned char arr_number[] = {0x03, 0x9f, 0x25, 0x0d, 0x99, 0x49, 0x41, 0x1f, 0x01, 0x09}; // 0~9的数码管短型

static unsigned int s_icnt1; // 作为单位百分比(也就是1%), 用于在占空比中推进的一个指针,最大值是1(100%)
static unsigned int s_icnt2; // 作为单位时间(取决定时器), 最大值是完整周期(T)
static unsigned int s_iDuty; // 单位为百分比, 表示占空比——高电平占总周期的多少,最大值是完整周期(T)
static unsigned int T;       // 单位为时间, 作为cnt2的上限, 可以改变一次完整呼吸的时间
static unsigned int s_iflag; // 占空比增加/减少的标志: 0减少1增加

void init_timer0() //中断初始化
{
    ET0 = 1; //允许定时器中断——小门
    EA = 1;  //总中断允许标志位，0断，1通——大门

    TMOD = 0X02; //把定时器0的工作模式设置为16位定时器
    //设置8位自动重装定时器的初始值————定时时间100us
    TH0 = 0X9c;
    TL0 = 0X9c;

    TR0 = 1; //启动定时器0
}

static void Delay(int nms)//@12.000MH——延迟1毫秒
{
    unsigned int i, j;
    for (i = 0; i < nms; i++)
    {
        for (j = 0; j < 123; j++)
        {
        }
    }
}

void main()
{
    init_timer0(); // 定时器初始化
    while (1)
    {
        //数码管显示
        SEL0 = 0; // 第一位数码管显示
        P0 = arr_number[T / 1000];
        Delay(2);
        SEL0 = 1;

        SEL1 = 0; // 第二位数码管显示
        P0 = arr_number[(T % 1000) / 100];
        Delay(2);
        SEL1 = 1;

        SEL2 = 0; // 第三位数码管显示
        P0 = arr_number[(T % 100) / 10];
        Delay(2);
        SEL2 = 1;

        SEL3 = 0; // 第四位数码管显示
        P0 = arr_number[T % 10];
        Delay(2);
        SEL3 = 1;

        T = (KEY3 == 0 || T > 9999) ? 0 : T; // 清零
        }
}


static timer0() interrupt 1 // 定时器0的中断函数
{
    s_icnt1++; 
    if (s_icnt1 >= 100) // 周期位10ms————100us*100——>10ms
    {
        s_icnt1 = 0;
    }

    s_icnt2++;
    /*———以下部分不能放在主函数while循环里面————因为while循环里有数码管的延迟函数，并且延迟时间长，得出不到正确结果，现象不正确————*/
    if (s_icnt2 >= T)
    {
        s_icnt2 = 0;
        
        //以下内容即每T执行一次完整呼吸灯
        
        //设置占空比标志
        s_iflag = (s_iDuty == 0) ? ~s_iflag : s_iflag;
        s_iflag = (s_iDuty >= 100) ? ~s_iflag : s_iflag;
        //占空比的改变
        s_iDuty += (s_iflag) ? 1 : -1;
        //按键控制呼吸快慢
        T += (KEY2 == 0) ? 1 : 0; // 周期总长+1
        T -= (KEY1 == 0) ? 1 : 0; // 周期总长-1
    }
    LED1 = (s_icnt1 <= s_iDuty) ? 1 : 0; // 控制打开/关闭led
}
```


#### 串口字符发送
![[Pasted image 20230916171848.png]]
- 通过虚拟串口建立一个`COM1`和`COM2`的串口对, 一个在仿真中, 一个是串口助手那
- `COM2`串口助手(其实仿真中那个黑色的`shell`一样的), 用于接收`COM1`发送出去的字符
- `COM1` 则来接收`COM2`发送的字符

```c
#include <reg51.h>
void UART_Init(void)
{
    SCON = 0x50;  //设置 串口 工作模式1，并打开REN接收允许
    PCON &= 0XEF; //设置 波特率加倍(即其中的SMOD字段置为0.其他字段不变 )

    // 波特率时钟
    TMOD = 0X20; //设置 定时器1 工作模式2（8位自动重装定时器定时器）
    TL1 = 0XFD;  //设置 定时器1 计数初值，波特率为4800
    TH1 = TL1;   //设置 定时器1重装值
    TR1 = 1;     //打开计数器

    ES = 1; //打开中断
    EA = 1;
}

void Send_chr_test() //测试单字符接收
{
    SBUF = '3';
    while (!TI);
    TI = 0;

    SBUF = '1';
    while (!TI);
    TI = 0;

    //传送了3和1,即"31"

    // EA(总闸)和ES将允许串口接收中断发生，当串口接收缓冲器SBUF接收到数据并触发了串口接收中断（RI位被置位）时，中断服务程序会被执行。在你的代码中，如果没有提供串口接收中断的处理程序，则当串口接收到数据时，没有正确处理中断,CPU将进入中断服务程序，//这可能会导致串口只接收到一个'3'，然后中断使能被打开，但没有处理'1'，因此你只看到了'3'。
}
void Send_str(char *str) //测试多字符接收
{
    unsigned char i;
    for (i = 0; str[i] != '\0'; i++)
    {
        SBUF = str[i];
        while (!TI);
        // TI = 0;
        //下面的串口中断函数用于将两个标志位置零的操作,既然中断函数里在每次接收和发送都触发了,
        //那这里就不用进行置零操作了,上面的测试函数也是的, 但是懒得修改了
    }
}

void Nixie_show(unsigned char ch)
{
    unsigned char leddata[] = {0x3f, 0x06, 0x5b, 0x4f, 0x66, 0x6d, 0x7d, 0x07, 0x7f, 0x6f, 0x77, 0x7c, 0x39, 0x5e, 0x79, 0x71};
    P1 = leddata[ch - '0']; //在COM1接收到COM2发送的字符串
}

void main()
{
    P1 = 0x00;         //初始化数码管
    UART_Init();       //初始化串口
    Send_chr_test();   //进行单字符发送测试 //在COM2接收31
    Send_str("hello"); //进行多字符发送测试 //在COM2接收hello
    while (1) ;
}

////每次接收和发送都会触发中断,此时根据中断标志位来判断是接收还是发送即可

void UART_handle() interrupt 4
{
    unsigned char tmp;
    if (RI) //为1的时候就说明有数据接收了
    {
        RI = 0;
        tmp = SBUF; //将缓存器中的数据取出来
        Nixie_show(tmp);
    }
    else //发送中断
    {
        TI = 0;
    }
}
```
- 这里直接使用了`puts`函数（未重定义的那种） ，但出现了只能在串口助手上打印常量而不能打印变量的缘故
```c

#if 1
#include <reg52.h>
#include <stdio.h>

unsigned char s;
unsigned int a;

void UART_Init(void)
{
    SCON = 0x50;  //设置 串口 工作模式1，并打开REN接收允许
    PCON &= 0XEF; //设置 波特率加倍(即其中的SMOD字段置为0.其他字段不变 )

    // 波特率时钟
    TMOD = 0X20; //设置 定时器1 工作模式2（8位自动重装定时器定时器）
    TL1 = 0XF3;  //设置 定时器1 计数初值，波特率为4800
    TH1 = TL1;   //设置 定时器1重装值
    TR1 = 1;     //打开计数器

    ES = 1; //打开中断
    EA = 1;
}

static void Delay(int nms) //@12.000MH——延迟1毫秒
{
    unsigned int i, j;
    for (i = 0; i < nms; i++)
    {
        for (j = 0; j < 123; j++)
        {
        }
    }
}
void main()
{
	UART_Init();
	while(1)
	{
		TI=1;
		puts("你好");
		while(!TI);
		TI=0;
		Delay(500);
	}
}


#endif
```

#### 模拟串口接发（多字符）

原先是`2022-07-13 10:24:23 WED`
通过串口助手自发自收，改变其为`2022-08-13 10:24:23 WED`
![[Pasted image 20230923023015.png]]
```c
#if 1

#include <reg52.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

unsigned char Date_Str[23]= "2022-07-13/10:24:23/WED";   // \0
unsigned char index=0;

void UART_Init(void)
{
    SCON = 0x50;  //设置 串口 工作模式1，并打开REN接收允许
    PCON &= 0XEF; //设置 波特率加倍(即其中的SMOD字段置为0.其他字段不变 )

    // 波特率时钟
    TMOD = 0X20; //设置 定时器1 工作模式2（8位自动重装定时器定时器）
    TL1 = 0XF3;  //设置 定时器1 计数初值，波特率为4800
    TH1 = TL1;   //设置 定时器1重装值
    TR1 = 1;     //打开计数器

    ES = 1; //打开中断
    EA = 1;
}

void UART_SendByte(unsigned char byte)
{
    SBUF = byte;
    while (!TI)
    {
    } // 等待发送完成
    TI = 0;
}


void UART_SendMessage(unsigned char * arr)
{
    unsigned char i;

    for (i = 0; arr[i] != '\0'; i++)
    {
        if (arr[i] == '/') //遇到'/'则发送空格并进行跳跃到下一个字符的发送
        {
            UART_SendByte(' ');
            continue;
        }
        UART_SendByte(arr[i]);
    }
    UART_SendByte('\r'); // 将光标移动到当前行的开头，但不换行到下一行。
    UART_SendByte('\n'); // 将光标移到下一行的开头，实现换行。
}

static void Delay(int nms) //@12.000MH——延迟1毫秒
{
    unsigned int i, j;
    for (i = 0; i < nms; i++)
    {
        for (j = 0; j < 123; j++)
        {
        }
    }
}


void main()
{
	UART_Init();
	//ISP_CONTR |= 0x20;
	while(1)
	{
		UART_SendMessage(Date_Str);
		
		Delay(500);
	}
}

void UART_Handler() interrupt 4
{
    if (RI)
    {
	   if(index==25)index=0;		
        RI = 0;
        if (SBUF == ' ')
        {
            Date_Str[index] = '/';
        }
        Date_Str[index++] = SBUF;
    }
}
 
#endif
```
#### 外部中断模拟死机的喂狗操作
```c
#include <reg52.h>

sbit BEEP = P1 ^ 0;    //定义蜂鸣器
sfr WDT_CONTR = 0xE1;  //定义看门狗寄存器

static void DelayNms(int nms)
{
  unsigned int i,j;
  for(i = 0; i < nms; i++)
  {
    for(j = 0; j < 123; j++)
    {
      
    }
  }
}

static void InitInterrupt()
{
  IT0 = 1;   //设置 外部中断0 触发方式为下降沿触发
  EX0 = 1;   //打开 定时器0 中断允许
  EA  = 1;   //打开 总中断允许
}

void main()
{
  unsigned char i;        //定义循环计数变量i
   
  BEEP = 0;               //打开蜂鸣器
  DelayNms(100);          //延时100ms，让蜂鸣器短暂鸣叫
  BEEP = 1;               //关闭蜂鸣器

  InitInterrupt();        //初始化中断
  WDT_CONTR = 0x33;       //设置 看门狗溢出时间为524.2ms
  while (1)
  {
    //流水灯
    P2 = 0xEF;              //点亮LED1
    for(i = 0; i < 4; i++)
    {
      DelayNms(500);        //延时500ms
      P2 = P2 << 1;         //左移一位
      WDT_CONTR |= 0x10;    //喂狗操作
    }
  }
}

void External0_Handler() interrupt 0 
{
  DelayNms(2000);        //模拟单片机因外部干扰而卡机
}

```

#### 串口调试
```c
void UART_Handler() interrupt 4
{
	static unsigned char s_iCounter = 0;
	if( 1 == RI )
	{
		arrBuffer[s_iCounter] = SBUF;
		RI = 0;
		s_iCounter ++;
	}
}

void SendByte(unsigned char s_iCounter)
{
	SBUF = s_iCounter; //调试以查看s_iCounter的值变化情况
	while(!TI);
	TI=0;
}

void External0_Handler() interrupt 0 //外部中断来用于发送数据
{
	SendByte(s_iCounter);
}
```

#### 模拟EEPROM
编写程序实现内部Flash的读写与擦除操作，断电前的每一次按键控制的亮灭操作都将进行一次擦除和写入更新，以记录下LED灯的亮灭状态，并能在重新通电复位后， 读取扇区以恢复断电前LED灯的亮灭状态，
```c
#include <reg52.h>
#include <EEPROM.h>
sfr ISP_DATA  = 0XE2;    //定义ISP-IAP操作时的数据寄存器
sfr ISP_ADDRH = 0XE3;    //定义ISP-IAP操作地址寄存器低位
sfr ISP_ADDRL = 0XE4;    //定义ISP-IAP操作地址寄存器高位
sfr ISP_CMD   = 0XE5;    //定义ISP-IAP命令寄存器
sfr ISP_TRIG  = 0XE6;    //定义ISP-IAP命令触发寄存器
sfr ISP_CONTR = 0XE7;    //定义ISP-IAP命令寄存器

sbit LED1 = P2 ^ 4;    //位定义LED1
sbit LED2 = P2 ^ 5;    //位定义LED2
sbit LED3 = P2 ^ 6;    //位定义LED3
sbit KEY1 = P3 ^ 2;    //位定义KEY1
sbit KEY2 = P3 ^ 3;    //位定义KEY2
sbit KEY3 = P3 ^ 4;    //位定义KEY3

static void IAPTrigger()
{
  ISP_TRIG = 0x46;          //对ISP-IAP命令触发寄存器写入触发命令0x46
  ISP_TRIG = 0xB9;          //对ISP-IAP命令触发寄存器写入触发命令0xB9
}

static void IAPDisable()
{
  ISP_CONTR = 0x00;          //禁用IAP读写EEPROM
  ISP_CMD   = 0x00;          //无IAP操作
  ISP_TRIG  = 0x00;          //关闭IAP功能
}

unsigned char IAPByteRead(unsigned int addr)
{
  unsigned char dat;        //定义数据缓存变量
  ISP_CONTR = 0x81;         //打开IAP功能，允许编程改变Flash，设置Flash操作等待时间
  ISP_CMD   = 0x01;         //允许对"Data Flash/EEPROM区"进行字节读取
  
  ISP_ADDRL = addr;         //IAP操作地址寄存器低位
  ISP_ADDRH = addr >> 8;    //IAP操作地址寄存器高位

  IAPTrigger();             //触发IAP功能
  dat = ISP_DATA;           //将需要读出的数据放进缓存变量
  IAPDisable();             //禁用IAP功能
  return dat;               //将读取到的数据作为返回值
}

void IAPSectorErase(unsigned int addr)
{
  ISP_CONTR = 0x81;           //打开IAP功能，允许编程改变Flash，设置Flash操作等待时间
  ISP_CMD   = 0x03;           //允许对"Data Flash/EEPROM区"进行扇区擦除

  ISP_ADDRL = addr;           //写入IAP操作地址寄存器低位
  ISP_ADDRH = addr >> 8;      //写入IAP操作地址寄存器高位

  IAPTrigger();               //触发IAP功能
  IAPDisable();               //禁用IAP功能
}

void IAPByteWrite(unsigned int addr, unsigned char dat)
{
  ISP_CONTR = 0x81;          //打开IAP功能，允许编程改变Flash，设置Flash操作等待时间
  ISP_CMD   = 0x02;          //允许对"Data Flash/EEPROM区"进行字节写入

  ISP_ADDRL = addr;          //IAP操作地址寄存器低位
  ISP_ADDRH = addr >> 8;     //IAP操作地址寄存器高位
  ISP_DATA = dat;            //将需要写入的数据放进ISP_DATA

  IAPTrigger();              //触发IAP功能
  IAPDisable();              //禁用IAP功能
}


static void DelayNms(int nms)
{
  unsigned int i,j;
  for(i = 0; i < nms; i++)
  {
    for(j = 0; j < 123; j++)
    {
      
    } 
  }
}

void main()
{
  P2 = IAPByteRead(0x2000);               //读取起始地址为0x2000的扇区中的值，赋值给P2寄存器，恢复LED灯状态
  while(1)  
  {
    if(0 == KEY1)                         //第一次检测到KEY1按键被按下
    {
      DelayNms(50);                       //等待约50ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if(0 == KEY1)                       //第二次检测到KEY1按键被按下
      {
        LED1 = ~LED1;                     //对LED1状态取反，改变LED1的亮灭状态
        while(0 == KEY1);                 //等待按键弹起  
        IAPSectorErase(0x2000);           //擦除起始地址为0x2000的扇区
        IAPByteWrite(0x2000,P2);          //对起始地址为0x2000的扇区写入P2寄存器的值
      }
    }

    if(0 == KEY2)                         //第一次检测到KEY2按键被按下
    {                       
      DelayNms(50);                       //等待约50ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if(0 == KEY2)                       //第二次检测到KEY2按键被按下
      {
        LED2 = ~LED2;                     //对LED2状态取反，改变LED2的亮灭状态
        while(0 == KEY2);                 //等待按键弹起
        IAPSectorErase(0x2000);           //擦除起始地址为0x2000的扇区
        IAPByteWrite(0x2000,P2);          //对起始地址为0x2000的扇区写入P2寄存器的值
      }
    }

    if(0 == KEY3)                         //第一次检测到KEY3按键被按下
    {
      DelayNms(50);                       //等待约50ms后再次检测按键是否被按下，消除按键抖动带来的影响
      if(0 == KEY3)                       //第二次检测到KEY3按键被按下
      {
        LED3 = ~LED3;                     //改变LED3的开关状态
        while(0 == KEY3);                 //等待按键弹起 
        IAPSectorErase(0x2000);           //擦除起始地址为0x2000的扇区
        IAPByteWrite(0x2000,P2);          //对起始地址为0x2000的扇区写入P2寄存器的值
      }
    }
  }
}

```


#### 串口发送字符并打印电子钟

设计一个具有实用价值的电子钟。要求能够在串口助手上每间隔 1 秒打印一 次日期与时间，格式如下： 年-月-日 时:分:秒 星期 例如： 
```
2022-07-13 10:24:23 Wed
2022-07-13 10:24:24 Wed
2022-07-13 10:24:25 Wed
```  

```c

#if 1
#include <reg52.h>
void InitTimer0();
void InitInterrupt1(); //外部中断实现
void InitUART();       //串口专用定时器
void InitInterrupt2();
void flow_light();
void show(unsigned char *x, unsigned char *y);
void refresh_time(); //转数字
void convert_year(unsigned int number, unsigned char *year1, unsigned char *year2, unsigned char *year3, unsigned char *year4);
void DelatNms(int nms); //延时函数
void conver_time(unsigned char number, unsigned char *damn, unsigned char *xyq);
void show_chuankou_SBUF(unsigned char damn);
void show_chuankou();
void CountWeek(); //计算该日为星期几
void weekprintf();
sbit SegmentG1 = P2 ^ 3; //定义数码管1
sbit SegmentG2 = P2 ^ 2; //定义数码管2
sbit SegmentG3 = P2 ^ 1; //定义数码管3
sbit SegmentG4 = P2 ^ 0; //定义数码管4
sbit LED1 = P2 ^ 4;
sbit LED2 = P2 ^ 5;
sbit LED3 = P2 ^ 6;
sbit LED4 = P2 ^ 7; //定义LED
unsigned char flag = 1;

unsigned char lu = 0;

static unsigned char arrNumber[] = {0x03, 0x9F, 0x25, 0x0D, 0x99, 0x49, 0x41, 0x1f, 0x01, 0x09, 0xFE};

void InitInterrupt1() //外部中断实现
{
    IT0 = 1; //设置外部中断0为下降沿触发
    EX0 = 1; //打开外部中断0的中断允许
    ET0 = 1; //打开定时器0的中断允许

    IT1 = 1; //设置外部中断1为下降沿触发
    EX1 = 1; //打开外部中断1的中断允许
}

void InitTimer0()
{
    TMOD = 0x01; //设置定时器0为工作模式1
    TH0 = 0x9E;  //设置定时器0的计算初值的高8位
    TL0 = 0x58;  //设置定时器0的计数初值的低8位，1ms后溢出
    TR0 = 1;     //打开定时器
}

void InitUART() //串口专用定时器
{
    SCON = 0x50; //设置串口工作模式1，并打开接收允许
    TMOD = 0x20; //设置定时器1，工作模式2（八位重装载）
    PCON = 0x80; //设置波特率加倍
    TL1 = 0xF3;  //设置定时器1的初值，波特率为4800
    TH1 = TL1;   //设置定时器1重装值
    TR1 = 1;     //打开计数器1
}

void InitInterrupt2()
{
    ES = 1; //打开串口中断允许
    EA = 1; //打开总中断允许
}

void DelatNms(int nms) //延时函数
{
    unsigned int i, j;
    for (i = 0; i < nms; i++)
    {
        for (j = 0; j < 123; j++)
        {
        }
    }
}

void flow_light()
{
    LED1 = 0;
    DelatNms(500);
    LED1 = 1;

    LED2 = 0;
    DelatNms(500);
    LED2 = 1;

    LED3 = 0;
    DelatNms(500);
    LED3 = 1;

    LED4 = 0;
    DelatNms(500);
    LED4 = 1;
}

void show(unsigned char *x, unsigned char *y)
{
    SegmentG1 = 0;
    P0 = arrNumber[*x / 10];
    DelatNms(5);
    SegmentG1 = 1; //消影

    //十位数显示
    SegmentG2 = 0;
    P0 = arrNumber[*x % 10];
    DelatNms(5);
    SegmentG2 = 1; //消影

    //百位数显示
    SegmentG3 = 0;
    P0 = arrNumber[*y / 10];
    DelatNms(5);
    SegmentG3 = 1; //消影

    SegmentG4 = 0;
    P0 = arrNumber[*y % 10];
    DelatNms(5);
    SegmentG4 = 1; //消影
}

unsigned char receivedString[30]; //接收数组

unsigned char ji = 0;
unsigned int year;
unsigned char month;
unsigned char day;
unsigned char hour;
unsigned char min;
unsigned char second;
unsigned char week;
unsigned char *i, *j;

unsigned char year1, year2, year3, year4;
unsigned char month1, month2;
unsigned char day1, day2;
unsigned char hour1, hour2;
unsigned char min1, min2;
unsigned char second1, second2;

void refresh_time() //转数字
{

    year = (receivedString[0] - '0') * 1000 + (receivedString[1] - '0') * 100 + (receivedString[2] - '0') * 10 + (receivedString[3] - '0');
    month = (receivedString[4] - '0') * 10 + (receivedString[5] - '0');
    day = (receivedString[6] - '0') * 10 + (receivedString[7] - '0');
    hour = (receivedString[8] - '0') * 10 + (receivedString[9] - '0');
    min = (receivedString[10] - '0') * 10 + (receivedString[11] - '0');
    second = (receivedString[12] - '0') * 10 + (receivedString[13] - '0');
}

void convert_year(unsigned int number, unsigned char *year1, unsigned char *year2, unsigned char *year3, unsigned char *year4)
{
    *year1 = (number / 1000) + '0'; //四位转换
    *year2 = ((number / 100) % 10) + '0';
    *year3 = ((number / 10) % 10) + '0';
    *year4 = (number % 10) + '0';
}

void conver_time(unsigned char number, unsigned char *damn, unsigned char *xyq)
{
    *damn = (number / 10) + '0';
    *xyq = (number % 10) + '0';
}

void show_chuankou_SBUF(unsigned char damn)
{
    SBUF = damn;
    while (!TI)
    {
    } // 等待发送完成
    TI = 0;
}

void show_chuankou()
{
    ji = 0;
    CountWeek();
    show_chuankou_SBUF(year1);
    show_chuankou_SBUF(year2);
    show_chuankou_SBUF(year3);
    show_chuankou_SBUF(year4);
    show_chuankou_SBUF('-');
    show_chuankou_SBUF(month1);
    show_chuankou_SBUF(month2);
    show_chuankou_SBUF('-');
    show_chuankou_SBUF(day1);
    show_chuankou_SBUF(day2);
    show_chuankou_SBUF(' ');
    show_chuankou_SBUF(hour1);
    show_chuankou_SBUF(hour2);
    show_chuankou_SBUF(':');
    show_chuankou_SBUF(min1);
    show_chuankou_SBUF(min2);
    show_chuankou_SBUF(':');
    show_chuankou_SBUF(second1);
    show_chuankou_SBUF(second2);
    show_chuankou_SBUF(' ');

    weekprintf();

    show_chuankou_SBUF('\r');
    show_chuankou_SBUF('\n');
}

void CountWeek() //计算该日为星期几
{
    unsigned char c = (year / 1000) * 10 + ((year / 100) % 10);
    unsigned char y = ((year / 10) % 10) * 10 + (year % 10);
    unsigned char m;
    unsigned char d = day;
    if (month == 1 || month == 2) // 1月与二月单独处理
    {
        m = month + 12;
    }
    else //其他月
    {
        m = month;
    }
    week = y + y / 4 + c / 4 - 2 * c + (26 * (m + 1) / 10) + d - 1;
    week = week % 7;
}

void weekprintf()
{
    switch (week)
    {
    case 1:
        show_chuankou_SBUF('M'), show_chuankou_SBUF('o'), show_chuankou_SBUF('n');
        break;
    case 2:
        show_chuankou_SBUF('T'), show_chuankou_SBUF('e'), show_chuankou_SBUF('s');
        break;
    case 3:
        show_chuankou_SBUF('W'), show_chuankou_SBUF('e'), show_chuankou_SBUF('d');
        break;
    case 4:
        show_chuankou_SBUF('T'), show_chuankou_SBUF('e'), show_chuankou_SBUF('s');
        break;
    case 5:
        show_chuankou_SBUF('F'), show_chuankou_SBUF('r'), show_chuankou_SBUF('i');
        break;
    case 6:
        show_chuankou_SBUF('S'), show_chuankou_SBUF('a'), show_chuankou_SBUF('t');
        break;
    case 7:
        show_chuankou_SBUF('S'), show_chuankou_SBUF('u'), show_chuankou_SBUF('n');
        break;
    }
}

void main()
{
    i = &month, j = &day; //默认显示月日
    InitInterrupt1();     //外部中断实现
    InitTimer0();         //定时器0;
    InitInterrupt2();     //串口中断允许
    InitUART();           //串口用定时器
    while (1)
    {
        // flow_light();
        if (lu == 1)
        {
            convert_year(year, &year1, &year2, &year3, &year4);
            conver_time(month, &month1, &month2);
            conver_time(day, &day1, &day2);
            conver_time(hour, &hour1, &hour2);
            conver_time(min, &min1, &min2);
            conver_time(second, &second1, &second2); //转换
            show(i, j);
        }
    }
}

void Timer0_Handler() interrupt 1
{
    static unsigned int s_iCient = 0; //计数变量
    TH0 = 0x9E;
    TL0 = 0x58;
    s_iCient++;
    if (s_iCient == 400) // 1s到了
    {
        s_iCient = 0;
        if (second == 60)
        {
            second = 0;
            if (min == 60)
            {
                hour++;
                min = 0;
                if (hour == 24)
                {
                    day++;
                    hour = 0;
                }

                else
                {
                    hour++;
                }
            }

            else
            {
                min++;
            }
        }
        else
        {
            second++;
        }
        show_chuankou();
    }
}

void Timer2_Handler() interrupt 0 //外部中断服务函数
{
    flag = ~flag;
    if (flag == 1) //显示月日
    {
        i = &month, j = &day;
    }

    else //显示分秒
    {
        i = &min, j = &second;
    }
}

void Timer3_Handler() interrupt 2 //蜂鸣器外部中断服务函数
{
}

void UART_Handler() interrupt 4 //中断服务函数
{
    if (RI) // RI=0
    {
        lu = 1;
        RI = 0;
        if (SBUF >= '0' && SBUF <= '9')
        {
            receivedString[ji] = SBUF;
            ji++;
        }
        refresh_time();
    }
}
#endif
		
```


#### 模拟串口复杂控制计时器

![[Pasted image 20230920202805.png]]

```c
/*******************************************************************************
程序功能：1.时分秒的动态显示。2.用三个按键实现时分秒的修改，调节的数字闪烁提示。
3.串口控制时钟的暂停、开始、清零、读取
输入指令(回车结束):
暂停计时:stop
开始计时:start
时钟清零:reset
读取当前时间:read
*******************************************************************************/
 
#include <reg52.h>		 //包含需要的头文件
#include <string.h>		 //包含需要的头文件
 
#define u8 unsigned char
#define u16 unsigned int	
 
u8 WeiMa[6]={0xFE,0xFD,0xFB,0xF7,0xEF,0xDF};
u8 DuanMa[10]={0x3F,0x06,0x5B,0x4F,0x66,0x6D,0x7D,0x07,0x7F,0x6F};
 
//函数声明
void Delay_ms(u16 xms);
void ShuMaGuan(u8 wei,u8 duan);
void Display_Timer(u8 hour,u8 min,u8 sec);
u8 Key_Scan();
void PutChar(u8 n);
void UartInit();
void PutString(u8 *p);
void Key_Timer_Set();
void Uart_Timer_Set();
void crlf();
void Pintf_Uart();
 
 
//引脚定义
sbit SW1=P3^2;
sbit SW2=P3^3;
sbit SW3=P3^4;
 
u8 Hour=0,Min=0,Sec=0;//全局变量，时分秒
u8 mode=0;//全局变量：状态切换，0：时钟显示，1：调节时；2：调节分；3：调节秒
bit flash_tip=1;//数码管闪烁标志，为0时数码管熄灭，为一时数码管显示
 
#define Data_SIZE 15 //数据长度
u8 USART_RX_BUF[Data_SIZE]; //接收缓冲,最大Data_SIZE个字节.末字节为换行符 
u8 Data_length=0;   //数据长度
u8 USART_RX_STA=0;  //接收状态标记	
 
//函数功能：定时器初始化
void Time0init()
{
	TMOD|=0x01;				//设置定时器模式
	TF0=0;					//清除TF0标志
	TH0=(65536-50000)/256;  //设置定时初值
	TL0=(65536-50000)%256;	//设置定时初值
	TR0=1;					//定时器0允许计时
	ET0=1;					//中断允许
	EA=1;					//CPU中断允许位打开
}
 
//串口初始化
void UartInit()  //9600bps@11.0592MHz
{
 
	 PCON &= 0x8F;  //波特率倍速
	 SCON = 0x50;  //8位数据,可变波特率
	 TMOD &= 0x0F;  //清除定时器1模式位
	 TMOD |= 0x20;  //设定定时器1为8位自动重装方式
	 TL1 = 0xFD;   //设定定时初值
	 TH1 = 0xFD;  //设定定时器重装值
	 TR1 = 1;  //启动定时器1
	 EA=1;
	 ES=1;  //打开接收中断
}
 
/*******************************************************************************
* 函 数 名: void main()
* 函数功能: 主函数
*******************************************************************************/
void main()
{
	
	
	Time0init();//定时器
	UartInit(); //串口
	Pintf_Uart();//输入提示
	while(1)
	{
		Key_Timer_Set();//按键控制时钟
		Uart_Timer_Set();//按键调节时钟
		Display_Timer(Hour,Min,Sec);//数码管显示
		
	}
}
 
/*******************************************************************************
* 函 数 名: void Pintf_Uart()
* 函数功能: 串口助手输入指示
*******************************************************************************/
void Pintf_Uart()
{
	/***************输入指示*******************/
	PutString("请输入指令");
	PutString("(回车结束):");
	crlf();
	PutString("暂停计时:stop");
	crlf();
	PutString("开始计时:start");
	crlf();
	PutString("时钟清零:reset");
	crlf();
	PutString("读取当前时间:read");
	crlf();
	/*******************************************/
}
 
/*******************************************************************************
* 函 数 名: void Key_Timer_Set()
* 函数功能: //按键调节时钟
*******************************************************************************/
void Key_Timer_Set()
{
	u8 keynum;
	keynum=Key_Scan();//按键返回值
	if(keynum)   //非0表示有按键按下
	{
		switch(keynum)  //判断是哪个按键按下，按键一调节模式，按键2自加，按键3自减
		{
			case 1:if(++mode>=4) mode=0;break;  //++mode为先自增再判断是否大于4
			case 2:
				if(mode==1) if(++Hour>=24) Hour=0;//++mode为先自增再判断是否大于4
				if(mode==2) if(++Min>=60) Min=0;//++Min先自增再判断是否大于60
				if(mode==3) if(++Sec>=60) Sec=0;//++Sec先自增再判断是否大于60
				break;										
			case 3:
				if(mode==1)	if(--Hour==255) Hour=23;//--Hour先自增再判断是否溢出
				if(mode==2) if(--Min==255) Min=59;//--Min先自增再判断是否大溢出
				if(mode==3) if(--Sec==255) Sec=59;//--Sec先自增再判断是否大溢出
				break;
			default:break;
		}
	}	
}
/*******************************************************************************
* 函 数 名: void Key_Timer_Set()
* 函数功能: 串口调节时钟
//strstr(str1,str2) 函数用于判断字符串str2是否是str1的子串。
//如果是，则该函数返回str2在str1中首次出现的地址；否则，返回NULL。
*******************************************************************************/
void Uart_Timer_Set()
{
	static char str[10];//串口数据缓存区
	u8 i;
	if(USART_RX_STA) //如果串口接收到数据
	{
		for(i=0;i<Data_length;i++)  //将数据存入数组
		{
			str[i]=USART_RX_BUF[i];
		}			
		USART_RX_STA=0; //接收完毕
		if(strstr(str,"stop"))  //暂停计时
		{
			TR0=0;
			PutString("已暂停");
			crlf();
		}
			
		else if(strstr(str,"start"))  //暂停计时
		{
			TR0=1;
			PutString("已开始");
			crlf();
		}
		else if(strstr(str,"reset"))  //暂停计时
		{
			 Hour=0;
			 Min=0;
			 Sec=0;
			 PutString("已清零");
			 crlf();
		}
		else if(strstr(str,"read"))  //暂停计时
		{
			 PutString("当前时间为:");
			 PutChar(Hour/10+48);  //转化ASCII码字符，0为48，1为48+1=49.....
			 PutChar(Hour%10+48);
			 PutChar(':');
			 PutChar(Min/10+48);
			 PutChar(Min%10+48);
			 PutChar(':');
			 PutChar(Sec/10+48);
			 PutChar(Sec%10+48);
			 crlf();
		}
		else
		{
			PutString("指令错误！请重新输入");
			crlf();
		}			
			
		ES=1;//打开接收中断
	}
}
 
/*******************************************************************************
* 函 数 名: void Delay_ms(u16 xms)
* 函数功能: 软件延时函数,xms为延时多少毫秒
*******************************************************************************/
void Delay_ms(u16 xms)
{
	
	unsigned char i, j;
	while(xms--)
	{
		i = 2;
		j = 135;
		do
		{
			while (--j);
		} while (--i);
	}
}
 
/*******************************************************************************
* 函 数 名: void ShuMaGuan(u8 wei,u8 duan)
* 函数功能: 静态显示一位,参数：wei控制位选duan控制段选，表示要显示的一个数字
*******************************************************************************/
void ShuMaGuan(u8 wei,u8 duan)
{
	P1=WeiMa[wei];    //位选
	P2=DuanMa[duan];  //段选
	Delay_ms(1); //间隔一段时间扫描
	P1=0xFF;    
	P2=0xFF;	   //消隐
}
 
/*******************************************************************************
* 函 数 名: void Display_Timer(u8 hour,u8 min,u8 sec)
* 函数功能：数码管动态显示
  flash_tip为数码管闪烁标志，为0时数码管熄灭，为一时数码管显示
  flash_tip每4.5秒进行取反
*******************************************************************************/
void Display_Timer(u8 hour,u8 min,u8 sec)
{
	if(mode!=1 || flash_tip==1) //mode=1时，左边的条件一直为假，当flash_tip=1时，或运算为真，进入if,数码管显示
	{
		ShuMaGuan(5,hour/10);
		ShuMaGuan(4,hour%10);
	}
	else P1=0xFF;
	
	
	if(mode!=2 || flash_tip==1)//mode=2时，左边的条件一直为假，当flash_tip=1时，或运算为真，进入if,数码管显示
	{
		ShuMaGuan(3,min/10);
		ShuMaGuan(2,min%10);
	}
	else P1=0xFF; 
	
	
	if(mode!=3 || flash_tip==1)//mode=3时，左边的条件一直为假，当flash_tip=1时，或运算为真，进入if,数码管显示
	{
		ShuMaGuan(1,sec/10);
		ShuMaGuan(0,sec%10);
	}
	else P1=0xFF;  
}
 
/*******************************************************************************
* 函 数 名: u8 Key_Scan()
* 函数功能: 独立按键检测,按键按下分别返回1.2.3
*******************************************************************************/
u8 Key_Scan()
{
	static u8 key_up=1; //按键按松开标志
	if(key_up && (SW1==0 || SW2==0 || SW3==0))
	{
		Delay_ms(10); //去抖动
		key_up=0; //松手标志为0，那么下次再检测，if结果为0，则不会进入这里的语句
		if(SW1==0) return 1;
		if(SW2==0) return 2;
		if(SW3==0) return 3;
	}
	else if(SW1 == 1 && SW2 == 1 && SW3 == 1) key_up=1; //松手标志
	return 0; // 无按键按下
}
 
/*******************************************************************************
* 函 数 名: void PutChar(u8 n)
* 函数功能: 发送一个字符
*******************************************************************************/
void PutChar(u8 n)
{
	 SBUF=n;
	 while(!TI);
	 TI=0;
}
 
/*******************************************************************************
* 函 数 名: void PutString(u8 *p)
* 函数功能: 发送字符串
*******************************************************************************/
void PutString(u8 *p)
{
	while(*p!='\0')
	{
		PutChar(*p);
		p++;
	}
}
 
/*******************************************************************************
* 函 数 名: void crlf()
* 函数功能: 换行函数通过输出两个ASCII字符实现
*******************************************************************************/
void crlf()
{
	PutChar(0x0D);
	PutChar(0x0A);
}
 
/*******************************************************************************
* 函 数 名: void uart() interrupt 4
* 函数功能: 串口中断服务函数，单片机接收数据并存入USART_RX_BUF[]数组中
*******************************************************************************/
void uart() interrupt 4
{
	static u8 Data_count=0;
	u8 Data;
	if(RI==1)
	{
		 RI=0;
		 Data=SBUF;
		 if(Data!='\n') //判断是否接收到结束符
		 {
            USART_RX_BUF[Data_count]=Data;//数据还没结束发送，就存到USART_RX_BUF[]数组中
            Data_count++;
        }
        else
        {
            Data_length=Data_count;//记录其数据长度
            Data_count=0;
			USART_RX_STA=1;//接收完成
			ES=0;
        }
	}
}
 
/*******************************************************************************
* 函 数 名: void Time0() interrupt 1
* 函数功能: 定时器0中断服务函数，时钟效果
*******************************************************************************/
void Time0() interrupt 1
{
	static unsigned char flag_1,flag_2; 
	TH0=(65536-50000)/256;  
	TL0=(65536-50000)%256;//重新赋初值
	
	if(mode==0)flag_1++;  //mode为0时，数码管正常显示
	else flag_2++;   
	
	if(flag_1==20 && mode==0)  //每秒执行一次
	{
		flag_1=0;
		if(++Sec>=60) //++Sec先自增再判断是否大于60  
		{
			Sec=0;
			if(++Min>=60)//++Min先自增再判断是否大于60
			{
				Min=0;
				if(++Hour>=24)//++Hour先自增再判断是否大于60
				{
					Hour=0;
				}
			}
		}
	}
	if(flag_2==9)
	{
		flash_tip=~flash_tip;//每4.5秒进行取反
		flag_2=0;
	}
 
}
```
