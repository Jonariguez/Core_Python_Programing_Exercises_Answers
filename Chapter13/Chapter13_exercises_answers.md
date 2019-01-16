# 面向对象编程

## 13-1 请列举一些面向对象编程与传统旧的程序设计形式相比的先进之处

首先结构化的编程可以把程序组织成逻辑块，以便重复使用；创建程序的过程变得更具逻辑性。而面向对象编程增强了结构化编程，实现了数据与动作的融合：数据层和逻辑层可以由一个可用以创建这些对象的简单抽象层来描述。

## 13-2 函数与方法之间的区别是什么？

* 函数定义在python程序中，而方法要定义在class类中。
* 方法有一些比如私有方法(\_\_fun())和特殊方法(\_\_fun\_\_())等特殊方法，而函数没有
* 函数可以在程序运行的时候随时调用，而方法是与类实例的绑定的，即方法需要绑定到实例才能被调用(静态方法除外)。
* 通过类对象访问的类中定义的函数，是属于函数调用；而通过实例对象调用类中的函数，则属于方法调用，使用句点(instan.method())。而且前者需要自己手动传入self实例，后者python解释器会自动传入。

使用下面的代码来帮助理解
```python
>>> class MyClass(object):
	def __init__(self):
		self.name='Name'
	def func(self):
		print(self.name)

		
>>> myclass=MyClass()
>>> myclass.func()
Name
>>> MyClass.func()  #说明需要手动传入self实例参数
Traceback (most recent call last):
  File "<pyshell#8>", line 1, in <module>
    MyClass.func()
TypeError: func() missing 1 required positional argument: 'self'
>>> MyClass.func(myclass)
Name
>>> from types import FunctionType,MethodType
>>> print(isinstance(myclass.func,FunctionType))
False
>>> print(isinstance(myclass.func,MethodType)) #实例调用属于方法调用
True
>>> print(isinstance(MyClass.func,FunctionType)) #类对象调用属于函数调用
True
>>> print(isinstance(MyClass.func,MethodType))
False
```

参考博客  
https://www.cnblogs.com/logo-88/p/8361769.html  
https://www.cnblogs.com/zzy-9318/p/9192802.html

## 13-3 
无附加题的类定义如下：
```python
#coding=utf-8

def dollarize(num):
    return ('-$' if num<0 else '$')+format(abs(num),',.2f')

class MoneyFmt(object):
    def __init__(self,money):
        self.money=money

    def update(self,new_money):
        self.money=new_money

    def __nonzero__(self):
        # 初始时采用如下方式
        #return self.money!=0
        return self.money>=1.0

    def __repr__(self):
        return self.money

    def __str__(self):
        return dollarize(self.money)
```

附加题代码：
```python
#coding=utf-8

def dollarize(num):
    return ('-$' if num<0 else '$')+format(abs(num),',.2f')

class MoneyFmt(object):
    def __init__(self,money,Minus=True):
        self.money=money
        self.Minus=Minus    #是否使用负号，默认为True

    def update(self,new_money):
        self.money=new_money

    def __nonzero__(self):
        # 初始时采用如下方式
        #return self.money!=0
        return self.money>=1.0

    def __repr__(self):
        return self.money

    def __str__(self):
        Money = dollarize(self.money)
        if not self.Minus and self.money<0:
            Money = '<'+Money[1:]+'>'
        return Money
```

## 13-4用户注册
```python
#coding=utf-8
import time

class User(object):
    def __init__(self,username,password,logintime=0):
        self.username=username
        self.password=password
        self.logintime=float(logintime)

    def writeFile(self,filepath):
        with open(filepath+self.username+'.txt','w') as f:
            f.write('%s: %s: %.2f'%(self.username,self.password,self.logintime))
            f.close()


class UserDataBase(object):
    def __init__(self,filepath):
        self.filepath=filepath
        self.allUsers=[]
        try:
            with open(self.filepath+'allUsers.txt','r') as f:
                self.allUsers.append(f.readlines())
                f.close()
        except IOError:
            self.allUsers=[]

    def isExist(self,username):
        return username in self.allUsers

    def new_user(self,username,password,logintime=0):
        if self.isExist(username):
            return None         #用户名已存在
        new_user = User(username,password,logintime)
        self.allUsers.append(username)  #申请了新用户，就添加到用户库里
        new_user.writeFile(self.filepath)   #将自己写到本地
        return new_user

    def login(self,username,password,logintime):
        if not self.isExist(username):
            return (0,0)        #返回0，说明不存在
        user = self.get_user_with_name(username)
        if user.password!=password:
            return (-1,0)       #返回-1，说明密码不对
        if user.logintime==0:
            user.logintime=logintime
            user.writeFile(self.filepath)   #修改之后要写回到自己的username.txt文件中去，下同
            return (-2,0)       #返回-2，说明第一次登陆
        time_interval = user.logintime-logintime
        last_time = user.logintime
        user.logintime=logintime
        user.writeFile(self.filepath)
        return (time_interval,last_time)    #返回大于0的，说明是时间间隔


    def get_user_with_name(self,username):
        if not self.isExist(username):
            return None
        with open(self.filepath+username+'.txt') as f:
            name,pwd,lgtm=f.readline().split(': ')
            f.close()
            user = User(name,pwd,lgtm)
            return user

    def update_info(self,username,ch,new_info):
        if not self.isExist(username):
            return None
        with open(self.filepath+username+'.txt','r') as f:
            info=f.readline().split(': ')
            info[ch]=new_info
            if ch==0:
                self.allUsers.remove(username)
                self.allUsers.append(new_info)
            new_user = User(*info)
            new_user.writeFile(self.filepath)
            return new_user

    def quit(self):
        with open(self.filepath+'allUsers.txt','w') as f:
            f.writelines(self.allUsers)
            f.close()


class Manager(object):
    def __init__(self,filepath):
        self.filepath = filepath
        self.manager = UserDataBase(self.filepath)

    def new_user(self):
        username = input('输入用户名: ')
        password = input('输入密码  : ')
        # 在这里进行用户名和密码的规则验证
        user = self.manager.new_user(username,password,0)
        if user == None:
            print('用户名已存在')
        else :
            print('注册成功')

    def login(self):
        username = input('输入用户名:')
        password = input('输入密码  :')
        # 在这里进行用户名和密码的规则验证
        retval,last_time = self.manager.login(username,password,time.time())
        if retval == 0:
            print('用户不存在')
        elif retval == -1:
            print('密码不正确')
        else :
            print('登陆成功')
            if retval == -2 or retval>4*60*60:
                print('welcome ',username)
            else:
                Time = time.gmtime(last_time)
                print('welcome back ',username,', you already logged in at: %d-%d-%d %d:%d:%d'%\
                      (Time.tm_year,Time.tm_mon,Time.tm_mday,Time.tm_hour,Time.tm_min,Time.tm_sec))


M=Manager('')   #空字符串代表当前目录下
for i in range(10):
    c=int(input('ch:'))
    if c==1:
        M.new_user()
    elif c==2:
        M.login()
    elif c==0:
        break

```







