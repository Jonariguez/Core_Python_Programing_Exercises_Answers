# 函数和函数式编程

## 11-1
```python
def countToFour1():
    for eachNum in range(5):
        print eachNum,

def countToFour2(n):
    for eachNum in range(n,5):
        print eachNum,

def countToFour3(n=1):
    for eachNum in range(n,5):
        print eachNum,
```

| <b>Input</b> | countToFour1 | countToFour2 | countToFour3 |
|:---:|:---:|:---:|:---:|
| 2 | ERROR | 2 3 4 | 2 3 4 |
| 4 | ERROR | 4 | 4 |
| 5 | ERROR | None | None |
| (nothing) | 0 1 2 3 4 | ERROR | 1 2 3 4 |


## 11-5 修改习题5-7使得销售税的脚本以便让销售税不再是函数输入的必要之物。
```python
def calTax(money,ratio=0.02):
    return money*ratio
```

## 11-7 map()的使用
```python
#python2.7
>>> map(None,[1,2,3],['abc','def','ghi'])
[(1, 'abc'), (2, 'def'), (3, 'ghi')]

#python3.6
>>> map(None,[1,2,3],['abc','def','ghi'])
<map object at 0x0000002951B88320>

#使用zip
>>> zip([1,2,3],['abc','def','ghi'])
[(1, 'abc'), (2, 'def'), (3, 'ghi')]
```

## 11-8 filter()的使用
```python
#python2.7
>>> filter(lambda year: year%4==0 and year%100!=0 or year%400==0,[1999,2000,2001,2003,2004,2100,2400])
[2000, 2004, 2400]

#python3.6
>>> filter(lambda year: year%4==0 and year%100!=0 or year%400==0,[1999,2000,2001,2003,2004,2100,2400])
<filter object at 0x0000002951B95198>

#列表解析
[year for year in [1999,2000,2001,2003,2004,2100,2400] if year%4==0 and year%100!=0 or year%400==0]
```

## 11-9 reduce()的使用
```python
def average(listNum):
    return reduce(lambda x,y: x+y,listNum)/len(listNum)
```
如果使用python3的话，需要`from functools import reduce `.

## 11-10 分析下列代码做了什么
```python
files = filter(lambda x:x and x[0]!='.',os.listdir(folder))
```
将`folder`文件夹下的的文件夹和文件名称作为一个列表存在files中。

## 11-11 给定文件名，将该文件中的每一行内容的开头和结尾的空白去掉，然后再存到另一个新的文件中。再用列表解析完成一次。
```python
#方法一
filein = raw_input('Enter filename1:')
fileout = raw_input('Enter filename2:')
with open(fileout,'w') as f:
    for new_line in map(lambda line: line.strip(),open(filein,'r').readlines()):
        f.write(new_line+'\n')
    f.close()

#方法二
filein = raw_input('Enter filename1: ')
fileout = raw_input('Enter filename2: ')
with open(fileout,'w') as f:
    f.writelines(map(lambda line: line.strip(),open(filein,'r').readlines()))
    f.close()
```

## 11-12 编写timeit()函数
```python
import time
def cal(x,y):
    return x+y
def timeit(func,*kargs,**kwargs):
    start = time.time()
    retval = func(*kargs,**kwargs)
    dur = time.time()-start
    return retval,dur
```

## 11-13
#### (a)
```python
def mult(x,y):
    return x*y
```
#### (b)
```python
reduce(mult,range(1,n+1))
```
#### (c)
```python
reduce(lambda x,y: x*y,range(1,5+1))
```

#### (d)
```python
def factorial1(n):
    res=1
    for i in range(1,n+1):
        res*=i
    return res

def factorial2(n):
    return reduce(lambda x,y: x*y,range(1,n+1))

def factorial3(n):
    if n==1:
        return 1
    else:
        return n*factorial3(n-1)


import time
def timeit(func,*kargs,**kwargs):
    start = time.time()
    print('start=%.30f'%start)
    retval = func(*kargs,**kwargs)
    print('end=%.30f'%time.time())
    dur = time.time()-start
    return retval,dur

print('%d %.30f'%timeit(factorial1,20))
print('%d %.30f'%timeit(factorial2,20))
print('%d %.30f'%timeit(factorial3,20))
```
