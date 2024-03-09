#### 递归打印一个顺序阶乘
```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)

for i in range(1, 11):
    print(i, factorial(i))
```

#### 买鱼
	已知大鱼 5 元一条,中鱼 3 元一条,小鱼 1元三条。编写程序,用 100 元买 100条鱼,求能买大鱼、中鱼、小鱼各多少条。
```python
for big_fish in range(1, 21):
    for medium_fish in range(1, 35):
        small_fish = 100 - big_fish - medium_fish
        if 5 * big_fish + 3 * medium_fish + small_fish // 3 == 100 and small_fish % 3 == 0:
            print(f"大鱼{big_fish}条，中鱼{medium_fish}条，小鱼{small_fish}条")
```

#### 输入一个列表a,计算得到一个元组t,要求元组t的第一个元素是列表表a的最大值,其余元素是该最大值在列表a中的下标

```python
list2=eval(input("readin list>>>"))
max_value = max(list2)
max_index = [i for i, j in enumerate(list2) if j == max_value] # 列表推导式,记录i(i是list2的下标,j是list2的值,且该i的值j必须满足等于最大值,此时i才是最大值下标)
tuple1 = (max_value,) + tuple(max_index) # 只有一个最大值的元组加上另一个转换成元组的列表迭代器
print(tuple1)
```


#### 一个四位数中间分割之后再相加再平方等于原先四位数
```python
for num in range(1000, 10000):
    a, b, c, d = map(str, num)  # 将字符串num分割成四个位数
    ab = int(a+b) # 转成字符串后拼接成整型
    cd = int(c+d)
    abcd = int(a+b+c+d)
    if (ab+cd)**2 == abcd:
        print(" (%d + %d)^2 = %d" % (ab, cd, abcd))
```

#### 随机生成一个包含20个元素的列表,将该列表的前10个元素按降序排序,后10个元素按升序排序

```python
list3=[]
for i in range(20):
    list3.append(random.randint(1, 100))
list4=sorted(list3[:10],)
list5=sorted(list3[10:],reverse=True)
```
#### 通过旧列表计算得到新列表
	第i个元素是旧列表前i个元素的积(首元素不变)
	1) a的首元素先存入到列表b
	2) 从第二个开始,依次求出前i个元素的累积乘
	3) 利用append()追加到b列表尾部
```python
a=eval(input("list:"))
b=[a[0]]
product=1
for i in range(1,len(a)):
# len(a)是总元素个数,如果作为下标就越界了,但是恰巧range语句遍历时不包括这个下标,而是len(a)-1
	product*=i
	b.append(product)
```
#### 列表反转(其实可以直接切片或者方法reverse())
	1) 利用eval()直接输入一个列表
	2) 以列表长度一半(n//2)为中间点 (即i从0到n//2)
	3) 利用循环将a[i]和a[n-1-i]对调 
```python
a=eval(input("list:"))
i=0
while i< n//2:
	t=a[i]
	a[i]=a[n-1-i]
	a[n-1-i]=t
	i=i+1
print(a)
```

#### 列表中相差最小的两个数字
```python
a=eval(input("list:"))
a.sort(reverse=True) # 直接修改列表a,将其变成按大小降序排列
i=0
diff=0
for i in range(len(a)): # 下标从0遍历到len(a)-1
	if (a[i]-a[i-1])<diff
		diff=a[i]-a[i-1]
		num1,num2=a[i],a[i-1]
print(num1,num2)
```

#### 列表转置
```python
a = eval(input("输入一个二维数组"))

print("转置前:")
for i in range(len(a)):
    print(a[i])  # 自动换行

for i in range(len(a)):
    for j in range(i):  # 从0开始,i结束
        t = a[i][j]
        a[i][j] = a[j][i]  # 转置开始
        a[j][i] = t
        
print("转置后:")  # 自动换行
for i in range(len(a)):
    print(a[i])
```

#### 列表自定界,大于界置后,小于界置前
	随机生成一个1~20的列表
	定界后进行置前置后的操作
	注意pop()和insert(),append()的联合使用
```python
import random
a = list(range(20))
random.shuffle(a)# 打乱列表顺序
n = int(input("该列表(范围是0~20)的整数界:"))

for i in range(len(a)-1, -1, -1):
    if a[i] < n:  # 将小于n的元素置前
        a.insert(0, a.pop(i))
for i in range(len(a)-1, -1, -1):
    if a[i] > n:  # 将大于n的元素追加在后面
        # a.insert(len(a)-1, a.pop(i))
        a.append(a.pop(i))
print(a)
```

#### 列表某重复数的批量删除
	1) 要删除所有重复出现的指定数字,需要通过遍历完成
		因为remove一次只能删一个,而且还是第一个匹配项
	2) 该方法存在漏洞,remove会造成内存收缩
		但是for-in的i仍然盲目的依次索引,但其实my_list中的元素已经往前进了
```python
my_list=[1,1,1,2,1,3,1,1] 
for elem in my_list:
	if elem == 1: # 删除列表中的重复1
		my_list.remove(elem)
print(my_list)

# 以下是过程
a;[1,1,2,1,3,1,1] # 遍历索引第一个元素1,发现与1相同,删除
a:[1,2,1,3,1,1] # 遍历索引第二个元素2,发现与1不同,保留
a:[1,2,1,3,1,1] # 遍历索引第三个元素1,发现与1相同,删除
a:[1,2,3,1,1] # 遍历索引第四个元素1,发现与1相同,删除
a:[1,2,3,1] # 无第五个元素,视为遍历结束
```
	1) 代码完善:可以从后往前遍历来完成指定元素的删除,也就是从后往前删,而非从前往后删,这样remove位置之后的元素往前推进问题就可以忽视了
	2) 删掉就直接删掉,删掉后前面未遍历的元素坐标可以正常索引,不能索引的是遍历过的
```python
my_list=[1,1,1,2,1,3,1,1]
for i in range((len(a)-1,-1,-1))
	if i == 1  # 删除列表中的重复1
		my_list.remove(a[i]) # 可以直接del a[i]
```

#### 寻找列表马鞍点
	矩阵的鞍点是指矩阵中的某个元素，它是所在行的最大值，并且是所在列的最小值
	一个矩阵中可能有多个鞍点，
	1) 第一个for首先求出一行的最大值
	2) 接着第二个for接着求出该最大值的列下最小值
	3) 接着比对该最大值和该最小值是否一致
```python
a=[[2,2,3],[4,5,6],[7,5,4]]
for i in range(len(a)): # 从0开始,不包括len(a)
    print(a[i]) # 展示矩阵
count=0 # 鞍点个数
for i in range(len(a)): # 行数
	row_max=max(a[i]) # 直接找到行行最大值,但是不知道列数
    for j in range(len(a[i])): # 列数 # a[i]还是一个列表
	    if a[i][j]==row_max # i固定,j变化 # 寻找到行最大值的列数j
			column_min=a[i][j] # 先初始一个列最小值
			for k in range(len(a)) # j固定,k变化
				if a[k][j] < column_min
					column_min=a[k][j]
			if column_min==row_max
				count+=1 # 注意这个if嵌套在哪,注意外面不要再被嵌套一个if
				print(f"第{count}个鞍点:[{i},{j}]") # 注意是i不是k 
if count == 0
	print("无鞍点")
else 
	print(f"有{count}个鞍点")
```

#### 字符串匹配准确率
	使用zip()函数将原始字符串和用户输入字符串中每队对应的字符打包成一个个元组
	并构成一个zip对象(迭代器)
	通过for循环遍历每个对应字符,判断是否相等,记录总共多少字符相等
	最后将相等的次数除以字符串长度即为准确率
```python
origin=input("请输入原文:")
userinput=input("请用户输入:")
num=0
if len(origin) != len(userinput):
	print("对不起,输入的内容长度和原文必须相等")
else:
	for origin_ch,user_ch in zip(origin,userinput):
		if origin_ch == user_ch:
			num+=1
rate=num/len(origin)
print("准确率:",rate)
```

#### 统计输入字符串的单词数量
	利用split函数对字符串进行分割,split会返回一个分割好的列表
```python
text=input("输入一段英文句子,单词用空格隔开")
wordslist=text.split() # 按空格分割
print("该句的单词数量:",len(words)) # len来判断该分割好的的列表有几个元素
```

#### 打印金字塔
	计算每一行输出的*号个数和行号之间的关系,第i行的*号个数是2*i+1(i从0开始)
	其次计算最后一行所占宽度(即最后一行*号的个数),明显是2*n-1
	在循环输出每一行*号时,只要将所有*号按照最后一行输出的*号个数为宽度来居中输出
	字符串方法center()可以将*号自动居中存放
```python
n=eval(input("输入一个正整数"))
for i in range(n):
	print(('*'*(2*i+1)).center(n*2-1))
```

#### 统计单词出现频率
	首先将句子中的所有单词分割出来,并统计出不同单词出现的次数,并找出次数最大值,
	最后再根据该最大值找出所有出现次数最多的单词(出现频率最高的单词可能不止一个)
```python
text=input("请输入单词句子:")
wordslist=text.split() # 按空格分割文章
result=dict() # 定义一个字典
for word in wordslist:
	result[word]=result.get(word,0)+1 # 将单词出现次数计入到单词键的值里
max=0
for key,value in result.items():
	if value > max:
		max=value # 寻找频率最高的单词出现次数

for key,value in result.items():
	print(key,"出现次数:",value)
	
maxlist=[]
for key,value in result.items():
	if value == max:
		maxlist.append(key)
print("出现频率最高的单词:", maxlist)
```

#### 统计大小写字母以及数字字符及其他字符的个数
	要求存放到字典中并输出
```python
text=input("请输入字符串")
big_ch=0
small_ch=0
num_ch=0
other_ch=0
char_dic=dict()
for ch in text:
	if 'a'<=ch<='z':
		small_ch+=1
	elif 'A'<=ch<='Z':
		big_ch+=1
	elif '0'<=ch<='9':
		num_ch+=1
	else:
		other_ch+=1
char_dic['big_ch']=big_ch # 如果字典中没有该键会自动新增一个
char_dic['small_ch']=small_ch
char_dic['num_ch']=num_ch
char_dic['other_ch']=other_ch

for item in char_dic.items:
	print(item)
```

#### 输入整型之后转换成英文单词
```python
def convert_to_english(num_list):
    # 定义数字到英文单词的映射
    word_dict = {
        0: 'zero', 1: 'one', 2: 'two', 3: 'three', 4: 'four', 
        5: 'five', 6: 'six', 7: 'seven', 8: 'eight', 9: 'nine',
        10: 'ten', 11: 'eleven', 12: 'twelve', 13: 'thirteen', 14: 'fourteen', 
        15: 'fifteen', 16: 'sixteen', 17: 'seventeen', 18: 'eighteen', 19: 'nineteen',
        20: 'twenty', 30: 'thirty', 40: 'forty', 50: 'fifty', 
        60: 'sixty', 70: 'seventy', 80: 'eighty', 90: 'ninety'
    }
    
    # 创建空字符串
    english_str = ''
    
    # 将数字转换为字符串并反转def 
    # 列表接收字符串并逐一转换成整型后放入列表
    num_list = []
    num_list = list(map(int, str(input())))[::-1] #这里细品   
    # 遍历数字列表，从最高位到最低位
    for i in range(len(num_list)-1, -1, -1):
        # 获取当前数字
        num = num_list[i]
        
        # 如果当前位数为 0，跳过
        if num == 0:
            continue
        
        # 如果当前是百位
        if i == 2:
            english_str += word_dict[num] + ' hundred'
            # 如果百位后面还有数字，加上 "and"
            if num_list[1] != 0 or num_list[0] != 0:
                english_str += ' and '
        
        # 如果当前是十位
        elif i == 1:
            # 如果十位是 1，特殊处理
            if num == 1:
                # 获取十位和个位组成的数字
                num_tens = num_list[1] * 10 + num_list[0]
                # 在字典中查找对应的英文单词
                english_str += word_dict[num_tens]
                # 已经处理完所有数字，返回结果
                return english_str
            # 如果十位不是 1，获取对应的英文单词
            else:
                english_str += word_dict[num * 10]
                # 如果个位不是 0，加上连接词
                if num_list[0] != 0:
                    english_str += ' '
        
        # 如果当前是个位
        else:
            english_str += word_dict[num]
    
    # 返回结果
    return english_str

```
#### 元组存储列表最大值及其索引
	输入一个列表a,计算得到一个元组t
	要求元组t的第一个元素是列表表a的最大值,其余元素是该最大值在列表a中的下标
```python
list2=eval(input("readin list>>>"))
# 输入一个列表
max_value = max(list2)
# 利用max直接寻找列表的最大值
max_index = [i for i, j in enumerate(list2) if j == max_value]
# 直接用另一个列表来存储最大值的下标
# 第一个i代表不做操作,迭代器里只有i
# i,j 对应 枚举迭代器list2中的索引和值
# 随后添加一个判断,只有符合条件的j,其索引i才会被添加进迭代器
tuple1 = (max_value,) + tuple(max_index)
# 注意第一个元组还有个逗号,这才能表明元组定义
# 后面那个max_index是一个列表推导式直接转换成元组即可
# 用一个新元组来接收两个元组拼接的结果
print(tuple1)
```

#### 元组存储两个正整数的最大公约数和最小公倍数
	1) 最大公约数：
	辗转相除法的基本思想是：用较大的数除以较小的数，
	再用出现的余数去除较小的数（原先除的较小的数去除以出现的余数），
	如此反复，直到最后余数是0为止。
	如果是求两个数的最大公约数，则最后的除数就是这两个数的最大公约数。
	2) 最小公倍数：
	两个数的最小公倍数等于这两个数的乘积除以它们的最大公约数

	输入两个正整数，计算得到一个元组，
	该元组为两个整数的最大公约数，第二个元素是最小公倍数的代码：
```python
def gys(a, b):
    if a < b: # a<b时进行交换
        a, b = b, a
    while b != 0:
        temp = a % b #a作为最大数对最小数b进行求余
        a = b #a变成较小数
        b = temp #而b变成刚刚余数，准备到下一次循环被较小数除
    return a

def gbs(a, b):
    return a * b // gcd(a, b) # 两数乘积整除最大公约数

a = int(input("请输入第一个正整数："))
b = int(input("请输入第二个正整数："))
print((gys(a,b), gbs(a,b)))
```

#### 列表存储并降序打印随机数,并记录出现次数

	这个是我最起初的做法,单纯想到桶排序中的
	随机数去当了下标,出现的次数会作为元素值才存放到下标对应的位置,
```python
intlist=[]
countlist=[0]*101
for i in range(100):
    num=random.randint(0,100)
    countlist[num]+=1 #该处的次数自增一次
for i in countlist: 
    if i != 0:#遍历寻找出现次数不为0的数字
        print(f"{countlist.index(i)}:{i}次") #下标才是对应数字
        intlist.append(countlist.index(i))
        countlist[countlist.index(i)]=0 
        #追加完之后要把数字的次数归零,因为index()会根据出现次数去找对应数字,如果没有归零,要是有两个数字的出现次数一样且有个数字在另一个数字前面,就又会回到那个数字那里,归零之后,会确保每次index()都不会去查询已经访问过的数字
intlist.sort(reverse=True)
print(intlist)
```
	解释一下sorted_list = sorted(list(set(intlist)), reverse=True)
		`intlist`是一个列表，用于记录每个数字在100个随机数中出现的次数。
		由于随机数的范围是0到100，因此`intlist`中可能包含重复的数字。
		如果不使用`set`函数将列表转换为集合，
		那么`sorted_list`列表将包含多个相同的值，
		这些值是在`intlist`中出现的不同数字的数量。
		使用`set`函数将`intlist`转换为集合，可以消除这些重复的数字，
		并将其作为元素放入集合中。然后将集合转换为列表并对其进行排序，
		以便获得`intlist`中不同数字的数量的有序列表
```python
	import random
	
	intlist=[0]*101
	countlist = [0] * 101
	index=0
	for i in range(100):
	 num = random.randint(0, 100)
	 countlist[num] += 1
	for i, count in enumerate(countlist):
	if count != 0:
		print(f"{i}:{count}次")
		intlist[index]=i # 存储下标i
		index+=1 
	sorted_list = sorted(list(set(intlist)), reverse=True)
	print("THE lIST>>\n",sorted_list)
```
#### 统计随机数出现的次数,并按不同数字出现的次数降序输出
	`countdict.items()`返回一个包含字典`countdict`中所有键值对的元组列表。其中每个元组包含两个值：键和值
	`lambda item: item[1]`,该匿名函数传入item这个元组表示对每个元组（即字典中的键值对）取第二个元素作为排序键,
	`reverse=True`参数指示将序列进行降序排序，因此，在排序后，元组中出现次数较多的数字排在前面。
```python
countdict={}
newcountlist=[0]*101
for i in range(200):
    num=random.randint(0,100)
    newcountlist[num]+=1
for j in newcountlist:
    if j !=0:
        countdict[newcountlist.index(j)]=j
        newcountlist[newcountlist.index(j)]=0
print(countdict)
sorteddict = dict(sorted(countdict.items(), key=lambda item: item[1],reverse=True))
print(sorteddict)
```


#### 解决重复连续出现的单词,查重并进行纠正
```python
article2 = "apple apple is fruit"
mylist = article2.split(" ")
antherlist = [0]
antherlist[0] = mylist[0]
for i in range(1, len(mylist)):
    if mylist[i] != antherlist[-1]:
        antherlist.append(mylist[i])
antherlist = " ".join(antherlist)
print(antherlist)
```
	1.  `article2 = "apple apple is fruit"`：定义了一个字符串变量`article2`，其中包含了一段文本。
	2.  `mylist = article2.split(" ")`：将`article2`字符串按空格进行分割，生成一个列表`mylist`，列表中的每个元素都是原始字符串中的一个单词。
	3.  `antherlist = [0]`：创建了一个名为`antherlist`的列表，并将初始值设为0。
	4.  `antherlist[0] = mylist[0]`：将`mylist`列表中的第一个单词赋值给`antherlist`列表的第一个元素    
	5.  `for i in range(1, len(mylist)):`：使用`for`循环遍历`mylist`列表中除了第一个元素之外的所有元素。  
	6.  `if mylist[i] != antherlist[-1]:`：检查当前遍历到的单词是否与`antherlist`列表的最后一个元素不同。
	7.  `antherlist.append(mylist[i])`：如果当前单词与`antherlist`列表的最后一个元素不同，将当前单词添加到`antherlist`列表中。
	8.  `antherlist = " ".join(antherlist)`：使用空格将`antherlist`列表中的元素连接成一个字符串。
	9.  `print(antherlist)`：打印输出最终得到的字符串

#### 输入一个字符串,判断回文串

```python
palindrome = input("input the string:")
reverse_palindrome = palindrome[::-1] # 新建一个列表来接收倒序的原数组
if reverse_palindrome == palindrome: # 直接比较两个列表是否相等即可
    print("This is the palindrome!!")
else:
    print("This isn't a palindrome!!")
```

