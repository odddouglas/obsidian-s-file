
>[!note] STM32f103vet6


#### LED初始化
>[!faq] LED的引脚位置分别是 PB1(BULE) PB0(GREEN) PB5(RED)

```c
/**
* @brief  Register configuration of LED
* @param  none
* @retval none
*/
void LED_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
	//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_LED_Drive;				//定义结构体变量
	
	GPIO_LED_Drive.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_LED_Drive.GPIO_Pin = GPIO_Pin_1|GPIO_Pin_0|GPIO_Pin_5; 	//GPIO引脚，赋值为第0号引脚
	GPIO_LED_Drive.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOB, &GPIO_LED_Drive);	
	//将赋值后的构体变量传递给GPIO_Init函数
	//函数内部会自动根据结构体的参数配置相应寄存器
	//实现GPIOA的初始化
	GPIO_SetBits(GPIOB,GPIO_Pin_1|GPIO_Pin_0|GPIO_Pin_5); //将引脚提前赋值为1,此时就是默认熄灭
}	
```

#### LED单色开关
```c
/**
* @brief  Turn on the LED 
* @param  color: the specified color of LED
* @retval none
*/
void LED_ON(const char* color)
{
    if (strcmp(color, "GREEN") == 0) 
    {
        GPIO_ResetBits(GPIOB, GPIO_Pin_0);
    } 
    else if (strcmp(color, "RED") == 0) 
    {
        GPIO_ResetBits(GPIOB, GPIO_Pin_5);
    } 
    else if (strcmp(color, "BLUE") == 0) 
    {
        GPIO_ResetBits(GPIOB, GPIO_Pin_1);
    } 
    else 
    {
        // 如果传入的颜色参数不匹配，可以选择默认操作，例如全部LED都关闭
        GPIO_SetBits(GPIOB, GPIO_Pin_0 | GPIO_Pin_5 | GPIO_Pin_1);
    }
}
/**
* @brief  Turn off the LED 
* @param  color: the specified color of LED
* @retval none
*/
void LED_OFF(const char* color)
{
    if (strcmp(color, "GREEN") == 0) 
    {
        GPIO_SetBits(GPIOB, GPIO_Pin_0);
    } 
    else if (strcmp(color, "RED") == 0) 
    {
        GPIO_SetBits(GPIOB, GPIO_Pin_5);
    } 
    else if (strcmp(color, "BLUE") == 0) 
    {
        GPIO_SetBits(GPIOB, GPIO_Pin_1);
    } 
    else 
    {
        // 如果传入的颜色参数不匹配，可以选择默认操作，例如全部LED都关闭
        GPIO_ResetBits(GPIOB, GPIO_Pin_0 | GPIO_Pin_5 | GPIO_Pin_1);
    }
}
```

#### LED多色闪烁
```c
/**
* @brief  LED flashes at a custom frequency
* @param  tick: The higher the value, the higher the frequency
* @retval none
*/
void LED_Blink(uint32_t tick)
{
	/*设置PA0引脚的高低电平，实现LED闪烁，下面展示3种方法*/
		
	/*方法1：GPIO_ResetBits设置低电平，GPIO_SetBits设置高电平*/
	GPIO_ResetBits(GPIOB, GPIO_Pin_0);	//将PA0引脚设置为低电平
	Delay_ms(tick);										//延时500ms
	GPIO_SetBits(GPIOB,GPIO_Pin_0 );	//将PA0引脚设置为高电平
	Delay_ms(tick);										//延时500ms
	
	/*方法2：GPIO_WriteBit设置低/高电平，由Bit_RESET/Bit_SET指定*/
	GPIO_WriteBit(GPIOB, GPIO_Pin_1, Bit_RESET);		//将PA0引脚设置为低电平
	Delay_ms(tick);										//延时500ms
	GPIO_WriteBit(GPIOB, GPIO_Pin_1, Bit_SET);			//将PA0引脚设置为高电平
	Delay_ms(tick);										//延时500ms
	
	/*方法3：GPIO_WriteBit设置低/高电平，由数据0/1指定，数据需要强转为BitAction类型*/
	GPIO_WriteBit(GPIOB, GPIO_Pin_5, (BitAction)0);		//将PA0引脚设置为低电平
	Delay_ms(tick);										//延时500ms
	GPIO_WriteBit(GPIOB, GPIO_Pin_5, (BitAction)1);		//将PA0引脚设置为高电平
	Delay_ms(tick);	
}
```
#### LED流水灯
>[!caution] 由于`vet6`上面三个颜色的灯集成一个了,所以流水灯功能用不上这个板子,至少引脚就不是连续的

```c
/**
* @brief  LED Flashing like flowing water at a custom frequency
* @param  tick: The higher the value, the higher the frequency
* @retval none
*/
void LED_Flow()
{
	/*使用GPIO_Write，同时设置GPIOA所有引脚的高低电平，实现LED流水灯，注意数据有按位取反*/
	GPIO_Write(GPIOA, ~0x0001);	//0000 0000 0000 0001，PA0引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0002);	//0000 0000 0000 0010，PA1引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0004);	//0000 0000 0000 0100，PA2引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0008);	//0000 0000 0000 1000，PA3引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0010);	//0000 0000 0001 0000，PA4引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0020);	//0000 0000 0010 0000，PA5引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0040);	//0000 0000 0100 0000，PA6引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
	GPIO_Write(GPIOA, ~0x0080);	//0000 0000 1000 0000，PA7引脚为低电平，其他引脚均为高电平
	Delay_ms(100);				//延时100ms
}

```

#### KEY初始化
>[!faq] KEY的引脚位置分别是 PA0(K1) PC13(K2)

```c
/**
* @brief  Register configuration of Key
* @param  none
* @retval none
*/
void Key_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);	//开启GPIOA的时钟
	
	//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_KEY1_Drive;				//定义结构体变量KEY1
	GPIO_KEY1_Drive.GPIO_Mode = GPIO_Mode_IPU;		//GPIO模式，赋值为推挽输出模式
	GPIO_KEY1_Drive.GPIO_Pin = GPIO_Pin_0; 	//GPIO引脚，赋值为第0号引脚
	GPIO_KEY1_Drive.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_InitTypeDef GPIO_KEY2_Drive;				//定义结构体变量KEY2
	GPIO_KEY2_Drive.GPIO_Mode = GPIO_Mode_IPU;		//GPIO模式，赋值为推挽输出模式
	GPIO_KEY2_Drive.GPIO_Pin = GPIO_Pin_13; 	//GPIO引脚，赋值为第0号引脚
	GPIO_KEY2_Drive.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	
	GPIO_Init(GPIOA, &GPIO_KEY1_Drive);	
	GPIO_Init(GPIOC, &GPIO_KEY2_Drive);	
	
}	
```

#### KEY获取端口值
>[!attention]   此函数是阻塞式操作，当按键按住不放时，函数会卡住，直到按键松手。读取引脚输入寄存器的状态值，如果为高电平，则代表引脚处的按钮K1按下

```c
/**
* @brief  Get number from Keys
* @param  none
* @retval The key code value of the pressed button, ranging from 0 to 2, returns 0 to indicate that no key has been pressed
*/
uint8_t Key_Scan(void)
{
	uint8_t KeyNum = KEY_FLOATING; //定义变量，默认键码值为0

	if (GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0)) 
	//读PA0输入寄存器的状态，如果为1，则代表K1按下
	{
		Delay_ms(20); //延时消抖
		while (GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0))
			;		  //等待按键松手
		Delay_ms(20); //延时消抖
		KeyNum = KEY1_ON;	  //置键码为KEY1_ON(1)
	}

	if (GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_13)) 
	//读PC13输入寄存器的状态，如果为1，则代表按键2按下
	{
		Delay_ms(20); //延时消抖
		while (GPIO_ReadInputDataBit(GPIOC, GPIO_Pin_13))
			;		  //等待按键松手
		Delay_ms(20); //延时消抖
		KeyNum = KEY2_ON;	  //置键码为KEY2_ON(2)
	}

	return KeyNum; //返回键码值，如果没有按键按下，所有if都不成立，则键码为默认值0
}

```

#### LED反转
>[!question] 可以把熄灭的亮起，但是亮起的灭不掉，无法反转

```c
/**
* @brief  Reverse the LED status
* @param  none
* @retval none
*/
void LED_Reverse(void)
{
	if(!GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_0)) 
	{	
		GPIO_SetBits(GPIOB,GPIO_Pin_0);
	}
	if(GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_0))
	{
		GPIO_ResetBits(GPIOB,GPIO_Pin_0);
	}
	
	if(!GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_1))
	{
		GPIO_SetBits(GPIOB,GPIO_Pin_1);
	}
	if(GPIO_ReadOutputDataBit(GPIOB,GPIO_Pin_1))
	{
		GPIO_ResetBits(GPIOB,GPIO_Pin_1);
	}

	if(!GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_5))
	{
		GPIO_SetBits(GPIOB,GPIO_Pin5);
	}
	if(GPIO_ReadOutputDataBit(GPIOB,GPIO_Pin_5))
	{
		GPIO_ResetBits(GPIOB,GPIO_Pin_5);
	}
}
```

>[!success] 使用寄存器的方法

```c
/**
* @brief  Reverse the LED status
* @param  none
* @retval none
*/
void LED_Reverse(void)
{
    // 读取当前LED状态
    uint16_t LEDStatus = GPIOB->IDR;

    // 反转所有LED状态
    LEDStatus ^= (GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_5);

    // 更新LED状态
    GPIOB->ODR = LEDStatus;
}

```

#### OLED模块

>[!note] 直接调用即可,看看函数声明,了解传参

```c
#ifndef __OLED_H
#define __OLED_H

void OLED_Init(void);
void OLED_Clear(void);
void OLED_ShowChar(uint8_t Line, uint8_t Column, char Char);
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String);
void OLED_ShowNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
void OLED_ShowSignedNum(uint8_t Line, uint8_t Column, int32_t Number, uint8_t Length);
void OLED_ShowHexNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);
void OLED_ShowBinNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length);

#endif
```

```c
#include "stm32f10x.h"
#include "OLED_Font.h"


/*引脚配置*/
#define OLED_W_SCL(x)		GPIO_WriteBit(GPIOC, GPIO_Pin_7, (BitAction)(x))
#define OLED_W_SDA(x)		GPIO_WriteBit(GPIOC, GPIO_Pin_6, (BitAction)(x))

/*引脚初始化*/
void OLED_I2C_Init(void)
{
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
	
	GPIO_InitTypeDef GPIO_InitStructure;
 	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_7|GPIO_Pin_6;
 	GPIO_Init(GPIOC, &GPIO_InitStructure);
	
	
	OLED_W_SCL(1);
	OLED_W_SDA(1);
}

/**
  * @brief  I2C开始
  * @param  无
  * @retval 无
  */
void OLED_I2C_Start(void)
{
	OLED_W_SDA(1);
	OLED_W_SCL(1);
	OLED_W_SDA(0);
	OLED_W_SCL(0);
}

/**
  * @brief  I2C停止
  * @param  无
  * @retval 无
  */
void OLED_I2C_Stop(void)
{
	OLED_W_SDA(0);
	OLED_W_SCL(1);
	OLED_W_SDA(1);
}

/**
  * @brief  I2C发送一个字节
  * @param  Byte 要发送的一个字节
  * @retval 无
  */
void OLED_I2C_SendByte(uint8_t Byte)
{
	uint8_t i;
	for (i = 0; i < 8; i++)
	{
		OLED_W_SDA(Byte & (0x80 >> i));
		OLED_W_SCL(1);
		OLED_W_SCL(0);
	}
	OLED_W_SCL(1);	//额外的一个时钟，不处理应答信号
	OLED_W_SCL(0);
}

/**
  * @brief  OLED写命令
  * @param  Command 要写入的命令
  * @retval 无
  */
void OLED_WriteCommand(uint8_t Command)
{
	OLED_I2C_Start();
	OLED_I2C_SendByte(0x78);		//从机地址
	OLED_I2C_SendByte(0x00);		//写命令
	OLED_I2C_SendByte(Command); 
	OLED_I2C_Stop();
}

/**
  * @brief  OLED写数据
  * @param  Data 要写入的数据
  * @retval 无
  */
void OLED_WriteData(uint8_t Data)
{
	OLED_I2C_Start();
	OLED_I2C_SendByte(0x78);		//从机地址
	OLED_I2C_SendByte(0x40);		//写数据
	OLED_I2C_SendByte(Data);
	OLED_I2C_Stop();
}

/**
  * @brief  OLED设置光标位置
  * @param  Y 以左上角为原点，向下方向的坐标，范围：0~7
  * @param  X 以左上角为原点，向右方向的坐标，范围：0~127
  * @retval 无
  */
void OLED_SetCursor(uint8_t Y, uint8_t X)
{
	OLED_WriteCommand(0xB0 | Y);					//设置Y位置
	OLED_WriteCommand(0x10 | ((X & 0xF0) >> 4));	//设置X位置高4位
	OLED_WriteCommand(0x00 | (X & 0x0F));			//设置X位置低4位
}

/**
  * @brief  OLED清屏
  * @param  无
  * @retval 无
  */
void OLED_Clear(void)
{  
	uint8_t i, j;
	for (j = 0; j < 8; j++)
	{
		OLED_SetCursor(j, 0);
		for(i = 0; i < 128; i++)
		{
			OLED_WriteData(0x00);
		}
	}
}

/**
  * @brief  OLED显示一个字符
  * @param  Line 行位置，范围：1~4
  * @param  Column 列位置，范围：1~16
  * @param  Char 要显示的一个字符，范围：ASCII可见字符
  * @retval 无
  */
void OLED_ShowChar(uint8_t Line, uint8_t Column, char Char)
{      	
	uint8_t i;
	OLED_SetCursor((Line - 1) * 2, (Column - 1) * 8);		//设置光标位置在上半部分
	for (i = 0; i < 8; i++)
	{
		OLED_WriteData(OLED_F8x16[Char - ' '][i]);			//显示上半部分内容
	}
	OLED_SetCursor((Line - 1) * 2 + 1, (Column - 1) * 8);	//设置光标位置在下半部分
	for (i = 0; i < 8; i++)
	{
		OLED_WriteData(OLED_F8x16[Char - ' '][i + 8]);		//显示下半部分内容
	}
}

/**
  * @brief  OLED显示字符串
  * @param  Line 起始行位置，范围：1~4
  * @param  Column 起始列位置，范围：1~16
  * @param  String 要显示的字符串，范围：ASCII可见字符
  * @retval 无
  */
void OLED_ShowString(uint8_t Line, uint8_t Column, char *String)
{
	uint8_t i;
	for (i = 0; String[i] != '\0'; i++)
	{
		OLED_ShowChar(Line, Column + i, String[i]);
	}
}

/**
  * @brief  OLED次方函数
  * @retval 返回值等于X的Y次方
  */
uint32_t OLED_Pow(uint32_t X, uint32_t Y)
{
	uint32_t Result = 1;
	while (Y--)
	{
		Result *= X;
	}
	return Result;
}

/**
  * @brief  OLED显示数字（十进制，正数）
  * @param  Line 起始行位置，范围：1~4
  * @param  Column 起始列位置，范围：1~16
  * @param  Number 要显示的数字，范围：0~4294967295
  * @param  Length 要显示数字的长度，范围：1~10
  * @retval 无
  */
void OLED_ShowNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length)
{
	uint8_t i;
	for (i = 0; i < Length; i++)							
	{
		OLED_ShowChar(Line, Column + i, Number / OLED_Pow(10, Length - i - 1) % 10 + '0');
	}
}

/**
  * @brief  OLED显示数字（十进制，带符号数）
  * @param  Line 起始行位置，范围：1~4
  * @param  Column 起始列位置，范围：1~16
  * @param  Number 要显示的数字，范围：-2147483648~2147483647
  * @param  Length 要显示数字的长度，范围：1~10
  * @retval 无
  */
void OLED_ShowSignedNum(uint8_t Line, uint8_t Column, int32_t Number, uint8_t Length)
{
	uint8_t i;
	uint32_t Number1;
	if (Number >= 0)
	{
		OLED_ShowChar(Line, Column, '+');
		Number1 = Number;
	}
	else
	{
		OLED_ShowChar(Line, Column, '-');
		Number1 = -Number;
	}
	for (i = 0; i < Length; i++)							
	{
		OLED_ShowChar(Line, Column + i + 1, Number1 / OLED_Pow(10, Length - i - 1) % 10 + '0');
	}
}

/**
  * @brief  OLED显示数字（十六进制，正数）
  * @param  Line 起始行位置，范围：1~4
  * @param  Column 起始列位置，范围：1~16
  * @param  Number 要显示的数字，范围：0~0xFFFFFFFF
  * @param  Length 要显示数字的长度，范围：1~8
  * @retval 无
  */
void OLED_ShowHexNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length)
{
	uint8_t i, SingleNumber;
	for (i = 0; i < Length; i++)							
	{
		SingleNumber = Number / OLED_Pow(16, Length - i - 1) % 16;
		if (SingleNumber < 10)
		{
			OLED_ShowChar(Line, Column + i, SingleNumber + '0');
		}
		else
		{
			OLED_ShowChar(Line, Column + i, SingleNumber - 10 + 'A');
		}
	}
}

/**
  * @brief  OLED显示数字（二进制，正数）
  * @param  Line 起始行位置，范围：1~4
  * @param  Column 起始列位置，范围：1~16
  * @param  Number 要显示的数字，范围：0~1111 1111 1111 1111
  * @param  Length 要显示数字的长度，范围：1~16
  * @retval 无
  */
void OLED_ShowBinNum(uint8_t Line, uint8_t Column, uint32_t Number, uint8_t Length)
{
	uint8_t i;
	for (i = 0; i < Length; i++)							
	{
		OLED_ShowChar(Line, Column + i, Number / OLED_Pow(2, Length - i - 1) % 2 + '0');
	}
}

/**
  * @brief  OLED初始化
  * @param  无
  * @retval 无
  */
void OLED_Init(void)
{
	uint32_t i, j;
	
	for (i = 0; i < 1000; i++)			//上电延时
	{
		for (j = 0; j < 1000; j++);
	}
	
	OLED_I2C_Init();			//端口初始化
	
	OLED_WriteCommand(0xAE);	//关闭显示
	
	OLED_WriteCommand(0xD5);	//设置显示时钟分频比/振荡器频率
	OLED_WriteCommand(0x80);
	
	OLED_WriteCommand(0xA8);	//设置多路复用率
	OLED_WriteCommand(0x3F);
	
	OLED_WriteCommand(0xD3);	//设置显示偏移
	OLED_WriteCommand(0x00);
	
	OLED_WriteCommand(0x40);	//设置显示开始行
	
	OLED_WriteCommand(0xA1);	//设置左右方向，0xA1正常 0xA0左右反置
	
	OLED_WriteCommand(0xC8);	//设置上下方向，0xC8正常 0xC0上下反置

	OLED_WriteCommand(0xDA);	//设置COM引脚硬件配置
	OLED_WriteCommand(0x12);
	
	OLED_WriteCommand(0x81);	//设置对比度控制
	OLED_WriteCommand(0xCF);

	OLED_WriteCommand(0xD9);	//设置预充电周期
	OLED_WriteCommand(0xF1);

	OLED_WriteCommand(0xDB);	//设置VCOMH取消选择级别
	OLED_WriteCommand(0x30);

	OLED_WriteCommand(0xA4);	//设置整个显示打开/关闭

	OLED_WriteCommand(0xA6);	//设置正常/倒转显示

	OLED_WriteCommand(0x8D);	//设置充电泵
	OLED_WriteCommand(0x14);

	OLED_WriteCommand(0xAF);	//开启显示
		
	OLED_Clear();				//OLED清屏
}

```

#### EXTI外部中断按键计数
>[!note] 上拉输入对应下降沿信号，下拉输入对应上升沿信号

>[!attention] 注意中断函数的名字也要跟着引脚变, EXTI_Line0 对应 EXTI0_IRQHandler

```c
#include "stm32f10x.h"                  // Device header

uint16_t CountSensor_Count;	//全局变量，用于计数,在主程序中用获取函数来获取
/**
  * brief：Count Sensor Initiate
  * param：none
  * retval：none
  */
void CountSensor_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);		
	//开启GPIOA的时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE);		
	//开启AFIO的时钟，外部中断必须开启AFIO的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING; //可以上拉
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);						
	//将PA0引脚初始化为浮空
	
	/*AFIO选择中断引脚*/
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource0);
	//将外部中断的0号线映射到GPIOA，即选择PA0为外部中断引脚
	
	/*EXTI初始化*/
	EXTI_InitTypeDef EXTI_InitStructure;						
	//定义结构体变量
	EXTI_InitStructure.EXTI_Line = EXTI_Line0;					
	//选择配置外部中断的0号线
	EXTI_InitStructure.EXTI_LineCmd = ENABLE;					
	//指定外部中断线使能
	EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;			
	//指定外部中断线为中断模式
	EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;		
	//指定外部中断线为下降沿触发
	EXTI_Init(&EXTI_InitStructure);								
	//将结构体变量交给EXTI_Init，配置EXTI外设
	
	/*NVIC中断分组*/
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);				
	//配置NVIC为分组2		
	//即抢占优先级范围：0~3，响应优先级范围：0~3
	//此分组配置在整个工程中仅需调用一次
	//若有多个中断，可以把此代码放在main函数内，while循环之前
	//若调用多次配置分组的代码，则后执行的配置会覆盖先执行的配置
	
	/*NVIC配置*/
	NVIC_InitTypeDef NVIC_InitStructure;			
	//定义结构体变量
	NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;
	//选择配置NVIC的EXTI0线
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;	
	//指定NVIC线路使能
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;
	//指定NVIC线路的抢占优先级为1
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;			
	//指定NVIC线路的响应优先级为1
	NVIC_Init(&NVIC_InitStructure);								
	//将结构体变量交给NVIC_Init，配置NVIC外设
}

/**
  * brief: Get the number from countSensor
  * param: none
  * retval: Count ( 0~65535 )
  */
uint16_t CountSensor_Get(void)
{
	return CountSensor_Count;
}


/**
  * brief: EXTI15_10 external func
  * param: none
  * retval: none
  * tips：此函数为中断函数，无需调用，中断触发后自动执行
  *           函数名为预留的指定名称,是和通道一一对应的，可以从启动文件复制
  *           请确保函数名正确，不能有任何差异，否则中断函数将不能进入
  */
void EXTI0_IRQHandler(void)
{
	if (EXTI_GetITStatus(EXTI_Line0) == SET)		
	//判断是否是外部中断0号线触发的中断
	{
		/*如果出现数据乱跳的现象，可再次判断引脚电平，以避免抖动*/
		if (GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_0) == 0)
		{
			CountSensor_Count ++;					
			//计数值自增一次
		}
		EXTI_ClearITPendingBit(EXTI_Line0);		
		//清除外部中断0号线的中断标志位
		//中断标志位必须清除
		//否则中断将连续不断地触发，导致主程序卡死
	}
}

```

#### EXTI外部中断旋转计数
>[!note] 没有旋转编码器,暂时没跑过这个鬼东西 

```c
#include "stm32f10x.h"                  // Device header

int16_t Encoder_Count;				//全局变量，用于计数旋转编码器的增量值

/**
  * 函    数：旋转编码器初始化
  * 参    数：无
  * 返 回 值：无
  */
void Encoder_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);		
	//开启GPIOB的时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE);		
	//开启AFIO的时钟，外部中断必须开启AFIO的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);						
	//将PB0和PB1引脚初始化为上拉输入
	
	/*AFIO选择中断引脚*/
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOB, GPIO_PinSource0);
	//将外部中断的0号线映射到GPIOB，即选择PB0为外部中断引脚
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOB, GPIO_PinSource1);
	//将外部中断的1号线映射到GPIOB，即选择PB1为外部中断引脚
	
	/*EXTI初始化*/
	EXTI_InitTypeDef EXTI_InitStructure;						
	//定义结构体变量
	EXTI_InitStructure.EXTI_Line = EXTI_Line0 | EXTI_Line1;		
	//选择配置外部中断的0号线和1号线
	EXTI_InitStructure.EXTI_LineCmd = ENABLE;					
	//指定外部中断线使能
	EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;			
	//指定外部中断线为中断模式
	EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Falling;		
	//指定外部中断线为下降沿触发
	EXTI_Init(&EXTI_InitStructure);								
	//将结构体变量交给EXTI_Init，配置EXTI外设
	
	/*NVIC中断分组*/
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);				
	//配置NVIC为分组2	
	//即抢占优先级范围：0~3，响应优先级范围：0~3
	//此分组配置在整个工程中仅需调用一次
	//若有多个中断，可以把此代码放在main函数内，while循环之前
	//若调用多次配置分组的代码，则后执行的配置会覆盖先执行的配置
	
	/*NVIC配置*/
	NVIC_InitTypeDef NVIC_InitStructure;						
	//定义结构体变量
	NVIC_InitStructure.NVIC_IRQChannel = EXTI0_IRQn;			
	//选择配置NVIC的EXTI0线
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;				
	//指定NVIC线路使能
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;	
	//指定NVIC线路的抢占优先级为1
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;			
	//指定NVIC线路的响应优先级为1
	NVIC_Init(&NVIC_InitStructure);								
	//将结构体变量交给NVIC_Init，配置NVIC外设

	NVIC_InitStructure.NVIC_IRQChannel = EXTI1_IRQn;			
	//选择配置NVIC的EXTI1线
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;				
	//指定NVIC线路使能
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 1;	
	//指定NVIC线路的抢占优先级为1
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 2;			
	//指定NVIC线路的响应优先级为2
	NVIC_Init(&NVIC_InitStructure);								
	//将结构体变量交给NVIC_Init，配置NVIC外设
}

/**
  * 函    数：旋转编码器获取增量值
  * 参    数：无
  * 返 回 值：自上此调用此函数后，旋转编码器的增量值
  */
int16_t Encoder_Get(void)
{
	/*使用Temp变量作为中继，目的是返回Encoder_Count后将其清零*/
	/*在这里，也可以直接返回Encoder_Count
	  但这样就不是获取增量值的操作方法了
	  也可以实现功能，只是思路不一样*/
	int16_t Temp;
	Temp = Encoder_Count;
	Encoder_Count = 0;
	return Temp;
}

/**
  * 函    数：EXTI0外部中断函数
  * 参    数：无
  * 返 回 值：无
  * 注意事项：此函数为中断函数，无需调用，中断触发后自动执行
  *           函数名为预留的指定名称，可以从启动文件复制
  *           请确保函数名正确，不能有任何差异，否则中断函数将不能进入
  */
void EXTI0_IRQHandler(void)
{
	if (EXTI_GetITStatus(EXTI_Line0) == SET)		
	//判断是否是外部中断0号线触发的中断
	{
		/*如果出现数据乱跳的现象，可再次判断引脚电平，以避免抖动*/
		if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) == 0)
		{
			if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_1) == 0)		
			//PB0的下降沿触发中断，此时检测另一相PB1的电平，目的是判断旋转方向
			{
				Encoder_Count --;					
				//此方向定义为反转，计数变量自减
			}
		}
		EXTI_ClearITPendingBit(EXTI_Line0);			
		//清除外部中断0号线的中断标志位
		//中断标志位必须清除
		//否则中断将连续不断地触发，导致主程序卡死
	}
}

/**
  * 函    数：EXTI1外部中断函数
  * 参    数：无
  * 返 回 值：无
  * 注意事项：此函数为中断函数，无需调用，中断触发后自动执行
  *           函数名为预留的指定名称，可以从启动文件复制
  *           请确保函数名正确，不能有任何差异，否则中断函数将不能进入
  */
void EXTI1_IRQHandler(void)
{
	if (EXTI_GetITStatus(EXTI_Line1) == SET)		
	//判断是否是外部中断1号线触发的中断
	{
		/*如果出现数据乱跳的现象，可再次判断引脚电平，以避免抖动*/
		if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_1) == 0)
		{
			if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_0) == 0)		
			//PB1的下降沿触发中断，此时检测另一相PB0的电平，目的是判断旋转方向
			{
				Encoder_Count ++;					
				//此方向定义为正转，计数变量自增
			}
		}
		EXTI_ClearITPendingBit(EXTI_Line1);			
		//清除外部中断1号线的中断标志位
		//中断标志位必须清除
		//否则中断将连续不断地触发，导致主程序卡死
	}
}

```

#### TIM定时器内部中断初始化以及中断服务函数

>[!warning] 一定要注意中断服务函数例如(TIM2_IRQHandler) 和TIM要对应
```c
 OLED_ShowNum(1, 7, timer_cnt, 5);            // OLED不断刷新显示秒数值
 OLED_ShowNum(2, 5, TIM_GetCounter(TIM2), 5); // 可以在主函数中用OLED实时显示定时器中计数器的值,会到自动重装值之后置零
```

```c
#include "stm32f10x.h" // Device header

extern uint16_t timer_cnt;
// 如果没有用extern语句去寻找其他文件中的计数变量timemr_cnt
// 则可以把最底下的定时器中断函数，复制到使用它的地方


/**
 * brief: Timer interrupt iinitialization
 * param：none
 * retval：none
 */
void Timer_Init(void)
{
    /*开启时钟*/
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE); //开启TIM2的时钟

    /*配置时钟源*/
    TIM_InternalClockConfig(TIM2); //选择TIM2为内部时钟，若不调用此函数，TIM默认也为内部时钟

    /*时基单元初始化*/
    TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;                
	//定义结构体变量
    TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;      
     //时钟分频，选择不分频，此参数用于配置滤波器时钟，不影响时基单元功能
    TIM_TimeBaseInitStructure.TIM_CounterMode   = TIM_CounterMode_Up; 
	//计数器模式，选择向上计数
    TIM_TimeBaseInitStructure.TIM_Period        = 10000 - 1;         
     //计数周期，即ARR的值
    TIM_TimeBaseInitStructure.TIM_Prescaler     = 7200 - 1;          
	//预分频器，即PSC的值
	//对72M进行7200分频得到10K的计数频率，在10K的计数频率下，计10000个数，就是1s的时间
    TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;
	//重复计数器，高级定时器才会用到
    TIM_TimeBaseInit(TIM2, &TIM_TimeBaseInitStructure); 
	//将结构体变量交给TIM_TimeBaseInit，配置TIM2的时基单元
	
    /*中断输出配置*/
    TIM_ClearFlag(TIM2, TIM_FLAG_Update); 
	//清除定时器更新标志位
	//TIM_TimeBaseInit函数末尾，手动产生了更新事件
	//若不清除此标志位，则开启中断后，会立刻进入一次中断
     //如果不介意此问题，则不清除此标志位也可

    TIM_ITConfig(TIM2, TIM_IT_Update, ENABLE); //开启TIM2的更新中断

    /*NVIC中断分组*/
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); 
    //配置NVIC为分组2
    //即抢占优先级范围：0~3，响应优先级范围：0~3
    //此分组配置在整个工程中仅需调用一次
    //若有多个中断，可以把此代码放在main函数内，while循环之前
    //若调用多次配置分组的代码，则后执行的配置会覆盖先执行的配置

    /*NVIC配置*/
    NVIC_InitTypeDef NVIC_InitStructure;                              //定义结构体变量
    NVIC_InitStructure.NVIC_IRQChannel                   = TIM2_IRQn; //选择配置NVIC的TIM2线
    NVIC_InitStructure.NVIC_IRQChannelCmd                = ENABLE;    //指定NVIC线路使能
    NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 2;         //指定NVIC线路的抢占优先级为2
    NVIC_InitStructure.NVIC_IRQChannelSubPriority        = 1;         //指定NVIC线路的响应优先级为1
    NVIC_Init(&NVIC_InitStructure);                                 //将结构体变量交给NVIC_Init，配置NVIC外设

    /*TIM使能*/
    TIM_Cmd(TIM2, ENABLE); //使能TIM2，定时器开始运行
}

/**
 * brief: Timer interrupt
 * param：none
 * retval：none
 */
void TIM2_IRQHandler(void)
{
    // 检查是否触发了定时器 TIM2 的更新中断
    if (TIM_GetITStatus(TIM2, TIM_IT_Update) == SET) {
        // 如果触发了更新中断，增加计时器变量 timer_cnt 的值
        timer_cnt++;
        // 清除中断挂起标志位，以便允许下一次中断触发
        TIM_ClearITPendingBit(TIM2, TIM_IT_Update);
    }
}


```
#### TIM&KEY按键控制定时器暂停
>[!question] 发现个问题就是KEY1KEY2长按的时候定时器暂停

```c
/**
 * @brief Start or stop timer TIM2 based on key presses
 * @param None
 * @retval None
 */
void Timer_KEYStop(void)
{
    if (Key_Scan() == KEY1_ON) {
        // 如果检测到 KEY1_ON 被按下，停止定时器 TIM2
        TIM_Cmd(TIM2, DISABLE);
    }
    if (Key_Scan() == KEY2_ON) {
        // 如果检测到 KEY2_ON 被按下，启动定时器 TIM2
        TIM_Cmd(TIM2, ENABLE);
    }
}

```
#### TIM&EXTI定时器外部中断

```c

#include "stm32f10x.h" // Device header


/**
 * brief：Timer External interrupt iinitialization
 * param：none
 * retval：none
 */
void Timer_Init(void)
{
	/*开启时钟*/
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);  //开启TIM2的时钟
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE); //开启GPIOA的时钟

	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure); //将PA0引脚初始化为上拉输入

	/*外部时钟配置*/
	//此函数配置为外部时钟，定时器相当于计数器
	TIM_ETRClockMode2Config(TIM2,TIM_ExtTRGPSC_OFF,TIM_ExtTRGPolarity_NonInverted, 0x0F);
	//选择外部时钟模式2，时钟从TIM_ETR引脚输入
	//注意TIM2的ETR引脚固定为PA0，无法随意更改
	//最后一个滤波器参数加到最大0x0F，可滤除时钟信号抖动

	/*时基单元初始化*/
	TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;				
	//定义结构体变量
	TIM_TimeBaseInitStructure.TIM_ClockDivision = TIM_CKD_DIV1;		
	//时钟分频，选择不分频，此参数用于配置滤波器时钟，不影响时基单元功能
	TIM_TimeBaseInitStructure.TIM_CounterMode = TIM_CounterMode_Up; 
	//计数器模式，选择向上计数
	TIM_TimeBaseInitStructure.TIM_Period = 10 - 1;					
	//计数周期，即ARR的值
	TIM_TimeBaseInitStructure.TIM_Prescaler = 1 - 1;				
	//预分频器，即PSC的值
	TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;			
	//重复计数器，高级定时器才会用到
	TIM_TimeBaseInit(TIM2, &TIM_TimeBaseInitStructure);				
	//将结构体变量交给TIM_TimeBaseInit，配置TIM2的时基单元

	/*中断输出配置*/
	TIM_ClearFlag(TIM2, TIM_FLAG_Update);
	//清除定时器更新标志位	  
	// TIM_TimeBaseInit函数末尾，手动产生了更新事件
	//若不清除此标志位，则开启中断后，会立刻进入一次中断
	//如果不介意此问题，则不清除此标志位也可

	TIM_ITConfig(TIM2, TIM_IT_Update, ENABLE); //开启TIM2的更新中断

	/*NVIC中断分组*/
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2); 
	//配置NVIC为分组2
	//即抢占优先级范围：0~3，响应优先级范围：0~3
	//此分组配置在整个工程中仅需调用一次
	//若有多个中断，可以把此代码放在main函数内，while循环之前
	//若调用多次配置分组的代码，则后执行的配置会覆盖先执行的配置

	/*NVIC配置*/
	NVIC_InitTypeDef NVIC_InitStructure;					  
	//定义结构体变量
	NVIC_InitStructure.NVIC_IRQChannel = TIM2_IRQn;			  
	//选择配置NVIC的TIM2线
	NVIC_InitStructure.NVIC_IRQChannelCmd = ENABLE;			  
	//指定NVIC线路使能
	NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 2; 
	//指定NVIC线路的抢占优先级为2
	NVIC_InitStructure.NVIC_IRQChannelSubPriority = 1;		  
	//指定NVIC线路的响应优先级为1
	NVIC_Init(&NVIC_InitStructure);							  
	//将结构体变量交给NVIC_Init，配置NVIC外设

	/*TIM使能*/
	TIM_Cmd(TIM2, ENABLE); //使能TIM2，定时器开始运行
}

/**
 * brief：Get count number from the 
 * param：none
 * retval：the count number ，range：0~65535
 */
uint16_t Timer_GetCounter(void)
{
	return TIM_GetCounter(TIM2); //返回定时器TIM2的CNT
}

/* 定时器中断函数，可以复制到使用它的地方
void TIM2_IRQHandler(void)
{
	if (TIM_GetITStatus(TIM2, TIM_IT_Update) == SET)
	{
		// sth
		TIM_ClearITPendingBit(TIM2, TIM_IT_Update);
	}
}
*/

```

#### PWM&LED呼吸灯
>[!note]
>和之前的TIM内部中断初始化相比,其他都是一样的,但PWM我们不配置中断,因此删去TIM的相关中断配置函数以及NVIC的配置函数: 即(//*中断输出配置*///*NVIC中断分组*//*NVIC配置*//)

>[!warning] 一定要注意个别TIM的函数( TIM_OC3Init,TIM_SetCompare3 ) 要对应外设引脚的TIM_CH通道 (可以查引脚表)

>[!note] ARR 确定 PWM 分辨率,再和 PSC 共同确定 PWM 的频率; 还和 CRR 共同确定 PWM 占空比

```c
#include "stm32f10x.h" // Device header

/**
 * brief: Timer Output PWM iinitialization
 * param：none
 * retval：none
 */
void PWM_Init(void)
{
    /*开启时钟*/
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM3, ENABLE);  //开启TIM2的时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE); //开启GPIOA的时钟

    /*GPIO重映射*/
    //	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO, ENABLE);			//开启AFIO的时钟，重映射必须先开启AFIO的时钟
    //	GPIO_PinRemapConfig(GPIO_PartialRemap1_TIM2, ENABLE);			//将TIM2的引脚部分重映射，具体的映射方案需查看参考手册
    //	GPIO_PinRemapConfig(GPIO_Remap_SWJ_JTAGDisable, ENABLE);		//将JTAG引脚失能，作为普通GPIO引脚使用

    /*GPIO初始化*/
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Mode  = GPIO_Mode_AF_PP; //将PA0引脚初始化为复用推挽输出 //受外设控制的引脚，均需要配置为复用模式
    GPIO_InitStructure.GPIO_Pin   = GPIO_Pin_0;      //
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_InitStructure);
    /*配置时钟源*/
    TIM_InternalClockConfig(TIM3); //选择TIM2为内部时钟，若不调用此函数，TIM默认也为内部时钟

    /*时基单元初始化*/
    TIM_TimeBaseInitTypeDef TIM_TimeBaseInitStructure;                    //定义结构体变量
    TIM_TimeBaseInitStructure.TIM_ClockDivision     = TIM_CKD_DIV1;       //时钟分频，选择不分频，此参数用于配置滤波器时钟，不影响时基单元功能
    TIM_TimeBaseInitStructure.TIM_CounterMode       = TIM_CounterMode_Up; //计数器模式，选择向上计数
    TIM_TimeBaseInitStructure.TIM_Period            = 100 - 1;            //计数周期，即ARR的值 
    TIM_TimeBaseInitStructure.TIM_Prescaler         = 720 - 1;            //预分频器，即PSC的值
    TIM_TimeBaseInitStructure.TIM_RepetitionCounter = 0;                  //重复计数器，高级定时器才会用到
    TIM_TimeBaseInit(TIM3, &TIM_TimeBaseInitStructure);                   //将结构体变量交给TIM_TimeBaseInit，配置TIM2的时基单元

    /*输出比较初始化*/
    TIM_OCInitTypeDef TIM_OCInitStructure;                        //定义结构体变量
    TIM_OCStructInit(&TIM_OCInitStructure);                       //结构体初始化，若结构体没有完整赋值
                                                                  //则最好执行此函数，给结构体所有成员都赋一个默认值,下面再一一修改
                                                                  //避免结构体初值不确定的问题
    TIM_OCInitStructure.TIM_OCMode      = TIM_OCMode_PWM1;        //输出比较模式，选择PWM模式1
    TIM_OCInitStructure.TIM_OCPolarity  = TIM_OCPolarity_High;    //输出极性，选择为高，若选择极性为低，则输出高低电平取反
    TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable; //输出使能
    TIM_OCInitStructure.TIM_Pulse       = 0;                      //初始的CCR值
    TIM_OC3Init(TIM3, &TIM_OCInitStructure);                      //将结构体变量交给TIM_OC1Init，配置TIM2的输出比较通道1


    /*TIM使能*/
    TIM_Cmd(TIM3, ENABLE); //使能TIM2，定时器开始运行
}

/**
 * @brief :Set PWM CCR
 * @param Compare: The value to be written to CCR, ranging from 0 to 100.
 * @retval :none
 */
void PWM_SetCompare1(uint16_t Compare) 
// CCR和ARR共同决定占空比，此函数仅设置CCR的值，并不是占空比
// Duty = CCR / (ARR + 1)
{
    TIM_SetCompare3(TIM3, Compare); //设置CCR1的值
}
```

#### PWM舵机角度

在使用PWM时，通常需要配置定时器的 ARR（自动重装载寄存器）和 CCR（捕获/比较寄存器）的值，以确定 PWM 的占空比（Duty Cycle）和频率。下面是一些通用的设置说明：

1. 首先，选择你要的 PWM 频率，这里是 50 Hz。
2. 计算 `PWM 周期（周期 = 1 / 频率）`。对于 50 Hz，周期是 20 毫秒（1 / 50 秒）。
3. 设置定时器的 ARR 寄存器的值为 PWM 周期的计数值。这将决定定时器何时重装载并重新开始计数。对于 50 Hz，ARR 的值将是定时器时钟频率乘以 0.02 秒（20 毫秒），即 `ARR = 定时器时钟频率 * 0.02`。
4. 然后，确定 PWM 的占空比。占空比是高电平（ON）时间占整个周期的比例，通常以百分比表示。如果你想要一个 50% 的占空比，那么高电平时间将是周期的一半，也就是 10 毫秒。
5. 为了实现所需的占空比，设置 CCR 寄存器的值为占空比乘以 ARR 的值。在这种情况下，`CCR = 0.5 * ARR`。
6. 最后，将 ARR 和 CCR 的值加载到你的定时器中，以启用 PWM 输出。

- PWM模块源文件中
```c
//这是一个演示的例子,这里的ARR和PSC先取以下值,便于理解舵机线性转换的计算

TIM_TimeBaseInitStructure.TIM_Period = 20000 - 1;	//计数周期，即ARR的值
TIM_TimeBaseInitStructure.TIM_Prescaler = 72 - 1;	//预分频器，即PSC的值

```

- 主程序源文件中
```c
	/*显示静态字符串*/
	OLED_ShowString(1, 1, "Angle:");	//1行1列显示字符串Angle:
	
	while (1)
	{
		KeyNum = Key_GetNum();			//获取按键键码
		if (KeyNum == 1)				//按键1按下
		{
			Angle += 30;				//角度变量自增30
			if (Angle > 180)			//角度变量超过180后
			{
				Angle = 0;				//角度变量归零
			}
		}
		Servo_SetAngle(Angle);			//设置舵机的角度为角度变量
		OLED_ShowNum(1, 7, Angle, 3);	//OLED显示角度变量
	}
```

- 在模块源文件中
```c
#include "stm32f10x.h"                  // Device header
#include "PWM.h"

/**
  * brief: Servo Initialization
  * param: none
  * retval: none
  */
void Servo_Init(void)
{
	PWM_Init();//初始化舵机的底层PWM
}

/**
  * brief：Steering gear setting angle
  * param：Angle The steering angle to be set, [ range: 0-180 ]
  * retval：none
  */
void Servo_SetAngle(float Angle)
{
	//将角度线性变换，对应到舵机要求的占空比范围上
	PWM_SetCompare2(Angle / 180 * 2000 + 500);	//线性转换成CCR 从而设置占空比
	// CRR的取值范围是500~2500，对应的是0.5ms~2.5ms
	// Duty = CCR / (ARR + 1)
}

```

#### PWM直流电机速度

- PWM模块源文件中
```c
	TIM_TimeBaseInitStructure.TIM_Period = 100 - 1;                 //计数周期，即ARR的值
	TIM_TimeBaseInitStructure.TIM_Prescaler = 36 - 1;               //预分频器，即PSC的值
	//PWM 频率足够大的时候,此时属于超声波,此时电机转速就不会吵了(人耳范围是 20Hz 到 20KHz )
	//我们减小 PWM 频率的时候,可以通过减小预分频器 PSC 来完成,这样不会影响占空比
```

- 主程序源文件中
```c
/*显示静态字符串*/
	OLED_ShowString(1, 1, "Speed:");		//1行1列显示字符串Speed:
	
	while (1)
	{
		KeyNum = Key_GetNum();				//获取按键键码
		if (KeyNum == 1)					//按键1按下
		{
			Speed += 20;					//速度变量自增20
			if (Speed > 100)				//速度变量超过100后
			{
				Speed = -100;				//速度变量变为-100
											//此操作会让电机旋转方向突然改变，可能会因供电不足而导致单片机复位
											//若出现了此现象，则应避免使用这样的操作
			}
		}
		Motor_SetSpeed(Speed);				//设置直流电机的速度为速度变量
		OLED_ShowSignedNum(1, 7, Speed, 3);	//OLED显示速度变量
	}
```

- PWM模块源文件中
```c
#include "stm32f10x.h"                  // Device header
#include "PWM.h"

/**
  * brief: Motor Initalization
  * param: none
  * retval: none
  */
void Motor_Init(void) //这里多了一个GPIO引脚的开启,这两个引脚是直流电机用于控制正反两个方向的
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);		//开启GPIOA的时钟
	
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4 | GPIO_Pin_5;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);						//将PA4和PA5引脚初始化为推挽输出	
	
	PWM_Init();													//初始化直流电机的底层PWM
}

/**
  * brief： DC motor set speed
  * param：Speed ，[ range：-100~100 ]
  * retval: none
  */
void Motor_SetSpeed(int8_t Speed)
{
	if (Speed >= 0)							//如果设置正转的速度值
	{
		GPIO_SetBits(GPIOA, GPIO_Pin_4);	//PA4置高电平
		GPIO_ResetBits(GPIOA, GPIO_Pin_5);	//PA5置低电平，设置方向为正转
		PWM_SetCompare3(Speed);				//PWM设置为速度值
	}
	else									//否则，即设置反转的速度值
	{
		GPIO_ResetBits(GPIOA, GPIO_Pin_4);	//PA4置低电平
		GPIO_SetBits(GPIOA, GPIO_Pin_5);	//PA5置高电平，设置方向为反转
		PWM_SetCompare3(-Speed);			//PWM设置为负的速度值，因为此时速度值为负数，而PWM只能给正数
	}
}

```