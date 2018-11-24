# 数字

## 5-1 讲讲Python普通整型和长整型的区别
Python中的普通整型与C语言中的int类似，根据机器的不同和采用的编译器不同，普通整型可以是32位或者64位的，所能表达的范围分别为-2<sup>31</sup>~2<sup>31</sup>-1或-2<sup>63</sup>~2<sup>63</sup>-1。<br>
Python中的长整型与C语言的long并不是一个概念，Python的长整型可以很<b>轻松地表示很大的数</b>，即并不局限于2<sup>63</sup>这样的范围，而具体能表示多大的数值，这与计算机的内存大小有关。即在资源范围内，能表示多大就表示多大。

## 5-3 写一段脚本，输入一个测试成绩，输出他的平分成绩(A-F)
### A: 90\~100 B: 80\~89 C: 70\~79 D: 60\~69  F:<60

```
grade = int(input('请输入成绩:'))

if 90<=grade<=100:
    print('A')

if 80<=grade<90:
    print('B')

if 70<=grade<80:
    print('C')

if 60<=grade<70:
    print('D')

if grade<60:
    print('F')
```

## 5-4 判断给定年份是否是闰年。

```
year = int(input('请输入年份:'))

if year%4 == 0 and year%100 != 0 or year%400 == 0:
    print('It is a leap years')
else :
    print('It is not a leap years')

```

## 5-5 取任意小于1美元的金额，然后计算可以换成最少多少枚硬币。
因为要求最少，所以肯定是先换面额大的硬币，然后再依次换小一点面额的金币。

```
money = float(input('请输入金额:'))
money = int(money*100)  #转换成美分

coins = 0

coin,remains=divmod(money,25)   #兑换25美分的硬币能换coin个，并剩remains美分
coins += coin
money=remains

coin,remains=divmod(money,10)
coins += coin
money=remains

coin,remains=divmod(money,5)
coins += coin
money=remains

coin,remains=divmod(money,1)
coins += coin

print('the leastest: %d' % coins)
```

## 5-9 回答关于数值格式的问题：
(a)
```
>>> 17 + 32
49
```
正常十进制运算
```
>>> 017 + 32
47
```
017前面以0开头是八进制表示，32是十进制，结果47也是十进制
```
>>> 017 + 032
41
```
017和032都是八进制数。41是十进制<br>
**注意**:在python2里0和0o开头都能表示这个数是八进制，而在python3里只用0o，0开头已经不适用。

(b)
```
>>> 56l+78l
134L
```
输入不是561和781而是56l和78l，即结尾是字符'l'，不是数字1，所以结果也就显而易见了。

## 5-10 写一对函数来进行华氏度为摄氏度的转换。

```
#输入temp_F为华氏度
def F_to_C(temp_F):
    temp_C = (temp_F-32)*5/9
    return temp_C
```
## 5-11 
#### (a) 使用循环和算术运算，求出0~20之间的所有偶数
```
#求出[0,20]的偶数
for i in range(21):
    if i%2 == 0:
        print(i)
```

#### (b) 同上，求出0~20之间的所有奇数
```
#求出[0,20]的奇数
for i in range(21):
    if i%2 == 1:
        print(i)
```

#### (c) 综合(a)和(b)，请问辨别奇数和偶数的最简单方法是什么？
直接将该数对2取余，余数为0则为偶数，否则为奇数。

#### (d) 使用(c)的成果，检测一个整型能否被另一个整型整除
直接用取余即可
```
def divisible(divisor,be_divisor):
    if divisor % be_divisor ==0:
        return True
    return False
```

## 5-13 写一个函数把由小时和分钟表示的时间转换为只用分钟表示的时间
小时*60+分钟数即可
```
def Time():
    Clock = str(input('输入时间:'))
    Hour,Min=Clock.split(':')
    Hour = int(Hour)
    Min = int(Min)
    return Hour*60+Min
```
## 5-14 写一个函数，以定期存款利率为参数，假定该账户每日计算复利，请计算并返回年回报率
假设本金是a，利率为r，那么一天之后本金变为a*(1+r)，两天后是a*(1+r)\*(1+r),... 一年后为a*(1+r)<sup>365</sup>,
回报率为(a*(1+r)<sup>365</sup>-a)/a=(1+r)<sup>365</sup>-1.
```
def Rate_of_Return(r):
    return (1+r)**365-1
```

## 5-15 计算两个整型的最大公约数和最小公倍数
```
#最大公约数
def gcd(a,b):
    if b==0:
        return a
    return gcd(b,a%b)
```

```
#最小公倍数
def lcm(a,b):
    return a//gcd(a,b)*b
```

## 5-16 写一个Payment()函数实现如课本所示的功能

```
def Payment():
    money = float(input('Enter opening balance:'))
    cost = float(input('Enter monthly payment:'))

    print('\t\tAmout Remaining')
    print('Pymt#\tPaid\tBalance')
    print('-----\t------\t---------')
    for i in range(13):
        if money <= 0.0:
            break
        print(str(i)+'\t',end='')
        if i==0:
            print('$0.00\t$'+str(money))
        else :
            if money <= cost:
                cost,money = money,0
            else :
                money -= cost
            print('$'+str(cost)+'\t$'+str(money))
```