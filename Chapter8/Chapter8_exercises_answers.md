# 条件和循环

## 8-1 看下面的代码：
```
# statement A
if x>0:
    # statement B
    pass
elif x<0:
    # statement C
    pass
else:
    # statement D
    pass
# statement E
```
(a)如果x<0, A,C和E将会被执行 <br>
(b)如果x==0, A,D和E将会被执行 <br>
(c)如果x>0, A,B和E将会被执行 <br>

## 8-2 编写程序，让用户输入3个数字：from，to和increment。以i为步长，从f计数到t。
```
f = int(input('from:'))
t = int(input('to:'))
inc = int(input('increment:'))

for i in range(f,t+1,inc):
    print(i)
```

## 8-3 用range()生成下面的列表。
#### (a) [0,1,2,3,4,5,6,7,8,9]
range(10)
#### (b) [3,6,9,12,15,18]
range(3,19,3)
#### (c) [-20,200,420,640,860]
range(-20,861,220)

## 8-4 写一个函数isprime(),如果输入的是一个素数返回True，否则返回False。
```
def isprime(n):
    if n==1: 
        return False
    for i in range(2,n//2):
        if n%i==0:
            return False

    return True
```

## 8-5 完成一个名为getfactors()函数，接受一个整型作为参数，返回它所有的约数的列表，包括1和它本身。
```
def getfactors(n):
    fact=[]
    for i in range(1,n+1):
        if n%i==0:
            fact.append(i)

    return fact

```

## 8-6 使用isprime()和getfactors()函数编写一个函数，它接受一个整型作为一个参数，返回该整型的所有素数因子的列表。如20为[2,2,5]
```
def primediv(n):
    temp = getfactors(n)
    res = []
    for i in temp:
        if isprime(i):
            while n>0 and n%i==0:
                res.append(i)
                n//=i

    return res
```

## 8-7 完全数
```
#需要借助练习8-5的getfactors()函数
def isprefect(n):
    temp = getfactors(n)
    temp.pop()
    return sum(temp)==n
```

## 8-8 阶乘
```
#使用循环
def factorial(n):
    res=1
    for i in range(1,n+1):
        res*=i

    return res
```
```
#使用递归
def factorial(n):
    if n==1:
        return 1
    return n*factorial(n-1)
```

## 8-9 斐波那契数列
```
#使用循环
def Fibonacci(n):
    f1=1
    f2=1
    f=0
    for i in range(3,n+1):
        f=f1+f2
        f1=f2
        f2=f

    return f
```
```
#使用递归
def Fibonacci(n):
    if n==1 or n==2:
        return 1
    return Fibonacci(n-1)+Fibonacci(n-2)
```

## 8-10 统计一句话中的元音，辅音以及单词的个数
```
import string
def Count(sentence):
    words = sentence.split(' ')
    word_cnt = len(words)
    vowel_cnt,con_cnt=0,0
    for word in words:
        for ch in word:
            if ch in 'aeiouAEIOU':
                vowel_cnt+=1
            elif ch in string.ascii_letters:
                con_cnt+=1

    return vowel_cnt,con_cnt,word_cnt
```
## 8-12 编写程序，用户给出start和end，然后生成一定格式的表格。
首先要知道，数字范围在[32,126]的都是可打印的，所以可以直接判断[start,end]中是否有可打印的字符。<br>
注意：程序在处理不可打印的字符时，按空格输出
```
start=int(input('start:'))
end = int(input('end:'))
Ascii = True
if end<32 or start>126: #其中的字符都不可以打印
    Ascii = False


if Ascii:
    print('DEC\t\tBIN\t\tOCT\t\tHEX\t\tASCII')
    print('-'*38)
else :
    print('DEC\t\tBIN\t\tOCT\t\tHEX')
    print('-'*31)

for i in range(start,end+1):
    print('%d\t\t'%i,end='')
    print(bin(i)[2:],end='')
    print('\t%o\t\t%x'%(i,i),end='')
    if Ascii:
        if i<32 or i>126:
            i=32        #如果是不可打印字符，则输出空格
        print('\t\t%c'%i)
    else :
        print()
```
