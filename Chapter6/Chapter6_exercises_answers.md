# 序列:字符串、列表和元组

## 6-1 string模块中是否有一种字符串方法或者函数可以帮我鉴定下一个字符串是否是另一个大字符串的一部分

**count()** 函数:count(str,sub)返回str字符串中的sub出现的次数，若不为0，则说明出现过。并且支持输入范围*count(str,sub,beg=0,end=len(str))* <br>

**find()** 函数:find(str,sub)返回str字符串中的sub第一次出现的下标，若不为-1，则说明出现过。并且支持输入范围*find(str,sub,beg=0,end=len(str))* <br>

**index()** 函数:index(str,sub)返回str字符串中的sub第一次出现的下标。与*find()* 函数类型，唯一不同就是，当str中不存在sub时，会报错：ValueError: *substring not found* <br>


**rfind()** 函数:与find()类似，只不过返回的是最后一次出现的下标。 <br>

**rindex()** 函数:与index()类似，只不过返回的是最后一次出现的下标。 <br>

### 注意：replace()函数和split()函数也能实现该功能
**replace()** 函数:使用 *replace()* 函数的话并不像前面的那么纯粹，而是使用 **replace(str,sub,'')==str** 的返回值来判断，若返回True，则说明str中没有sub，个中缘由，自行体会，并不难理解。 <br>

**split()** 函数:同样，我们也不纯粹的利用它，而是通过 **len(split(str,sub))** 的返回值来判断，如果返回值为1，说明str中没有sub，如果大于1，则说明包含 <br>

**注意** <br>
在python 2.7中string模块包含了上述函数，即上述函数有两种方式：
* count(str,sub) (这种方式要from string import *)
* str.count(sub)<br>

在python 3.6中string模块已经没有上述函数了，所以即使import string之后也不能使用第一种方式，只能：<br>
* str.count(sub)

## 6-2 修改例6-1的idcheck.py脚本，使之可以检测长度为1的表示符，并且可以识别Python关键字。

在程序的开头添加：
```
import keyword

myIn = raw_input('Identifier to test? ')

if len(myIn)==1:
    if myIn in '_'+string.letters:
        print "Yes"
    else :
        print "No"

if myIn in keyword.kwlist:
    print "this is a keyword"
```

## 6-3 排序
#### (a) 输入一串数字、并从大到小排列之

```
>>> aList
>>> aList = [123,112,521,520,52,110,321,1,3,5]
>>> aList
[123, 112, 521, 520, 52, 110, 321, 1, 3, 5]
>>> aList.sort(reverse=True)
>>> aList
[521, 520, 321, 123, 112, 110, 52, 5, 3, 1]
```
#### (b) 很a一样，不过要用字典序输入一串数字、并从大到小排列之

## 6-4 把测试得分放到一个列表中去，计算出一个平均分

```
score = [85,96,76,84,65,80,55,74]
tot = 0
for i in range(len(score)):
    tot += score[i]

print('avg:',tot*1.0/len(score))
```


## 6-6 创建一个string.strip()的替代函数：接受一个字符串，去掉它前面和后面的空格

```
def Strip(Str):
    while Str[0]==' ':
        Str=Str[1:]

    while Str[-1]==' ':
        Str=Str[:-1]

    return Str
```

## 6-7 调试
#### (a)研究这段代码想做什么，并填写注释
```
#输入一个数字
num_str = raw_input('Enter a number:')

#转化成int类型
num_num = int(num_str)

#初始化一个列表fac_list=[1,2,...,num_num]
fac_list = range(1,num_num+1)
print("before",fac_list)

#初始化遍历指针i=0
i=0

#遍历fac_list
while i<len(fac_list):

    #如果fac_list[i]是num_num的一个因子，则将它从列表中删掉
    if num_num % fac_list[i] == 0:
        del fac_list[i]

    #i增加1
    i=i+1

#输出结果
print("after:",fac_list)
```

#### (b)本段代码有问题，找出原因
在if num_num % fac_list[i] == 0成立的话则删除fac_list[i],此时fac_list元素个数减少一个，可以理解为fac_list[i]后面的元素都向前移动一个位置,这样的话就不需要i再增加1了。

#### (c)修正(b)的问题
```
if num_num % fac_list[i] == 0:
        del fac_list[i]
        continue    #不需要下面的i+=1了
```

## 6-8 给出一个整型值，返回代表该值的英文
```
def numToEnglish(num):
    Bit = ['zero','one','two','three','four','five','six','seven','eight','nine','ten']
    Ten = ['ten','eleven','twelve','thirteen','fourteen','fifteen','sixteen','seventeen',
           'eighteen','nineteen','twenty']
    dec = ['zeros','ten','twenty','thirty','forty','fifty','sixty','seventy','eighty','ninety']

    if num==1000:
        return 'one thousand'

    if num%100 == 0:
        return Bit[num//100]+' hundred'

    if num>=100:
        ans = Bit[num//100]+' hundred and '
    else :
        ans = ''
    #print(ans)

    num %= 100
    rest = ''

    if num == 0:
        return ans
    if 1<= num <=10:
        rest = Bit[num]
    elif num<20:
        rest = Ten[num%10]
    elif num<=100:
        if num==100:
            rest =  'one hundred'
        elif num%10 == 0 :
            rest =  dec[num//10]
        else :
            rest =  dec[num//10]+'-'+Bit[num%10]

    return ans+rest
```

## 6-9 接受分钟数，返回小时数和分钟数。总时间不变，要求小时数尽可能大
```
def solve(minutes):
    hours = minutes / 60
    mins = minutes % 60
    return hours,mins
```

## 6-10 写一个函数，要求将改字符串的大小写反转

```
import string

def UpperLower(Str):
    for i in range(len(Str)):
        ch = Str[i]
        if ch not in string.letters:
            continue
        if ch in string.uppercase:
            Str = Str[:i]+ch.lower()+Str[i+1:]
        if ch in string.lowercase:
            Str = Str[:i]+string.upper(ch)+Str[i+1:]

    return Str
```

## 6-12
#### (a) 写一个findchr(string,char)函数，在string中查找字符char，返回索引，找不到则返回-1
```
def findchr(string,char):
    for i in range(len(string)):
        if string[i] == char:
            return i

    return -1
```

#### (b) 写一个rfindchr(string,char)函数，在string中查找字符char，返回char最后一次出现的位置，找不到则返回-1
```
def rfindchr(string,char):
    for i in range(len(string)-1,-1,-1):
        if string[i] == char:
            return i

    return -1
```

#### (c) 写一个subchr(string,origchar,newchar)函数，如果找到匹配的字符就用新的字符替换原先的字符。返回修改后的字符串
```
def subchr(string,origchar,newchar):
    for i in range(len(string)):
        if string[i] == origchar:
            string = string[:i]+newchar+string[i+1:]

    return string
```

## 6-14 设计“石头、剪子、步”游戏，用户输入一个选项，计算机随机一个选项，并给出结果

```
import random
option = ['Rock','Scissors','Cloth']
option_title = ['R','S','C']
print('R for Rock')
print('S for Scissors')
print('C for Cloth')
your_choice = raw_input('your choice:')

your_choice = option[option_title.index(your_choice)]
computer_choice = random.choice(option)

print('You choose: \t'+your_choice)
print('Robot choose: \t'+computer_choice)

result = ['even','lose','won']
d=option.index(your_choice)-option.index(computer_choice)
print('Result: You '+result[d])
```

## 6-18 在6.13.2节里面关于zip()函数的例子中，zip(fn,ln)返回的是什么
首先，zip()返回的是一个列表list，列表里面的每个元素是一个元组，而没个元组包含的元素个数和<br>
zip()函数的参数个数一样多。而zip返回的列表的长度由zip函数的参数中最短的那个决定<br>
而在这个问题中，<br>
fn = ['ian','stuart','david']<br>
ln = ['bairnson','elliott','paton']<br>
zip(fn,ln)返回:<br>
[('ian','bairnson'),('stuart','elliott'),('david','paton')]


