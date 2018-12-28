# 文件和输入输出

## 9-1 显示一个文件的所有行，忽略以#之后的内容
```
import os
f=open('input.txt','r')
for line in f:
    idx = line.find('#')
    if idx==0:
        line=''
    elif idx>0:
        line=line[:idx]+os.linesep
    print(line,end='')

```

## 9-2 提示输入数字N和文件F，然后显示文件F的前N行
```
N=int(input('Enter a N:'))
F=str(input('Enter filename:'))
f=open(F,'r')
for i,line in enumerate(f):
    if i==N:
        break
    print(line,end='')
```

## 9-3 输入文件名，然后显示该文本文件的总行数
```
#readlines()会返回一个列表，每个元素是一个字符串代表文件中的一行内容
Filename=str(input('Enter filename:'))
f=open(Filename,'r')
print(len(f.readlines()))
```

## 9-4 输入文件名之后每次显示25行，然后询问是否继续显示
```
Filename=str(input('Enter filename:'))
f=open(Filename,'r')
page = 25
for i,line in enumerate(f):
    print(line,end='')
    if i%page==0:
        input('press Enter to continue...')
```

## 9-5 比较两个文件，如果不同，找到第一处不同的行号和列号
```
F1=str(input('Enter filename1:'))
F2=str(input('Enter filename2:'))
f1=open(F1,'r').readlines()
f2=open(F2,'r').readlines()
min_row=min(len(f1),len(f2))
for row in range(min_row):
    line1 = f1[row]
    line2 = f2[row]
    L1=len(line1)
    L2=len(line2)
    L=min(L1,L2)
    for col in range(L):
        if line1[col]!=line2[col]:
            break
    else:
        col+=1  #这里+1是为了向后移动一下说明不同发生在这之后一个
    if L1!=L2 or col<L:
        print('Difference takes place at %d row %d colum'%(row+1,col+1))    #这里+1是为了转换为行数和列数都是从1开始的
        break

else:
    row+=1     
    if len(f1)==len(f2):
        print('Full same')
    else:
        print('Difference takes place at %d row %d colum'%(row+1,1))

```
**注意：** 如果一个文件是:<br>
`abc`<br>
另一个文件是:<br>
`abc`<br>
`ab`<br>
那么不同之处是在1行4列，而不是2行1列，因为文件1中abc后面没有回车，而另一文件有，这也是不同之处。

## 9-8 模块研究
```
m=input('pls input a module name: ')
module=__import__(m)  
ml=dir(module)
for i in ml:  
    print('name: ',i)
    print('type: ',type(getattr(module,i)))
    print('value: ',getattr(module,i))
```
[参考这里](https://blog.csdn.net/qq_20113327/article/details/61202711)
