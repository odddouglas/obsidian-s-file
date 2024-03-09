#### PWM流水呼吸灯（copy）
- 注意是`stc15f2k60s2`
```c

#include "stc15f2k60s2.h"

sbit S4 = P3 ^ 3;
sbit S7 = P3 ^ 0;
bit stat_L = 0;
bit stat_4 = 0;
bit pause = 0;
bit stat_7 = 0;
unsigned char pwm_duty;
unsigned int count, count1;
unsigned char code SMG_duanma[] = {0x3F, 0x06, 0x5B, 0x4F, 0x66, 0x6D, 0x7D, 0x07, 0x7F, 0x6F, 0x77, 0x7C, 0x39, 0x5E, 0x79, 0x71, 0x40, 0x80};
unsigned char Stat_LED = 0xff;
char led_pos = 0;

void SelectHC573(unsigned char channel)
{
    switch (channel)
    {
    case 4:
        P2 = (P2 & 0X1F) | 0X80;
        break;

    case 5:
        P2 = (P2 & 0X1F) | 0XA0;
        break;

    case 6:
        P2 = (P2 & 0X1F) | 0XC0;
        break;

    case 7:
        P2 = (P2 & 0X1F) | 0XE0;
        break;

    case 0:
        P2 = (P2 & 0X1F) | 0X00;
        break;
    }
}

void DisplaySMG_bit(unsigned char pos, unsigned char value)
{
    EA = 0;
    P0 = 0XFF;
    SelectHC573(7);
    P0 = 0XFF;
    SelectHC573(6);
    P0 = 0X01 << pos;
    SelectHC573(7);
    P0 = value;
    SelectHC573(0);
    EA = 1;
}

void DisplaySMGAll_Off()
{
    P0 = 0XFF;
    SelectHC573(7);
    P0 = 0XFF;
    SelectHC573(6);
    P0 = 0XFF;
    SelectHC573(7);
    P0 = 0XFF;
}

void DelaySMG(unsigned int t)
{
    while (t--)
        ;
}

unsigned char Show[] = {0, 0, 0, 0, 0, 0, 0, 0};
void CatchShow()
{
    Show[0] = ~SMG_duanma[led_pos];
    Show[1] = 0xff;
    Show[2] = 0xff;
    Show[3] = 0xff;
    Show[4] = 0xff;
    if (pwm_duty > 99)
        Show[5] = ~SMG_duanma[pwm_duty / 100];
    else
        Show[5] = 0xff;
    if (pwm_duty > 9)
        Show[6] = ~SMG_duanma[(pwm_duty % 100) / 10];
    else
        Show[6] = 0xff;
    Show[7] = ~SMG_duanma[pwm_duty % 10];
}

void DisplaySMG()
{
    CatchShow();
    DisplaySMG_bit(0, Show【0】);
    DelaySMG(500);

    DisplaySMG_bit(5, Show【5】);
    DelaySMG(500);

    DisplaySMG_bit(6, Show【6】);
    DelaySMG(500);

    DisplaySMG_bit(7, Show【7】);
    DelaySMG(500);

    DisplaySMGAll_Off();
}

void InitSystem()
{
    SelectHC573(4);
    P0 = ~0X18;
    SelectHC573(5);
    P0 = 0X00;
    SelectHC573(7);
    P0 = 0XFF;
    SelectHC573(6);
    P0 = 0XFF;
    SelectHC573(7);
    P0 = 0XFF;
}

void DelayK(unsigned int t)
{
    while (t--)
        ;
}

void Timer0Init(void) // 100微秒@12.000MHz
{
    AUXR &= 0x7F; //定时器时钟12T模式
    TMOD &= 0xF0; //设置定时器模式
    TMOD |= 0x02; //设置定时器模式
    TL0 = 0x9C;   //设置定时初值
    TH0 = 0x9C;   //设置定时重载值
    TF0 = 0;      //清除TF0标志
    TR0 = 1;      //定时器0开始计时

    EA = 1;
    ET0 = 1;
}

void DisplayLED_bit(unsigned char pos, unsigned char value)
{
    switch (value)
    {
    case 1:
        P0 = 0XFF;
        Stat_LED &= ~(0x01 << pos);
        SelectHC573(4);
        P0 = Stat_LED;
        SelectHC573(0);
        break;
    case 0:
        P0 = 0XFF;
        Stat_LED |= 0x01 << pos;
        SelectHC573(4);
        P0 = Stat_LED;
        SelectHC573(0);
        break;
    }
}

void GetPWM_duty()
{
    if (count1 < 5000)
    {
        pwm_duty = count1 / 50;
    }
    else
    {
        pwm_duty = 100 - ((count1 - 5000) / 50);
    }
}
void DisplayPWM()
{
    if (count <= pwm_duty)
    {
        stat_L = 1;
    }
    else
    {
        stat_L = 0;
    }
    if (count == 100)
        count = 0;
    DisplayLED_bit(led_pos, stat_L);
}

void ExecuteS4()
{
    GetPWM_duty();
    if (pause == 0)
    {
        // DisplaySMGAll_Off();
        count1++;
        GetPWM_duty();
        if (stat_4 == 0)
        {
            // DisplayPWM();
            if (count1 == 10000)
            {
                led_pos++;
                count1 = 0;
                if (led_pos == 8)
                {
                    led_pos = 0;
                }
            }
        }
        else
        {
            // DisplaySMGAll_Off();
            // DisplayPWM();
            if (count1 == 10000)
            {
                led_pos--;
                count1 = 0;
                if (led_pos < 0)
                {
                    led_pos = 7;
                }
            }
        }
    }
}
void ServiceTimer0() interrupt 1
{
    count++;
    ExecuteS4();
    DisplayPWM();
}

void ScanKeys()
{
    if (S4 == 0)
    {
        DelayK(100);
        if (S4 == 0)
        {
            K4++;
            if
                while (S4 == 0)
                {
                    pause = 1;
                    // DisplayPWM();
                }
            stat_4 = ~stat_4;
            pause = 0;
        }
    }
    // else
    // {
    // pause = 0;
    // DisplayPWM();
    // }
    if (S7 == 0)
    {
        DelayK(100);
        if (S7 == 0)
        {
            while (S7 == 0)
            {
                pause = 1;
                // DisplayPWM();
                DisplaySMG();
            }
            pause = 0;
        }
    }
    // else
    // {
    // pause = 0;
    // DisplayPWM();
    // }
}

void main()
{
    InitSystem();
    while (S4 == 1)
        ;
    while (S4 == 0)
        ;
    Timer0Init();
    while (1)
    {
        ScanKeys();
    }
}
```
