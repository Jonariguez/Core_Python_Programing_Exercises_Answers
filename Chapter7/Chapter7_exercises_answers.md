# 映像和集合类型

## 7-1 哪个字典方法可以用来把两个字典合并到一起？
**update()方法** dict1.update(dict2)<br>
update()方法可以用来将一个字典的内容添加到另外一个字典中。字典中原有的键重复，那么重复键所对应的原有条目的值将被新键所对应的值覆盖。原来不存在的条目会被添加到字典中。

## 7-2 我们知道字典的值可以是任意的，但是键呢？查看哪些类型可以作为键，那些不行？并解释不行的原因
键并不是所有类型都可以的，有一定限制。<br>
除了数字和字符串以外，还有元组类型和不可变集合类型frozenset也可以作为键，而列表和字典则不可以。<br>
像列表和字典不能作为字典的键，是因为他们都属于可变类型。字典之所以要求其键不可变是因为：字典中是利用键的值来计算数据的存储位置的，即数据存在哪里由键的值唯一确定，如果键值改变，则哈希函数会映射到不同的地址，很明显如此便不可能可靠地存储和访问数据，故为了避免这种情况的发生，要求键是不可变的。
所以可变对象不能作为字典的键。

## 7-3 
#### (a)创建一个字典，并把键按字母顺序显示出来。
```
>>> dic = {'a':'d','b':'c','c':'b','d':'a'}
>>> print(sorted(dic.keys()))
['a', 'b', 'c', 'd']
```
#### (b)根据已按字母顺序排序好的键，显示这个字典的键和值。
```
>>> dic = {'a':'d','b':'c','c':'b','d':'a'}
>>> for k in sorted(dic.keys()):
	print(k,dic[k])
a d
b c
c b
d a
```
#### 同(c)，不过按照值排序后，显示这个字典的键和值。
```
>>> for v in sorted(dic.values()):
	for k in dic:
		if dic[k]==v:
			print(k,v)
			
d a
c b
b c
a d
```

## 7-4 给定两个长度相同的列表，如[1,2,3]和['a','b','c']，用这两个列表中的所有数据组成一个字典，如{1:'a',2:'b',3:'c'}
```
>>> a=[1,2,3]
>>> b=['a','b','c']
>>> c=dict(zip(a,b))
>>> c
{1: 'a', 2: 'b', 3: 'c'}
```
## 7-5 
(c)和(d)没有实现
```
#coding=utf-8
import string
import time
import math

db={}
reg_table=string.ascii_letters+string.digits

#判断用户名是否合法
def checkName(name):
    for c in name:
        if c not in reg_table:
            print('Illegal Username')
            return False
    return True


def newuser(name):
    pwd = input('passwd:')
    #一个用户名对应一个列表，列表里放的是密码和上次次的登录时间(上次登录时间初始为-1)
    db[name.lower()]=[pwd,-1]
    print('welcome ',name)

#老用户登录方法
def olduser(name):
    pwd = input('passwd:')
    passwd,last_time = db.get(name.lower())
    #取出当前时间
    cur = time.time()
    if passwd == pwd :
        print('welcome back',name)
        #按秒计算两次登录是否超过4个小时
        if last_time >= 0 and math.floor(cur-last_time)<=4*60*60:
            print('You alredy logged in at:'+time.ctime(last_time))
        db[name.lower()]=[passwd,cur]
    else :
        print('password incorrect')

def login():
    prompt = 'username:'
    while True:
        name = input(prompt)
        #输入不合法
        if checkName(name)==False:
            continue
        #用户名已存在
        if name.lower() in db:
            olduser(name.lower())
            return
        else :
            ch = input('This is a new username, create new account?[y/n]')
            if ch.lower() == 'y':
                newuser(name.lower())
                return
            else :
                print('Input the right username')
                continue


def manage():
    prompt = '''
    (D)elete An User
    (S)how Info
    '''

    try:
        choice = input(prompt).strip()[0].lower()
    except (EOFError,KeyboardInterrupt):
        choice = 'q'
    print('\nYou picked: [%s]'%choice)
    if choice not in 'ds' or choice=='q':
        print('Back')
        return
    if choice=='d':
        name = input('input the username you want to delete:')
        if checkName(name)==False:
            return
        if name.lower() not in db:
            print('No such username')
            return
        del db[name.lower()]
        print('Delete done')
    elif choice=='s':
        for name in db:
            print(name,db[name.lower()][0])


def showmenu():
    prompt = '''
    (U)ser Login
    (Q)uit
    (M)anage
    Enter choice:
    '''

    done = False

    while not done:
        chosen = False

        while not chosen:
            try:
                choice = input(prompt).strip()[0].lower()
            except (EOFError,KeyboardInterrupt):
                choice = 'q'
            print('\nYou picked: [%s]'%choice)
            if choice not in 'uqm':
                print('invalid option,try again')
            else :
                chosen = True

            if choice == 'q':done=True
            if choice == 'u':login()
            if choice == 'm':manage()

if __name__=='__main__':
    showmenu()

```

## 7-7 颠倒字典中的键和值
```
#保证dic的值都是可哈希的
def inv(dic):
    dict_res = dict([(dic[k],k) for k in dic])
    return dict_res
```

## 7-8 人力资源
```
db={}

def add():
    name=input('Name:')
    Id = input('id_No:')
    if name in db:
        print('Existed')
    else :
        db[name]=Id

def show_name():
    for name in sorted(db.keys()):
        print(name,db[name])

def show_id():
    for Id in sorted(db.values()):
        for name in db.keys():
            if db[name]==Id:
                print(name,Id)

def showmenu():
    prompt = '''
    (A)dd employee
    show_in_(N)ame
    show_in_(I)d
    Enter choice:
    '''

    done = False

    while not done:
        chosen = False

        while not chosen:
            try:
                choice = input(prompt).strip()[0].lower()
            except (EOFError,KeyboardInterrupt):
                choice = 'q'
            print('\nYou picked: [%s]'%choice)
            if choice not in 'aniq':
                print('invalid option,try again')
            else :
                chosen = True

            if choice == 'q':done=True
            if choice == 'a':add()
            if choice == 'n':show_name()
            if choice == 'i':show_id()

if __name__=='__main__':
    showmenu()
```

## 7-9 翻译
#### 能满足(a)和(c)
```
#coding=utf-8
def tr(srcstr,dststr,string):
    #如果两者相等，则不用翻译
    if srcstr == dststr:
        return string
    while True:
        idx = string.find(srcstr)
        if idx==-1:
            break
        string=string[:idx]+dststr+string[idx+len(srcstr):]
    return string
```
#### (b)区分大小写
```
#coding=utf-8
#默认是flag=0即区分大小写，flag=1时不区分大小写
def tr(srcstr,dststr,string,flag=0):
    #如果两者相等，则不用翻译
    if srcstr == dststr:
        return string
    new_srcstr=[srcstr,srcstr.lower()]
    while True:
        #每次都要用string更新一次new_string
        new_string=[string,string.lower()]
        idx = new_string[flag].find(new_srcstr[flag])
        if idx==-1:
            break
        string=string[:idx]+dststr+string[idx+len(srcstr):]
    return string

print(tr('Abcdef','abc','aBcdefdef',1))
#输出为abc，而不是abcdef，请体会
```

## 7-10 
#### (a)
```
#coding=utf-8
import string
def encrypt(plain):
    cipher = ''
    for char in plain:
        if char not in string.ascii_letters:
            cipher += char
        elif char in string.ascii_lowercase:
            cipher += string.ascii_lowercase[(string.ascii_lowercase.find(char)+13)%26]
        else :
            cipher += string.ascii_uppercase[(string.ascii_uppercase.find(char)+13)%26]

    return cipher

print(encrypt('This is a short sentence'))
```

## 7-11 什么组成字典中合法的键？举例说明字典中合法的键和非法的键
如果某对象是可哈希的，那么该对象可以作为字典中合法的键。<br>
比如合法的键有：数字，字符串，元组，和集合frozenset()<br>
不合法的键有：列表，字典，集合set。

## 7-12
#### (a) 数学上，什么是集合？
包含不同元素的集称为集合。
#### (b) Python中集合的定义是什么？
集合是一种类型，集合对象是由一组不同的、无序排列的、可哈希的值组成的集合。包括set和frozenset两种。<br>
**注意：集合里面的元素一定是可哈希的，比如set([[1,2],[3,4]])会出错，因为set里面的元素[1,2]和[3,4]为列表类型，列表不可哈希**

## 7-13 
```
import random
num_cntA = random.randint(1,10)
A = set([random.randint(0,9) for i in range(num_cntA)])
print(A)
num_cntB = random.randint(1,10)
B = set([random.randint(0,9) for j in range(num_cntB)])
print(B)
print(A|B)
print(A&B)
```
