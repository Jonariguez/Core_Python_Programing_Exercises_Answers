# Chapter3 Python基础
## 3-1 为什么Python中不需要变量名和变量类型声明？
因为Python是动态类型语言，对象的类型和内存占用都是运行时确定的，即不像C语言有预编译过程，所以不需要预先知道对象的名字和类型，故需要声明。<br>
因为尽管python代码会被编译成字节码，但Python仍然是一种解释性语言。

## 3-2 为什么Python中不需要声明函数类型？
类似于变量，函数一样不需要声明类型，都用def进行声明定义

## 3-3 为什么应当避免在变量名的开始和结尾处使用下划线？
因为Python用下划线作为变量前缀和后缀指定特殊变量。<br>
* _xxx 不用'from module import *'导入
* \_xxx_  系统定义名字
* _xxx  类中的私有变量名

## 3-4 在Python中一行可以书写多个语句吗？
可以，各语句之间用分号(;)隔开即可。<br>
有时候会降低可读性，不提倡

## 3-5 在Python中可以将一个语句分多行书写吗？
可以，使用\\或者闭合操作符(如小括号，中括号，花括号)或者三引号'''包裹也就可以。<br>
* if condition1 and \\<br>
    &nbsp;&nbsp;&nbsp;&nbsp;    condition2:<br>
&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; execute_code
* print '''hello<br>
&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;  world'''

## 3-6 变量赋值
(a) x=1;y=2;z=3;<br>
(b) x=3;y=1;z=2; 进行正常顺序的赋值即可。

## 3-7 标识符
**int32**&emsp;40XL&emsp;\$aving\$&emsp;**printf**&emsp;**print**<br>
**_print**&emsp;**this**&emsp;**self**&emsp;**\_\_name\_\_**&emsp;0x40L<br>
**bool**&emsp;**true**&emsp;big-daddy&emsp;2hot2touch&emsp;**type**<br>
thisIsn'tAVar&emsp;**thiIsAVar**&emsp;**R_U_Ready**&emsp;**Int**&emsp;**True**<br>
**if**&emsp;**do**&emsp;count-1&emsp;**access _**<br>

其中合法标识符已加粗，合法标识符中的关键字可对照下表查找。


Python中的标识符：
* 第一个字符必须是字母或者下划线；
* 剩下的字符可以是字母和数字或下划线；
* 大小写敏感。<br>

Keyword模块中同时包含了一个关键字列表和一个iskeyword()函数。<br>
```python
>> import keyword
>> keyword.kwlist
['and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del',
'elif', 'else', 'except', 'exec', 'finally', 'for', 'from', 'global',
 'if', 'import', 'in', 'is', 'lambda', 'not', 'or', 'pass', 'print',
  'raise', 'return', 'try', 'while', 'with', 'yield']
>>> keyword.iskeyword('if')
True
```

