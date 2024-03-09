
----
# FUNCTION
## 函数-循环-百元百鸡(sy4-1)
	//中國古代數學家張丘建提出的「百雞問題」：
	//一隻大公雞值五個錢，一隻母雞值三個錢，三個小雞值一個錢。
	// 現有100個錢，要剛好買100隻雞，且三種雞都有。
```c
#include<stdio.h>
int main()
{
	int x,y,z;
	for(x=1;5*x<100;x++)
	{
		for(y=1;3*y<100;y++)
		{
			for(z=1;z/3<100;z++)
			{
				if(z%3==0&&(5*x+3*y+z/3==100)&&z==(100-x-y))
				{	
					printf("%5d%5d%5d\n",x,y,z);
				}
			}
		}
	}
	return 0;
}

```

## 函数-循环-兑换硬币方案数(sy4-2)
	//將一筆零錢兌換成1分、2分、5分的硬幣，要求每種硬幣至少1枚
	//從鍵盤上輸入零錢的總數（整數，單位為分），顯示這筆零錢可有的兌換方案數
```c
#include<stdio.h>
int main()
{
	int count=0,coin,uno,dos,cinco;
	scanf("%d",&coin);
	for(cinco=1;cinco*5<coin;cinco++)
	{
		for(dos=1;dos*2<coin;dos++)
		{
			for(uno=1;uno<coin;uno++)
			{
				if(uno+dos*2+cinco*5==coin)
				{
					count++;
				}
			}
		}
	}
	getchar();
	printf("count=%d",count);
	return 0;
}
```

## 函数-循环-菱形图片(sy4-3)
	//（要求以第30列為對稱軸，即菱形的頂點在第30列）。
	
![[Pasted image 20221222145642.png]]

```c
#include<stdio.h> 
int main()
{
	int NUM,SPACE,H;
	for(NUM=1;NUM<=7;NUM++)
	{
		for(SPACE=1;SPACE<=30-NUM;SPACE++)
		{printf(" ");}
		for(H=1;H<=2*NUM-1;H++)
		{printf("%d",NUM);}
		printf("\n");
    }
		
	for(NUM=6;NUM>=1;NUM--)
	{
		for(SPACE=1;SPACE<=30-NUM;SPACE++)
		{printf(" ");}
		for(H=1;H<=2*NUM-1;H++)
		{printf("%d",NUM);}
		printf("\n");
	 }
	return 0;
	
}
```

## 函数-循环-水仙花数(sy4-4)
	//輸入正整數n[3，7]。
	//輸出n位水仙花數
```c
#include <stdio.h> 						//经典水仙花 


int powm (int m,int n)/*m的n次方*///不调用数学函数库 
{
	int x,y=1;
	for (x=1;x<=n;x++)
	y=m*y; //一直累计下去y一直乘以m 
	return y;
}

int main ()
{
	int a,b,sum,i,t;/*a为位数，b为水仙花数(后面用sum等价输出)，i为个位*/
	scanf ("%i",&a);
	
	for (b=powm(10,a-1);b<powm(10,a)-1;b++)
	/*将b限定在那个位数范围内的数（如1000~9999），然后穷举所有符合条件的b */
	{
		sum=0;t=b;
		while (t>=1)
		{
			i=t%10; //首先求个位 
			sum=sum+powm(i,a); //自幂数的定义，每个位数相加 
			t=t/10; //往前推进一个位数，继续求解个位，就变成先前的十位数了 
		}	
	    if (b==sum)
	    printf ("%i\n",sum);	
    }
    return 0;
}
```

## 函数-循环-区间内最大素数(sy4-5)
	//輸入正整數n，m，求不大於n的m個互不相同的最大的素數。
```c
#include<stdio.h>
//不大于n的m个互不相同的最大素数 
int main()
{
	int n,m,i,j,count=0;
	scanf("%d,%d",&n,&m);
	for(i=n;count<m;i--) //i是正在穷举的被除数 //count作为终结循环体的条件 
	{
		for(j=2;j<i;j++)//j是正在穷举的除数 
		{
			if(i%j==0) break;
	    } 
	    //下面这个if和内层循环并列 
			if(i==j) //找到质数 //这里是找的不能再找了，j都递增到i了，都仍然没有i能够整除j 
			{
			count++; //计数器 
			printf("%d\n",i);		
			}
		
	}
	return 0;
}
```

```c
#include <stdio.h>  //DZP的思路 
int main ()
{
	int n,m,a,b,i=0;/*a是除数，b是质数*/
	scanf ("%i,%i",&n,&m);
	
    for (b=n;i<m;b--)
    {
    	for (a=2;b%a!=0;a++)
    	{
    		if (a==b-1)
    		{
			printf ("%i\n",b);
    		i++;
    	    }
		}
	}
	return 0;
}
```

```c
int Is_prime(int num)     // 判断素数
{
	int i;
	if (num == 1)
	{
		return 0;
	}
	else
	{
		for (i = 2; i <= sqrt(num); i++)
		{
			if (num % i == 0)
			{
				return 0;
			}
		}
	}
	return 1;
}
```
## 函数-循环-求序列和(sy4-6)
	输入数字x和个数n,输出如图所示
> [[EXAMPLE_sy8#函数-求序列和(sy8-1)|函数-求序列和]]

![[Pasted image 20221222150127.png]]
```c
#include<stdio.h>
int sum(int c,int b)
{	
	int k,ten=1,s=0;
	for(k=b;k>=1;k--) 					//1x30000+2x3000+3x300+4x30+5x3
	{
		s=(k*c*ten)+s;					//迭加		 
		ten*=10;	
	}
	return s;
}
int main()
{
	int a,n,i,j;
	scanf("%d,%d",&a,&n);
	printf("%d",a);		//先打那个数字 
	for(i=2;i<=n;i++)
	{	
		printf("+");  
		for(j=1;j<=i;j++)  //for语句三个地方就中间那个地方，得用两个变量 
		{
			printf("%d",a);
		}  
	}
	printf("=");
	printf("%d",sum(a,n));
	return 0;
 } 
```

## 函数-循环-哥德巴赫猜想(sy4-7)
	//任何一個大於或等於6的偶數均可表示為2個素數之和。 
	/如，6 = 3 + 3，8 = 3 + 5，10 = 3 + 7，10 = 5 + 5,....20 = 3 + 17等。
	// 程式設計將6~30之間的偶數都表示為2個素數之和。
```c
#include <stdio.h> //DZP 
int main ()
{
	int a,x,y,b=0,c=0,m=2,n=2;/*a=b+c形式*/
	for (a=6;a<=30;a=a+2)                                     
	 /******特别注意*****/
	{
		b=0;c=0;/*归零*/
		for (x=2;x<a;x++)/*找质数x*/
		{
			for (m=2;x%m!=0&&x>m;m++)/*m为除数*/
			{
				if (m==x-1)
				{
					b=x;c=x;
					if (a==b+c)/*情况一：b==c*/
					{
						printf ("%i=%i+%i\n",a,b,c);
					}
					
					else /*情况二：b！=c*/ 
					{
						for (c=x+1;c<a;c++)/*找另一个质数,直接赋给c*/
						{
							for (n=2;c%n!=0;n++)/*n为除数*/
							{
								if (n==c-1&&a==b+c)
								printf ("%i=%i+%i\n",a,b,c);
							}
						}
					}
				}
			}
		}
	}
	return 0;
}
```

## 函数-循环-猴子吃桃(sy4-8)
	//一隻猴子第一天摘了一堆桃子，每天它都要吃掉一半
	//之後還要多吃一個，如此吃法，到第n天一早起來時，它發現只剩下一個桃子了
	//從鍵盤上輸入n，求它第一天摘的桃子總數。
```c
#include<stdio.h>  //猴子吃桃
int main()
{	
	int n,sum=1,i;
	scanf("%d",&n);
	for(i=1;i<n;i++)
	{
		sum=(sum+1)*2;
	}
	printf("Totals=%d",sum);
	return 0;
 } 
```

## 函数-循环-勾股数(sy4-9)
	//輸入正整數c，求從c開始的前4個勾股數
	//若數c的平方正好與另兩個數a、b的平方和相等，則稱c為一個勾股數。
```c
#include<stdio.h> //勾股数问题 

int main()
{
	int a,b,c,count=1;
	scanf("%d",&c);
	for( ;count<=4;c++)
	{
		for(a=1;a<c;a++)
		{
			for(b=1;b<c;b++)
	 		{
				if(a*a+b*b==c*c)
				{
					printf("NO%d:%d\n",count,c);
					count++;
					a=c;b=c;
				}
			}
		}	
	}	
	return 0;
}
```

## 函数-循环-区间闰年个数(sy4-10)
	//輸入一個年份區間，例如[1900，2015]，求該區間內的閏年的個數
	//閏年的判斷：能被4整除但不能被100整除，或者能被400整除
```c
#include<stdio.h> 		//闰年求解问题 求闰年个数 
int judgeyear(int year)
{
	int flag=0;  //初始为0 //平年返回0 
	if((year%4==0&&year%100!=0)||year%400==0)//闰年返回1
	{
		flag=1;
	}
	return flag;
}

int main()
{
	int i,a,b,count=0;
	scanf("[%d,%d]",&a,&b);
	for(i=a;i<=b;i++)
	{
		if(judgeyear(i)==1)
		{
			count++;
		}
	}
	printf("years=%d",count);
	return 0;
}
```

## 函数-循环-最大公约数最小公倍数(sy4-11)
	//輸入兩個正整數，求她們的最大公約數和最小公倍數。
```c
#include<stdio.h>  		 
int main()
	{	int a,b,i,j;
		scanf("%d,%d",&a,&b);
		for(i=a;i>=1;i--) 			//最大公约数穷举 
		{
			if(a%i==0&&b%i==0)
			{
				printf("gys=%d,",i);
				break;
			}
		}
		for(j=b; ;j++)				//最小公倍数穷举 
		{
			if(j%a==0&&j%b==0)
			{
				printf("gbs=%d",j);
				break;
			}
		}
		return 0;
		
	}
```

	这是一个求两个数的最大公约数的程序，使用了辗转相除法
	// 首先输入两个数a和b
	// 然后进入while，当b不为0时，执行循环体
	// 在循环体中，将a赋值给temp，将b赋值给a，将temp%a的值赋值给b
	// 这样就完成了一次辗转相除，直到b为0，此时a的值就是最大公约数
	// 最后输出a即可
```c
//宇宙最优解
int main()
{
	int a,b,c;
	scanf("%d %d",&a,&b);
	while(b!=0)
	{
		temp=a; 
		a=b;
		b=temp%a; //此时b更新为原先的a求余b,进入下一个循环,而a更新为原先的b
		
	}
	printf("%d",a);//这里求出了最大公约数 
	
	return 0;
}


int gcd(int a, int b) //great common divisor递归
{
	if (b == 0) return a; 
	//b其实就是上次传入的a%b的结果,也就是余数,余数为0时说明找到了最大公约数
	else return gcd(b, a % b);
}

  
////////////////////////////////
```
	兩個數的最大公約數=其中較小的數和兩數的差的最大公約數
	輾轉相除法也叫歐幾里德演算法，是最古老的演算法之一
## 函数-循环-展开式求和(sy4-12)
```c
#include<stdio.h>
#include<math.h>  			
//这题的函数返回值类型要时刻注意，变量的数据类型也要时刻注意
//这样在运算过程中才会精确，要改掉顺手就定义int变量 
 
double POW(double n,int j)
{
	double pow=1;
	int i;
	for(i=1;i<=j;i++) 
		pow=pow*n; 		//幂函数 	不调用数学库 
	return pow;
}
double FACT(int j)
{
	double fact=1,i;
	for(i=1;i<=j;i++)  	//阶乘 		不调用数学库 
		fact=fact*i;
	return fact;
}
int main()
{
	int i=1;
	double s=0,x,y=1; 
	scanf("%lf",&x);
	while(fabs(y)>=0.00001)
	{
		y=POW(x,i)/FACT(i);

		s+=y;
		i++;
	}
	printf("s=%.2lf",s);

	return 0;
}
```

## 函数-循环-求序列和(sy4-13)
	//每一项的分子是前一项分子与分母的和
	//每一项的分母是前一项分子 
```c
#include<stdio.h>
int main()		//序列和 
{
	int n,i;
	double s=0,temp1,temp2,x=2,y=1;
	scanf("%d",&n); 
	for(i=1;i<n;i++)
	{
		temp1=x+y;							//每一项的分子是前一项分子与分母的和 
		temp2=x;							//每一项的分母是前一项分子 
		s+=temp1/temp2;						//新一项大的值 
		x=temp1;
		y=temp2;
	
	}
	printf("s.2=%lf",s+2);
	return 0;
	
 } 
```

## 函数-循环-完备数(sy4-14)
	//輸入n，求[1，n]之間所有完數（一個數等於它的所有因數之和，這個數就稱為完數)
	//例如28的因數1+2+4+7+14=28，則28即為一個完數
```c
#include<stdio.h>
int main()
{
	int i,j,temp=0,sum=0,n;
	scanf("%d",&n);
	for(i=2;i<=n;i++) //被除数 
	{
		for(j=1;j<i;j++)		//除数 
		{
			if(i%j==0) 			//因子 
			{
				temp=j;
				sum+=temp;
			}	
		}	
		if(sum==i)
		{
			printf("%d\n",i);
		}
		sum=0;
	 } 
	 return 0;
 } 
```

## 函数-循环-某日是當年第幾天？(sy4-15) 
	\\（迴圈+switch實現）
```c
#include<stdio.h>
int main()
{
	int year,month,day,days,i,d;
	scanf("%d%d%d",&year,&month,&day);
	days=0;
 	for(i=1;i<month;i++)
	{
		switch(i) //不是month而是i 这是一个遍历 
		{	
		case 1:case 3:case 5:case 7:case 8:case 10:case 12:d=31;break;
		case 4:case 6:case 9:case 11: d=30;break;
		case 2:
		if(year%4==0 && year%100!=0 || year%400==0)
		d=29;
		else
		d=28;
		}
	days+=d;
	}
	printf("%d\n",days+day);
 	return 0;
}

```




## 函数-电阻之和(sy5-1)
	// 1/R總=1/R1+1/R2。//不能用这个直接算的
```c
#include<stdio.h>
int main()
{
	double R1,R2,Rsum;
	scanf("%lf %lf",&R1,&R2);
	Rsum=(R1*R2)/(R1+R2);
	printf("%.2lf",Rsum);
	return 0;	
}
```

## 函数-判断名次(sy5-7)
	//甲說：「丙得第一，我第三名」;
	//乙說：“我第一名，丁第四名”
	//丙說：“丁第二名，我第三名”
	//丁沒說話。
	//當最後結果公佈時，甲乙丙都只說對了一半，請給出正確的4人名次（沒有並列名次）。
```c
#include<stdio.h>
int main()  
{
	int a,b,c,d;
	printf("abcd\n");
	
	for(a=1;a<=4;a++)
	{
		for(b=1;b<=4;b++)
		{
			for(c=1;c<=4;c++)
			{
				for(d=1;d<=4;d++)
				{
					if(a!=b&&a!=d&&a!=c&&b!=c&&b!=d&&c!=d)
					{
						if((c==1) && !(a==3)|| !(c==1) && (a==3))
						{
							if((b==1) && !(d==4)|| !(b==1) && (d==4)) 
							{
								if((d==2) && !(c==3)|| !(d==2) && (c==3))
								{
									printf("%d%d%d%d",a,b,c,d);
								}
							}	
						}
						
					}
					
				}
			}
		}
	}
	
	return 0;
}
```




# ARRAY
## 数组-寻找最大差值(sy5-2)
```c
#include<stdio.h>  //寻找数列中最大值与最小值以及它们之间的差值 
void main() //预留主函数返回值类型 
{
	int n,i,j; double NUM,MAX=0,MIN=0,DIF; 
	scanf("%d\n",&n);
	if(n>=2&&n<=50)				//这个是题干条件 
	{
		double a[50]={0};		//我预留了50个位置 
		
		for(i=0;i<n;i++)
		{
			scanf("%lf",&NUM);	//n几次就输入几个 
			a[i]=NUM; 
			
			if(a[i]>MAX) 		//寻找MAX 
			{
				MAX=a[i];
			}
			NUM=0;	
		}
		
		MIN=MAX;				
		//先让MIN有个值 ，MIN如果初始值为0那就没人比他小了， 
		
		for(j=i-1;j>=0;j--)
		{
			if(a[j]<MIN)		
			//寻找MIN//这个思路有点逆，MIN一开始得先有个值 才能比较和替换下去 
			{
				MIN=a[j];
			}
		 }
    
		DIF=MAX-MIN; //运算即可 
		printf("%.2lf",DIF);
    }
	else return;			//n超出范围 
	
}
```

## 数组-封闭曲线(sy5-3)
	//設滿足條件的n條封閉曲線可將平面分成An個區域。
	//則 當n=1時，A1=2. 當n=2時，A2=4 當n=3時， A3=8
	//An = A（n-1） + 2*（n-1） 。
	輸入：一行一個數，表示封閉曲線數n
	輸入：一行一個數，表示這n條封閉曲線將一個平面分割成區域數目。
```c
#include<stdio.h> //封闭曲线分割平面 
int main()
{
	int i=0,j,n;				
	int A[128]={2};       //为防止内存溢出，初始为128 
	scanf("%d",&n);
	for(j=1;j<=n;j++)		//这里是An的下标，实际对应数组A(n-1) 这个i实际就是n-1 
	{
		A[j]=A[i]+2*(i);  //for嵌套可行但没必要 
		i++;
	 } 
	printf("%d",A[i]);		//这样一想，这里填i应该没关系 (一开始填的是j-1) 
	return 0;
	
}
```
## 数组-更列问题(sy5-4)
> [[EXAMPLE_sy11#递归-更列问题|递归-更列问题]]

```c
#include<stdio.h>
int main()
{
	int f[128]={0,1};
	int n,i;
	scanf("%d",&n);
									
	for(i=2;i<=n;i++)				
	//数组介于第一项数为0，故有些许差异f[n]=n*(f[n-2]+f[n-1]) 
	//就是这里的f[n]代表f(n-1) 
	{								
		f[i]=(i)*(f[i-2]+f[i-1]);  
		//f(5)=4(f(3)+f(4)) 表达式就是f(n)=(n-1)(f(n-2)+f(n-1)) 
		//外面那个系数其实就是项数减一， 
	}
	printf("%d",f[n-1]);
	return 0;
	
}
```

## 数组-简单字符加密(sy5-5)
	//輸入一行字元（不多於60個字元，以回車結束），將其加密。
	//1）將小寫字母轉換為相應的大寫字母;
	//2）將大寫字母轉換為相應的小寫字母; 
	//3）非字母字元不變; 
	//4）字母順序後延3個字元，且x->a，y->b，z->c; 
	//5）數位字元順序後延5個符號。
```c
#include<stdio.h>
int CHAR(int i) 
{
	if (i>='A'&&i<='W')	
	i+=('d'-'A'); 				//懒得用数字表示大小写之间的ASCII差值 
	else if (i>='a'&&i<='w')		
	i-=('a'-'D');
	else if (i>='X'&&i<='Z')
    i+=('a'-'X');
    else if (i>='x'&&i<='z')
    i-=('x'-'A');
    else if (i>='0'&&i<='4')
	i+=5;
    else if (i>='5'&&i<='9')
    i-=5;
	return i;
 } 
int main()
{
	int A[61]={0};int temp,i,j,count=0;			//60个位置 
	
	for(i=0;i<60; )
	{
		temp=getchar();
		A[i]=CHAR(temp); 	//缓存一个字符的ASCII 
		count++;
		if(temp=='\n') break; //回车结束循环(这个循环是为了输入并存入数组)
		else i++;
	}
	
	for(j=0;j<count;j++)
	{
		printf("%c",A[j]);
	}
	return 0;
}
```

## 数组-埃及分数(sy5-6)
	//設某個真分數的分子為a，分母為b;
	1把b除以a的商部分加1后的值作為埃及分數的某一個分母c;
	2將a乘以c再減去b，作為新的a;
	3將b乘以c，得到新的b;
	4如果a大於1且能整除b，則最後一個分母為b/a; 演算法結束;
	或者，如果a等於1，則最後一個分母為b; 演算法結束;
```c
#include<stdio.h>
int main()
{
	int a,b,c;
	scanf("%d %d",&a,&b); if(a>b) return;
	printf("%d/%d=",a,b);
	
	do		//难得用一次dowhile 
	{
		c=b/a+1;	
		a=a*c-b;
		b=b*c;
		printf("1/%d+",c);
	}
	while((a>1&&b%a!=0)&&a!=1); 
	//为什么不能写成while(a<1) //因为a绝对不可能是负数或者0 
	if(a>1&&b%a==0) printf("1/%d",b/a);
	if(a==1) printf("1/%d",b);
}
```


## 数组-阵列顺序比大小(sy6-1)
	//輸入一個整數n（1<n<=100），再輸入n個整數，將它們存入數位中。 
	//1)輸入一個整數x，然後再陣列中查找x，如果找到，輸入相應的最大下標，否則輸出-1。 
	//2)輸出陣列中大於x的數的個數
	//3)輸出陣列中小於x的數的個數。

	//第一行，表示數的個數為n,第二行，n個整數,第三行,一個數x
	//第一行，等於指定值的下標,第二行，大於指定值的數的個數,第三行，小於指定值的數的個數
```c
#include<stdio.h>
int main()
{
	int n,x,i,j,temp ;int A[128]={0};
	scanf("%d",&n);
	for(i=0;i<n;i++)
	{
		scanf("%d",&temp); //这里不用temp变量也可以，我那个时候乃年少无知 
		A[i]=temp;
	}
	scanf("%d",&x);
	
	int BIGcount=0,SMALLcount=0,ALRIGHT=0;
	for(j=0;j<n;j++)
	{
		if(A[j]==x) 
		{
		printf("%d\n",j);ALRIGHT++;
		}
		if(A[j]>x)  BIGcount++;
		if(A[j]<x)  SMALLcount++;
		
	}
	if (ALRIGHT==0&&(BIGcount!=0||SMALLcount!=0)) printf("%d\n",-1);
	printf("%d\n%d",BIGcount,SMALLcount);
	
	return 0;
 } 
```

## 数组-阵列指定位置删除和插入(sy6-2)
	//已知一初始化的陣列a[11]={1，2，3，4，5，6，7，8，9，0}。
	//輸入x1和n1，將數x1插入在陣列的n1（下標）處，原陣列從該處之後的元素順序後移，
	//如果插入的位置不合法，則插在數位的最後一個元素之後; 
	//執行完上述操作之後，輸入n2，將陣列下標為n2處的元素刪除，該處之後的元素順序前移
	//如果刪除的位置不合法，則刪除陣列的第一個元素（下標為0 ）

	//輸入：第一行，2個數，分別代表x1和n1    第二行，一個數，表示n2
	//輸出三行，每個數佔5列
	第一行，10個數，表示原陣列元素
	第二行，11個數，表示進行插入操作之後的陣列
	第三行，10個數，代表進行插入之後，又進行了刪除操作之後的陣列
```c
#include<stdio.h>
int main()
{
	int x1,n1,n2;int i,temp1,temp2;
	int a[11]={1,2,3,4,5,6,7,8,9,0};
	scanf("%d %d %d",&x1,&n1,&n2);
	for(i=0;i<10;i++) printf("%5d",a[i]);
	printf("\n");
	
	if(n1>=0&&n1<=10)
	{
		temp1=a[n1];
		temp2=a[n1+1];
		for(i=n1;i<=11;i++)  //正向temp存储两次，防止覆盖；反向则不用，0不怕覆盖 直接覆盖 
		{
			a[i+1]=temp1;
			temp1=temp2;
			temp2=a[i+2];
		}
		a[n1]=x1;
		for(i=0;i<=10;i++) printf("%5d",a[i]);
		printf("\n");
	}
	else
	{
		a[10]=x1;
		for(i=0;i<=10;i++) printf("%5d",a[i]);
		printf("\n");
	}
	
	
	if(n2>=0&&n2<=10)
	{
		a[n2]=0;
		for(i=n2;i<11;i++)	a[i]=a[i+1]; 
		for(i=0;i<10;i++) 	printf("%5d",a[i]);
	}

	else
	{
		a[0]=0;
		for(i=0;i<11;i++)	a[i]=a[i+1]; 
		for(i=0;i<10;i++) 	printf("%5d",a[i]);
	}
	
	return 0;
	
}
```

## 数组-阵列元素修改(sy6-3)
	//已知整型陣列a存儲的是某班20位同學的某門課成績。
	//初始化如下：
	a[20]={81,55,102,84,204,105,56,85,58,202,101,83,104,103,82,201,59,203,57,205}
	//1）0~59為不及格成績，這些同學需要進行補考; 
	//2）60~100為正常; 
	//3）大於100小於等於200的，例如101表示病假，102表示事假， 103表示休學...... 需進行緩考; 
	//4）大於200的，例如201表示曠考，202表示作弊...... ，這些同學需要重修。
	//現在，準備原陣列來存放這些同學此門課的狀態，
	//1）補考-1; 2）正常，以原成績表示; 3）緩考-2; 4）重休-3。
```c
#include<stdio.h>
int main()
{
	int i,j;
	int a[4][5]={{81,55,102,84,204},{105,56,85,58,202},{101,83,104,103,82},{201,59,203,57,205}};
	for(i=0;i<4;i++)  //二维数组 
	{
		for(j=0;j<5;j++)
		{
			if(a[i][j]>=0&&a[i][j]<=59) printf("   -1");
			if(a[i][j]>=60&&a[i][j]<=100) printf("%5d",a[i][j]);
			if(a[i][j]>100&&a[i][j]<=200) printf("   -2");
			if(a[i][j]>200) printf("   -3");
		}
		printf("\n");
	}
	return 0;
}
```

## 数组-单字符个数统计(sy6-4)
	//輸入一行字元（字元個數小於80）
	//這行字元包括小寫字母，大寫字母，數位，空格等其他可列印符號。 
	//請統計各字母的個數，小寫字母和大寫字母統計於小寫字母上
	//例如輸入字串為：aA123B，則字母a出現的次數為2，字母b出現的次數為 1
	//如果某字母小寫大寫都沒出現，則不用進行輸出
```c
#include<stdio.h>
int main()
{
	int i,count=0;
	char a[80]={0};
	for(i=0;i<80;i++)		
	//while((a[i]=getchar())!='\n') k++; a[k]='\0';
	//do{ a[i]=getchar();i++;) } while(a[i]!='\n');
	{
		//scanf("%c",&a[i]);
		a[i]=getchar(); //count++;          
		//这里的count可以来计算总字符个数 
		if(a[i]=='\n') 
		{              
			a[i]='\0';	//count--;		
			//这里是把最后那个存回车覆\0的那一个减去，那不是我们输入的字符 
			break;
		}
	}
	i=0;
	getchar(); 
	int j;
	for(j='A';j<='Z';j++) 
	{	
		for(i=0;i<80;i++)		//两个for循环嵌套互换内外 
		{	
			if((a[i]==j)||(a[i]==j+('a'-'A')) )
			{ 
				count++;
			}
			if(count!=0&&i==79) printf("%c is %d\n",j+32,count);
		}  
		count=0;
	
	}
	return 0;
}
```

## 数组-合数素数一左一右(sy6-5)
	//已知一整型陣列a，長度為n（1<n<=100），存放的是大於2的正整數
	//現欲將合數存放至陣列的左端，素數存放在陣列的右端
	//輸入陣列中實際元素的個數，以及各元素值。
	//輸出調整后的陣列，每個數佔5列。端，素數存放在陣列的右端
```c
#include <stdio.h>
#include <math.h>

int IsPrimer(int num)
{
    int i,flag=0;
    for(i=2;i<=num/2;i++) //寻找素数 1为素数(放置右边) 0为合数 
    {
        if(num%i==0)
        break;
    }

    if(i>num/2)
    flag=1;
    
    return flag;
}

int main()
{
    int a[100],n,i,j,temp;
    scanf("%d",&n);

    for(i=0;i<n;i++)
    scanf("%d",&a[i]);
    i=0,j=n-1;

    while(i<j)
	{
		for(;IsPrimer(a[i])==1&&j>0;j--)
		{
			if(IsPrimer(a[j])==0&&IsPrimer(a[i])==1)
			{
				temp=a[i],a[i]=a[j],a[j]=temp;
				i++;
				break;
			}
			else continue;
		}
		if(IsPrimer(a[i])==0) i++;
		/*
		if(n%2==0)
		{
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])!=0) 
			{
				temp=a[i],a[i]=a[(n-1)-i],a[(n-1)-i]=temp;i++;
			}
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])==0)
			{
				i++;continue;
			}
			if(IsPrimer(a[i])==1)
			{
				i++;continue;
			}
		}
		if(n%2!=0)
		{
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])!=0) 
			{
				temp=a[i],a[i]=a[(n-1)-i],a[(n-1)-i]=temp;i++;
			}
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])==0)
			{
				i++;continue;
			}
			if(IsPrimer(a[i])==1||i==n/2+1)
			{
				i++;continue;
			}
			
		}
		*/		
	}
    for(i=0;i<n;i++)
    printf("%5d",a[i]);

    return 0;
} 
```

## 数组-两阵列共同元素(sy6-6)
	//已定義兩個長度為10的陣列，其中一陣列中的元素值已初始化
	//另一陣列實際元素的個數及各元素值從鍵盤輸入，現需找出在兩個陣列中都存在的數。 
	//輸入陣列中實際元素的個數，以及各元素值。
	//輸出在兩個陣列中都出現的數（每個數佔5列寬度）。
```c

#include <stdio.h>
int main()
{   
    int n,i,j,s1[10]={10,21,34,12,15,8,17,20,23,30},s2[10];
    scanf("%d",&n);
    
    for(i=0;i<n;i++)
    scanf("%d",&s2[i]);
    for(j=0;j<10;j++)
	{
		for(i=0;i<n;i++)
		{
			if(s2[i]==s1[j]) //两重循环进行遍历 
			{	
				printf("%5d",s1[j]);
				s1[j]=0;    
				//为了防止后面s2数组又有一样的数字然后重复输出,我真聪明 
			}
			else continue;
		}
	}
    return 0;
}
```

## 数组-阵列循环右移(sy6-7)
	//從鍵盤輸入兩個整數k和n（1<k，n<20）
	//再輸入n個整數，使其前面各數順序向右移動 k個位置，即最後k個數變成最前面k個數。 
	//注意：只允許定義一個整型的一維陣列，且迴圈右移m次後陣列各個元素重新存放
```c
#include <stdio.h>
int main()
{
    int a[100],n,k,i,j;
    int tmp;
    scanf("%d",&k);
    scanf("%d",&n);
    
    for(i=0;i<n;i++)
    scanf("%d",&a[i]);

	
	for(j=0;j<k;j++)
	{
		for(i=n-1;i>0;i--)
		{	
			tmp=a[i]; 	//只给一个变量 则递减覆盖 
			a[i]=a[i-1]; 
			//注意a[i-1]这个地方的值并没有被覆盖，下次循环得到a[i]就是这里的a[i-1] 
			a[i+1]=tmp;
		}
 		a[i]=a[n]; 
	}

    for(i=0;i<n;i++)
    printf("%3d",a[i]);
    
    return 0;
}
```

## 数组-阵列逆序保存(sy6-8)
	//輸入一個整數n（1<n<=100），陣列元素由下式給出：
	a[i]=2*i+1  (0<=i<=n-1)
	//請將陣列逆序後儲存在原陣列中並輸出
```c
#include <stdio.h>
int main()
{
    int a[100],n,i,tmp;
    scanf("%d",&n);
    for(i=0;i<n;i++)
    	a[i]=2*i+1;
    for(i=0;i<n;i++)
    	printf("%3d",a[i]);	
	printf("\n");

	for(i=0;i<n/2;i++)  
	//这里循环条件限制到一半即可，要不然到后面又换回来了 那整个数组还是原样 
		tmp=a[i],a[i]=a[(n-1)-i],a[(n-1)-i]=tmp;
    	/*
		if(n%2==0)
		{
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])!=0) 
			{
				temp=a[i],a[i]=a[(n-1)-i],a[(n-1)-i]=temp;i++;
			}
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])==0)
			{
				i++;continue;
			}
			if(IsPrimer(a[i])==1)
			{
				i++;continue;
			}
		}
		if(n%2!=0)
		{
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])!=0) 
			{
				temp=a[i],a[i]=a[(n-1)-i],a[(n-1)-i]=temp;i++;
			}
			if(IsPrimer(a[i])==0 && IsPrimer(a[(n-1)-i])==0)
			{
				i++;continue;
			}
			if(IsPrimer(a[i])==1||i==n/2+1)
			{
				i++;continue;
			}
		}
		*/

    for(i=0;i<n;i++)
    printf("%3d",a[i]);

    return 0;
}
```






## 数组-最大数交换首元素(sy7-1)
	//輸入陣列（不超過20個元素），陣列的第一個最大數（有可能存在多個最大數）與陣列首元素交換。
	//一行共n+1個實數。 第一個是正整數n，表示陣列長度為n; 
	//之後是連續的n個實數，表示陣列的n個元素。
```c
#include<stdio.h>
int main()
{
	double a[20]={0};int n,i,k=0,temp;
 	scanf("%d",&n);
 	
 	for(i=0;i<n;i++)
 	{
 		scanf("%lf",&a[i]);
 	}
 	int max=a[0];
 	
 	for(i=0;i<n;i++)
 	{
 		if(a[i]>max) 
 		{
	 		max=a[i];
	 		k=i;
		}
	}
	a[k]=a[0],a[0]=max;	
	
	for(i=0;i<n;i++)
	{
		printf("%lf  ",a[i]);
	}
	
	return 0;
}
```

## 数组-统计数字字符个数(sy7-2)
	//輸入一行字元，以連續的二個“#”表示輸入結束。 統計這行字元中數位字元的個數
```c
#include<stdio.h>
int main()
{
	int i;char a[128]={0};int stringCOUNT,NUMcount=0,count=0;
	for(i=0;count!=2;i++)
	{
		a[i]=getchar();
		if(a[i]=='#') count++;
		else count=0;
	}
	
	if(count==2) a[i-1]='\0';  //这一步不知道要不要加上 
	
	stringCOUNT=sizeof(a)/sizeof(a[0]);
	for(i=0;i<stringCOUNT;i++)
	{
		if(a[i]<='9' && a[i]>='0') NUMcount++;
 	}
 	printf("sum=%d",NUMcount);
 	
 	return 0;
}
```

## 数组-求鞍点(sy7-3)
	//輸入正整數n（2<n<6）及n階方陣。
	//輸出鞍點的行列及值
	//在矩阵中，一个数在所在行中是最大值，在所在列中是最小值，则被称为鞍点
```c
#include<stdio.h> 

int main()
{
	int n,j,k,i,min,max,maxL=0,minH=0,count=0;
	scanf("%d",&n);
	int a[n][n];                                                                                                                                                                                        
	
	for(j=0;j<n;j++) //行数 
	{
		for(k=0;k<n;k++) //列数 
		{
			scanf("%d",&a[j][k]);			
			//找到最大数 
		}
	}
	
	for(j=0;j<n;j++) //行数 
	{
		max=a[j][k]; 
		for(k=0;k<n;k++) //列数 
		{
			if(a[j][k]>max) 
			{
				max=a[j][k];maxL=k;			//找到最大数那一列下的最小值 
			}
		}
		for(i=0,min=a[i][maxL];i<n;i++)
		{
			if(a[i][maxL]<min)
			{
				min=a[i][maxL];minH=i;		
				//直接判断 和前者一样 存储这个数的行，之前找最大值的时候就已经记录下了列 
			}	
		}
		if(min==max)
		{
			printf("a(%d,%d)=%d",minH,maxL,min); 	
			count++;								
			//计数器判断是否找到即可 
			break;
		}
		
	}
	if(count==0) printf("NO");
	
	return 0;
 }   
 ```
## 数组-删除二维数组元素并前移(sy7-4)
	//已知一個二維整型數位列已初始化
	//a[5][5]={{1,2,3,4,5}，{2,3,4,5,6}，{3,4,5,6,7}，{0,1,2,3,4}，{6,7,8,9,0}}
	//請刪除此數組中所有等於指定值的元素，刪除後，各元素依次前移
```c
#include <stdio.h>

int main()
{

    int i,j,count=0,num;
    int a[5][5]={{1,2,3,4,5},{2,3,4,5,6},{3,4,5,6,7},{0,1,2,3,4},{6,7,8,9,0}};
    scanf("%d",&num);
	for(i=0; i<5; i++)
		for(j=0; j<5; j++)
            if(a[i][j]!=num) //count要用上 
			{
				*(a[0]+count)=a[i][j]; 
				//不能写成*(a+count)=a[i][j] 
				//因为数组不能直接赋值给数组，就赋值左边不能出现数组名； 
	            count++;
            }           

	for(i=0; i<5; i++) //应该是修改完内存之后的普通遍历 
	{
        for(j=0;j<5;j++)
		{
			if(count!=0)
			{
				printf("%3d",a[i][j]);
				count--;
			}
		} 
		printf("\n");
    }
    return 0;
}
```

## 数组-小猴修仙(sy7-5)
	//小猴白天依據其體力向上爬高若干高度; 晚上緊抱玉柱休息，只是玉柱太滑，每晚下降若干高度。 
	//有時晚上下降得太厲害，可能會掉到地面上哦
	//第一行2個正整數，分別代表玉柱高度K和天數限制n，（0<K，n<1000）
	//第二行開始的連續n行，每行2個正整數，分別代表每天白天向上攀爬m和晚上向下滑落的高度p， 
	//（0<=m，p<=1000）。
	//能觸響鈴鐺，輸出“ YES”；不能觸響鈴鐺，輸出“ NO”。
```c
#include<stdio.h> //猴子修仙 蜗牛爬井 
int main()
{
	int K,n,i,sum=0;
	scanf("%d%d",&K,&n);
	int a[2*n];
	for(i=0;i<2*n;i++)
	{
		scanf("%d",(a+i));
	}
	for(i=0;i<2*n;i++)
	{
		if(i%2!=0) 
		{
			a[i]=-a[i];
			if((-a[i])>=sum)
			{
				sum=0;
				continue;
			}
		}
		sum+=a[i];
		if(sum>=K) 
		{
			printf("YES");
			break;
		}
		if((i==2*n-2)&&sum<K)
		{
			printf("NO");
		}
	}
	
	return 0;
 } 
```

## 数组-围圈报数(sy7-6)
	//新生軍訓開始了，1班n位同學給自己班級戰隊取名為“1level1”
	//n位同學圍成一圈，並順序編號為1~n。 1班準備了7面旗幟，分別寫上了1，l，e，v，e，l，1。 
	//他們從第一個人開始報數（從1到m報數）
	//凡報數為m的人恰好手上舉著一個寫著字母（數位）的旗幟並退出圈子。 
	//依次退出了7個人（1level1的長度為7）
	//按照退出的先後順序排成一排，旗幟上的字母依次正好組成了1level1。
	//你是班長，肯定知道班級有多少人，知道匯演口令是從1到m報數，怎麼去安排哪7人分別拿哪個旗幟？
	【輸入形式】一行兩數，用空格隔開，分別表示人數n和報數m
	【輸出形式】若干行，每行兩數，分別表示“第幾人”和“旗幟上的字元”
```c
#include<stdio.h> //回文绕圈报数 
int main()
{
	int n,m,i,j=0,count=0;
	scanf("%d%d",&n,&m);
	char a[n],c[]="1level1";
	for(i=0;i<n;i++)
	{
		a[i]='*';
	} 
	a[i]='\0';i=0;
	
	while(i<n&&j!=7)
	{
		count++; //计算报到哪个人了 
		if(a[i]!='*') 	count--; //跳过 
		if(count==m)
		{
			a[i]=c[j]; //直接字符赋值过去 
			j++;//预留下一个字符 
			count=0;
		}
		i++;
		if(i==n) i=0; 
	}
	
	for(i=0;i<n;i++)
	{
		
		if(a[i]!='*')
		{
			printf("%d %c\n",i+1,a[i]);
			j++;
		}
	}
	return 0; 
}
```

## 数组-构造回文(sy7-7)
	//給出一個陣列 A[1..n]，如果對於任意的 1<=i<=n，都有 A[i] = A[n-i+1]
	//那麼陣列 A就是回文陣列。 
	//給定 A陣列後，如果 A不是回文陣列，你要用最好的操作次數把 A 陣列變成回文陣列
	//每次操作你可以選定 A 陣列相鄰的兩個數
	//不妨假設被選定的兩個相鄰的數是 x 和 y 
	//那麼你可以用 x+y 帶替代原來的 x 和 y，即把 x 和 y 從 A 陣組刪掉，
	//用 x+y 的和替代 x 和 y 的位置，每次操作之後 A 陣列都會少一個數。 
	//計算最少需要多少次操作才能使得 A 陣列變成回文陣列？

	//1)首先，可以確定如果在原序列中a[i]已經等於a[n-i+1]的話，那麼這兩個數都不用動。
	//2)其次，在原序列中的a[i]如果不等於a[n-i+1]，
	//那麼這個回文陣列就不能成立，所以他們之中必有一個數要和他相鄰的合起來，那麼和誰合呢？
	//既然前面都已經是不用動的，那麼自然而然是和他後面的合（n-i+1的那個就跟他前面的合）
	//那麼問題又來了：難道i和n-i+1都要分別合嗎，未必。 
	//大家先看一個例子：1 2 4 6 1，（過程為：12461->1661）這個例子就是先把一個合起來。 
	//3)還有一個要注意的，兩個之中合哪一個？ 
	//顯而易見，就是小的那個先合，因為a[i]是正整數，所以後面不可能變小

	//第一行，一個整數 n，表示陣列有 n 個數構成。 1 <= n <= 1000000。
	//第二行，n個整數，第 i個整數是 A[i]， 1<=A[i] <= 10^9。
	//一個整數，表示最少的操作次數。 如果不能構成回文陣列，則輸出-1。
```c
#include <stdio.h>
#define Maxn 10001
int main()
{    
	int flag=0;
    int i,n,a[Maxn],left,right,ans=0;
    scanf("%d",&n);
    for(i=1;i<=n;i++) scanf("%d",&a[i]);
    left=1; right=n;        
    while(left<right)
    {
        if(a[left] == a[right])
        {
            if(left+1==right || left+2==right)//成功得到回文数组 
            {    
            	flag++; 
            }
            left++; right--;//left和right指针的移动 
        }
        else if(a[left] > a[right]) //右部做操作 
        {
            a[right-1] += a[right]; 
            right--;
            ans++;
        }
        else//左部做操作 
        {
            a[left+1] += a[left];
			left++; 
			ans++;
        }
    }
    if(flag==0) ans=-1;
    printf("%d\n",ans);
    return 0;
}

```

## 数组-判断回文数(sy7-8)
> [[EXAMPLE_sy9#指针-判斷回文字串|指针-判断回文字符串]]
```c
#include <stdio.h>//判断回文 
#include <math.h>
int is_huiwen(long x)
{
    int a[100],i=0,p;//此题请关注注意a[]数组的使用，x的数位将保留在数组中
    while(x)
	{
		a[i]=x%10;//将数位保存在数组，请注意下标的变化 
        x/=10;
        i++;
    }
    for(p=0;p<(i-1)/2;p++)
    {
    	if(a[p]!=a[i-p-1])
    	{	
    		return 0;//判断数组元素是否相等 
		}
	} 
	return 1;
} 
int main()
{
    long x;
    scanf("%ld",&x);
    if(is_huiwen(x))
        printf("YES");
    else
        printf("NO");
    return 0;
}

```




# FUNCTION
## 函数-求序列和(sy8-1)
> [[EXAMPLE_sy4#函数-循环-求序列和|函数-循环-求序列和]]

	//從鍵盤輸入兩個整型變數a和n的值，求a+aa+aaa+aa ...... a（n個a）之和。
	//例如，若輸入2和3，則輸出序列和為246（ 2+22+222）。
	//要求定義函數double fun（int a，int n）計算並返回a... a（n個a）之值。
```c
#include<stdio.h>
double fun(int a,int n)
{
	double sum=0;int i,t=0;
	for(i=0;i<n;i++)			
	{
		t=t*10+a;
		sum=sum+t;
	}
	return sum;
}
int main()
{
	int a,n;
	scanf("%d %d",&a,&n);
	printf("s=%.0lf",fun(a,n));
	
	return 0;
}
```

## 函数-单纯求和(sy8-2)
	//編寫一個求和函數int sum（int start，int count）返回從start開始前count個數的和值
	//例如：sum（2，4）返回2+3+4+5的和值】。 
	//編寫main函數通過調用sum函數求s=（1+2）+（2+3+4）+（3+4+5+6）+...; 
	//前n項的和值（n從鍵盤輸入）
```c
#include<stdio.h>

int sum(int start,int count)
{
	int SUM=0,i;
	for(i=0;i<count;i++) //注意次数啥的 很烦 
	{	
		SUM=SUM+start;
		start=start+1;
	
	}
	
	return SUM;
}
int main()
{
	int i,n,tmp=0;
	scanf("%d",&n);
	for(i=1;i<=n;i++)
	{
		tmp=tmp+sum(i,i+1);  //这个叠合的思想很常见 
	}
	printf("sum=%d",tmp);
	
	return 0; 
}
```

## 函数-斐波那契数列(sy8-3)
>[[EXAMPLE_sy11#递归-斐波那契数列|递归-斐波那契数列]]

	//編寫求Fibonacci數列第n項的值的函數int fib（int n）
	//並且在main函數中調用fib函數求Fibonacci數列中從第m項到第n項的和值
	//其中m、n從鍵盤輸入
```c
#include<stdio.h>//斐波纳契数列以如下被以递推的方法定义：
//F(1)=1，F(2)=1,                                                                                                                      （n>=3，n∈N*）
int fib(int n)
{                               
	int a[n];int sum=0,i;
	a[0]=1,a[1]=1;
	for(i=2;i<n;i++)
	{
		a[i]=a[i-1]+a[i-2]; //从第三项开始算 
	}
	for(i=0;i<n;i++)
	{
		sum=sum+a[i];
	}
	
	return sum;
}

int main()
{
	int m,n,sum;
	scanf("%d,%d",&m,&n);
	sum=fib(n)-fib(m-1); 
	//如果是fib(n)-fib(m)那就不是从m项开始的了，因为m项也没减去了 
	printf("sum=%d",sum);
	
	return 0;
}
```

## 函数-求区间内素数和(sy8-4)

```c
#include<stdio.h>
int PRIME(int m,int n,int *p)
{
	int i,j,count=0,sum=0;
	for(i=m;i<=n;i++)
	{
		for(j=2;j<i;j++)
		{
			if(i%j==0)
			{
				break;
			}
		}	 
		if(j==i) //这是穷举到不能再穷举的情况 
		{
			count++;sum+=i; 
		}
	}
	*p=count; //直接在函数内部操作内存 
	//利用指针达到返回第二个值给主函数的效果 
	
	return sum;
}

int main()
{
	int *p,m,n,count,sum;
	scanf("%d,%d",&m,&n);
	p=&count; //指针p存入count变量的地址 
	sum=PRIME(m,n,p);
	printf("count=%d,sum=%d",count,sum);
	
	return 0;
}
```

## 函数-哥德巴赫猜想(sy8-5)
	//驗證歌德巴赫猜想：任何大於2的偶數均可表示為兩個素數之和。 
	//例如：4=2+2，6=3+3，8=3+5，
	//試驗證[n，n+10]之間的偶數，每個偶數只需找到一組和式即可，其中n從鍵盤輸入
```c
#include<stdio.h>
int prime(int n)
{
	int flag=0,j; //为素数返回1 为合数返回0 
	
	for(j=2;j<n;j++)
	{
		if(n%j==0)	break;
	}
	if(j==n) 	flag=1; //这个得写在循环外面 
	return flag;
}

int main()
{
	int i,j,k,n,count=0;
	scanf("%d",&n); 
	for(i=n;i<=n+10;i+=2)
	{
		if(i%2!=0) i++;  //在这个范围内的所有偶数 
		//第一个数也就是左端点如果不是偶数，就让他变成奇数 
		for(j=2;j<i;j++)
		{
			for(k=2;k<i;k++)
			{
				if(prime(j)==1&&prime(k)==1) //素数时 
				{
					if(j+k==i&&count==0) 
					//这里让每个数的哥德巴赫组合只打出来一次 
					{
						printf("%d=%d+%d\n",i,j,k);	
						count++;  
						//相当于标记了count非零，此时就不会再打已经打出组合的那个数 
						break;	
						//这个break时break到j循环
						//我们重新赋零标记应该和j循环并列而非在里面 
					}
				}
				else continue;
			}	
		}
		count=0; //现在j和k穷举完了才重新归零count
		//此时下一步就是i递增，然后继续穷举j和k
		//此时归零的count就可以让穷举好的组合打出来一次 
	}
	
	return 0; 
}
```

## 函数-统计指定数字的个数(sy8-6)
	//輸入一個整數。
	//輸出其中2的個數
```c
#include<stdio.h>
int COUNT(int n)
{
	int t,count=0;
	while(n!=0)
	{
		t=n%10; //求余求的是最后一位上的数字 
		if(t==2) count++; //找到位上数字为2时 计数加一 
		n=n/10; //这是往前推一位 
	}
	
	return count;
}


int main()
{
	int n;
	scanf("%d",&n);
	printf("count=%d",COUNT(n));
	
	return 0;
 } 
```


# POINTER
## 指针-十进制转二进制
> 双精度小数进行二进制转换以及指针函数(返回指针的函数)的初尝试
> [[EXAMPLE_sy11|递归]]
```c
#include<stdio.h>
char *dec2bin(double x,int n)//保留n位二进制小数 
{
	int i,j,zs=(int)x;//这里先把小数x换成整型赋值给zs，但此时x仍然是小数 
	double xs=x-zs;//这里存入小数//之后的进制转换，是需要将小数和整数分开求余的 
	char t;
	static char result[100];//静态的字符数组 
	//静态变量默认的每一个数组元素都是0或者0.0或者\0 
	
	for(i=0;zs>0;i++)//对传入的整数进行求余 
	{
		result[i]='0'+zs%2; //存入字符1 or 0 
		zs=zs/2;			//但是注意这里的存入把位数小的放在数组靠前了 
	 } 
	for(j=0;j<i/2;j++) //余数反转//就是字符数组里是11001 变成10011 
	{					//我觉得可以进行数组倒序遍历打印，不用反转一次 
		t=result[j];
		result[j]=result[i-j-1];
		result[i-j-1]=t;  //这里就是一个基础的元素互换 
	 } 
	
	if(xs>0) ////////乘2取整法 //小数部分不为0
	{
		result[i]='.'; //此时的i承接整数倍部分存入数组的那一位
		for(j=1;xs>0 && j<=n;j++)//n是保留部分的位数 
		{
			xs=xs*2;//最先得到的就是小数部分的高位，不需要反转 
			result[i+j]=(int)xs+'0';//此时i仍然作为小数部分的首位下标即可,i不变j边 
			xs=xs-(int)xs; 
		 } 
		result[i+j]='\0';//这一步可以省略，因为静态数组里全是字符0了已经 
	 } 
	 return result;
 } 
 
 void main()
 {
 	double m; 
 	scanf("%lf",&m);
 	printf("%s\n",dec2bin(m,6));
 }
 
```
## 指针-连续数字字符转换为整型(sy9-01)
	//寻找字符串里连续的数字字符并转换成整数类型并存入一个整型数组 
```c
#include<stdio.h>//寻找字符串里连续的数字字符并转换成整数类型并存入一个整型数组 

int getdig(char *p,int *q)

{     
	int i=0;
	for( ;*p!='\0';p++)
	if(*p>='0'&& *p<='9')
	{     
		*q=*p -'0';       
		//这里的点就是字符本身就是整数 通过减去'0'可以直接变成对应的整数 
		p++;
		while(*p!='\0'&&(*p>='0'&& *p<='9'))
		{
			*q=*q*10+(*(p++) -'0');  //我是真的牛 运算的同时也做到自增的效果; 
		}
		i++;//一串连续的数字字符之后记一次 
        q++;
    }
	return i;               
}

int main()

{   
	char str[100],*p=str;
    int a[100],*q=a,count=0;
	gets(p); //存到p里 
	count=getdig(p,q);             
  	for(q=a;q<a+count; q++) //这里和平时不太一样 是指针作为循环变量 
	{
		printf("%4d",*q); //其实就是把整型数组a打出来而已 
	}
	   
	return 0;
}	   
```

## 指针-整数排序(sy9-02)
	//从键盘输入一个正整数n（n的值不超过10），然后输入n个互不相同的整数
	//通过调用一个排序函数void sort(int *x,int n)对这n个整数按从小到大的顺序排序
	//然后依次输出排序后的n个数。
```c
#include<stdio.h>
#define N 10

void sort(int *x,int n)

{    
	int i,j,k,t;
	for( i=0;i<n-1;i++)
	{        
		k=i;      
		for( j=i;j<n;j++)    //一开始我写的是1，后面改成i就对了;  j从i开始穷举即可; 
		{
			if( x[k]>x[j])
			{
				k=j;
			}
			t=x[i],x[i]=x[k],x[k]=t;
		}
	
	}
}

void main()
{      
	int a[N],n,*p;
	scanf("n=%d",&n);              
	for(p=a;p<a+n;p++)        
	{
		scanf("%d",p);
	} 
    sort(a,n);             
	for(p=a;p<a+n;p++)
	{
		printf("%3d",*p);
	}  
}

```

## 指针-删除字符数组中指定字符(sy9-1)
```c
#include<stdio.h>
void chardel(char *p,char c)
{
	char* t=p; 
	while(*p!='\0')
	//删除字符可以用一种覆盖原先数组的思想，用新变量k来一个一个覆盖 
	//另一个思路就是把非删除字符的字符放到新定义的新数组 
	{
		if(*p==c)
		{
			while((*(p+1)!='\0'))
			{
				*p=*(p+1);
				p++;
			}
			*p='\0',p=t;
			continue;
		}
		p++;
	}	
}
int main()
{
	char a[128];char *p=a;char ch;int i=0;
	a[i]=getchar();//写gets(p)也可以 
	while(a[i]!='\n')
	{
		i++;
		a[i]=getchar();
		
	}a[i]='\0';
	scanf("%c",&ch);
	chardel(p,ch); 
	printf("After delete:\n");
	for(p=a;*p!='\0';p++) //printf("%s",a); 
	{
		printf("%c",*p);
	}
	return 0;
 } 
```

## 指针-寻找数组元素(sy9-2)
	//数组元素值查找问题：输入一个整数，在数组中查找是否存在该数
	//若存在则显示其所在的数组下标位置，否则显示NOEXIST
	//查找使用search函数实现, 试完成空缺处的程序代码
```c
#include <stdio.h>
#define N 10
int search(int *p,int x,int n)
{   
   while(*(p+n)!='\0')
   {
   		n++;
		if(*(p+n)==x)
		{
			break;
		}
		
   }
   if(n==10) n=-1;
   return n;
}
int main()
{    int indx,m,a[N]={10,20,35,40,43,44,45,50,51,60};
     scanf("%d",&m);
	 indx=0;
     indx=search(a,m,indx); 	
     if(indx>=0)
        printf("index=%d\n",indx);
     else
        printf("NOEXIST\n");
     return 0;
}
```

## 指针-数组循环移位(头尾)(sy9-3)
	//输入移动的位数
	//数列进行循环移位
```c
#include "stdio.h" 
#define N 10
void cycle(int *p,int n,int count)
{   
	int i;
	while(count<n) //count传进来时为0 
    {
    	*(p+N)=*p;
    	for(i=0;i<N;i++)//N是10 
	    {//////////////////////////
	    
	    	*(p+i)=*(p+i+1);
	    //printf("\ni=%d,p[0]=%d,p[N]=%d\n",i,*p,*(p+N));
		}//////////////////////////
		count++;
	}
} 
int main()
{   
	int i,n,a[N]={1,2,3,4,5,6,7,8,9,10};
    scanf("%d",&n);
    ////////////////////////////////////
    printf("Before:\n");
    for(i=0;i<N;i++)
        printf("%4d",a[i]);
    ////////////////////////////////////
    i=0;
    cycle(a,n,i);
    ////////////////////////////////////
    printf("\nAfter:\n");
	for(i=0;i<N;i++)
        printf("%4d",a[i]);
    printf("\n");
    
    return 0;
}
```

## 指针-判斷回文字串(sy9-4)
> [[EXAMPLE_sy7#数组-判断回文数(sy7-8)|数组-判断回文数]]
```c
#include <stdio.h>  
int ishw(char *p)    
{      
	char *q;
	q=p;
    while(*q!='\0')
	{
		q++;
	}q--;
	for( ;*p!='\0'||*q!='\0';p++,q--) 
	{	
		//printf("%p: %c\n%p: %c\n\n",p,*p,q,*q);
		//通过调试发现问题 就是for的循环条件没有限制好 
		if(*q!=*p) return 0;
		if((q==p+1||q==p+2)&&*q==*p) return 1;
	}
	
    
}
main() //这地方还可以不写主函数类型的阿666
{      
	char str[20],*p=str;
    gets(p);
    if(ishw(p)) 
        printf("yes");
    else
        printf("no");    
}
```

# STRUCTURE
## 结构体-图书查阅(sy10-01)
	//定義了一個有5條記錄的圖書類型陣列，輸入待查找的書名，
	//通過調用函數 void search（struct book *p，int n，char *c）
	//在圖書陣列中查找該書名是否存在，如果存在， 顯示出該書的價格，否則顯示“No found” 
```c
#include<stdio.h>

struct  book
{    
	char name[20];
	int price;
};

void search(struct book *p,int n,char *c)   

{    
	int i,index=-1;;
	for(i=0;i<n;i++,p++)
	if(strcmp(p->name,c)==0)  //比较     
	{      			
		index++;
		printf ("price:%d",p->price); //指定价格 
		break;
	}
        if(index!=0) printf("No found");                      

}

void main()

{      
	char c[20];
	int i;
	struct  book bk[5]={{"c++",35},{"python",60},{"jsp",40},{"html",20},{"java",36}};                    
	gets(c);
	//i=sizeof(c)/sizeof(c[0]);如果自定义好了数组长度 
	//那么怎么算都是定好的那个长度(这里就是20) 
	i=0;			
	while(c[i]!='\0') //这里可以计算长度 
	{
		i++;
	}
	////////////////////
	search(bk,i,c);
}
```

## 结构体-时间换算问题(sy10-1)
	//输入当前时间以及经历的秒数,输出一个 新的时间
```c
#include<stdio.h>
struct TIME
{
	int second;
	int minute;
	int hour;
 }time; 

int main()
{
	int t;int a;
	scanf("%d:%d:%d",&time.hour,&time.minute,&time.second);
	scanf("%d",&t);a=t;//存一下最初的总秒数 
	////////////////////////////////
	time.hour+=t/3600;t-=t/3600*3600; 		
	//printf("hour:%d  t:%d\n",time.hour,t);
	time.minute+=t/60;t-=t/60*60;			
	//printf("hour:%d  t:%d\n",time.minute,t);
	if(time.minute>=60) time.hour++,time.minute-=60;
	if(time.hour==24) time.hour=0;
	time.second+=t;							
	//printf("hour:%d  t:%d\n",time.second,t);
	if(time.second>=60) time.minute++,time.second-=60;	
	////////////////////////////////	其实要注意逻辑的运算顺序就是考虑进位啥的			
	printf("After %d seconds is %d:%d:%d",a,time.hour,time.minute,time.second);
	
	return 0; 
 } 
```

## 结构体-通讯录(sy10-2)
	//輸入整數n及n個人的基本資訊。
	//輸出按照年齡由大到小的排序結果
```c
#include"stdio.h"
#define N 10
struct Date
{    int year;
     int month;
     int day;
};

struct Mate
{    
	 char name[10];
     struct Date birthday;
     char tel[12];
};
struct Mate classmate[N];

int Datecomp(struct Date d1,struct Date d2);
//d1年龄比d2大返回1，相等返回0，小则返回-1
//这里只是声明了一下 具体函数在最下面  

int main()
{    int i,j,k,n;//有个j在这里没有用到 
     struct Mate m;
     scanf("%d",&n);
     for(i=0;i<n;i++)   //输入n个同学的信息
     {   scanf("%s",classmate[i].name);//用%s直接输入字符串到数组里
         scanf("%d-%d-%d",
         &classmate[i].birthday.year,
         &classmate[i].birthday.month,
         &classmate[i].birthday.day);
         scanf("%s",classmate[i].tel);//用%s直接输入字符串到数组里
      }
      
      for(i=0;i<n-1;i++) //按照年龄降序排列
      {    
	  	 k=i;
	  	 j=i; 
	  	 while(k<=n-1)//这里加个<=就好了 也可以是<n主要是最后一个也要比较到 
	  	 {
	  	 	if(Datecomp(classmate[k].birthday,classmate[j].birthday)==1) j=k;
	  	 	k++;
		 }
	 	 k=j;
    	  //我真聪明
           if(k!=i)
          {    
		  	   m=classmate[i];
               classmate[i]=classmate[k];
               classmate[k]=m;
           }
       }
       for(i=0;i<n;i++)
           printf("name:%10s,%d-%d-%d,%12s\n",
           classmate[i].name,classmate[i].birthday.year,
           classmate[i].birthday.month,
           classmate[i].birthday.day,classmate[i].tel);
       return 0;
}

int Datecomp(struct Date d1,struct Date d2) //中规中矩 可能还有最优解法 
{
    if(d1.year<d2.year) 
    return 1;	
	else if(d1.year>d2.year) 
	return -1;   
    else if((d1.year==d2.year)&&(d1.month<d2.month)) 
    return 1;
	else if((d1.year==d2.year)&&(d1.month>d2.month)) 
	return -1;		
    else if((d1.year==d2.year)&&(d1.month==d2.month)&&(d1.day<d2.day)) 
    return 1;	
	else if((d1.year==d2.year)&&(d1.month==d2.month)&&(d1.day>d2.day))	 
    return -1;
	else return 0;
    
    
} 
```
## 结构体-复数乘积运算(sy10-3)
	////复数的成绩运算(a+bi)*(c+di)= (ac-bd)+ (bc+ad)i。
```c
#include "stdio.h"
 
struct complex
{
    double sb;
    double xb;
};
struct complex complexproduct(struct complex s1,struct complex s2);
int main()
{    struct complex c1,c2,c3;
     scanf("%lf+%lfi",&c1.sb,&c1.xb);
     scanf("%lf+%lfi",&c2.sb,&c2.xb);
     c3=complexproduct(c1,c2); 
     printf("c1*c2=%f+%fi\n",c3.sb,c3.xb);
     return 0;
}
struct complex complexproduct(struct complex s1,struct complex s2)
{ 
	 struct complex s3;
	 s3.sb=s1.sb*s2.sb-s1.xb*s2.xb;//(ac-bd) 
	 s3.xb=s1.xb*s2.sb+s1.sb*s2.xb;//(bc+ad)
	 return s3;
} 
```

## 结构体-学生成绩分析(sy10-4)
	//輸入學生人數及課程的門數，並逐個輸入學生的姓名和相應課程的成績
	//顯示各位同學單科低於該課程平均分的學生姓名及該科課程的成績，高於則不顯示
```c
#include "stdio.h"
#define N 10   //学生人数上限
#define M  5   //课程门数上限
struct stud
{   char name[10];   //姓名
    double course[M];//成绩
    double aver;     //平均分 //没用到。。很奇怪 
}s[N];

int main()
{   int n,m,i,j;  
    double sum,course[M]={0};   //用于统计每门课程的平均分//没用到sum 
    scanf("%d,%d",&n,&m);        //输入学生人数与课程门数
    //////////////////////////////
	for(i=0;i<n;i++)
    {
    	scanf("%s",&s[i].name);
    	for(j=0;j<m;j++)
    	{
    		scanf("%lf",&s[i].course[j]);
		}
	}
	for(i=0;i<m;i++)
	{
		for(j=0;j<n;j++)
		{
			course[i]+=s[j].course[i]; //j现在作为人数的变化，i此时作为课程的序号 
		}
	}
	//////////////////////////////
    printf("name      ");
    for(j=0;j<m;j++)             //求每门课程的平均分
    {    course[j]=course[j]/n;
         printf("CNO:%d   ",j+1);//显示栏目
    }
    printf("\n");
    for(i=0;i<n;i++)
    {    printf("%10s",s[i].name);
         for(j=0;j<m;j++)
            if(s[i].course[j]<course[j])
                printf("%5.1f   ",s[i].course[j]);
            else
                printf("%8c",32);//空格 
         printf("\n");    
     }
     return 0;
} 
```


# RECURSION
## 递归-更列问题(sy11-1)
> [[⬅️EXAMPLE_SY#数组-更列问题(sy5-4)]]
	//f（n）=（n-1）（f（n-2）+f（n-1）），其中 f（1）=0，f（2）=1
```c
int fun(int x)
{
	if(x==1) return 0;
	else if(x==2) return 1;
	else return (x-1)*(fun(x-2)+fun(x-1));
 } 
 
 int main()
 {
 	int n;
 	scanf("%d",&n);
 	printf("%d",fun(n));
 	
 	return 0;
 }

```

## 递归-二次幂(sy11-2)
```c
#include "stdio.h"
double power(double x,int n) //
{
	if(n==0) return 1;
    else if(n==1) return x;
    else return(x*power(x,--n));
}
int main()
{   double x,a;
    int n;
    scanf("%lf,%d",&x,&n);
    a=power(x,n);
    printf("a=%f\n",a);
    return 0;
} 
```

## 递归-斐波那契数列(sy11-3)
>[[EXAMPLE_sy8#函数-斐波那契数列(sy8-3)|函数-斐波那契数列]]

	//F(0)=0,F(1)=1, F(n)=F(n-1)+F(n-2)  (n≥2,n∈N*)
```c
#include "stdio.h"
int fib(int n)
{    
   if(n==1||n==2) return 1;
   else return fib(n-1)+fib(n-2);
}

int main()
{   int n;
    scanf("%d",&n);
    printf("NO%d=%d\n",n,fib(n));
    return 0;
}
```

## 递归-十进制转二进制(sy11-4)
>[[EXAMPLE_sy9|指针]]
```c
#include "stdio.h"
void dec2bin(int n)
{    
    if(n>=2) dec2bin(n/2);//进位 //这个n不用进行赋值也会随着递归而穷尽并计算 
	printf("%d",n%2);//求余得最高位到是0还是1 
}


int main()
{   
	int n;
    scanf("%d",&n);
    dec2bin(n);
    printf("\n");
    return 0;
}

```

## 递归-展开式求和(sy11-5)
	//计算f(x,n)=x-x^2+x^3-x^4+...+(-1)^(n-1)*x^n (n>0)
```c
#include<stdio.h>
///////////////////////////////////////这个递归式是我自己写的
double f(double x,int n) 
{
	double m=1;int i;
	if(n==1) return x;
	else 
	{
		for(i=0;i<n;i++) m*=x;
		if(n%2==0) m=-m;//偶数项带负号; 
		return  m+f(x,n-1); //递归求迭和; 
	}
}
////////////////////////////////////////这个递归式是老师写的
double f(double x,int n) 
{
	double result;
	if(n==1) result=x;
	else result=x*(1-f(x,n-1));
	return result;
}
////////////////////////////////////////

int main() 
{
	double x,a;
	int n;
	scanf("%lf,%d",&x,&n);
	a=f(x,n);
	printf("a=%f\n",a);
	return 0;
}

```


