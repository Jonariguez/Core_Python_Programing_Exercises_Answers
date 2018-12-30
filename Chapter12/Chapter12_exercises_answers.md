# 模块

## 12-1 路径搜索和搜索路径之间有什么不同
路径搜索：在所有的文件路径里搜索某个文件的操作<br>
搜索路径：是文件系统“预定义的区域”，包含多个路径。<br>
在搜索路径里进行路径搜索，来搜索某些文件。

## 12-2 假设模块mymodule里有一个函数foo()
#### (a)把这个函数导入到你的名称空间有哪两种方法？
* `import mymodule`
* `from mymodule import foo`
  
#### (b)两种方法导入后的名称空间有什么不同
`from mymodule import foo`会将foo()函数导入到命名空间，可以直接用`foo()`来直接调用，而`import mymodule`是会把整个包导入到命名空间，调用可以用`mymodule.foo()`来调用。

## 12-3 导入"import module"和"from module import *"有什么不同
`import module`是会把包`module`导入到命名空间,访问包中的属性时需要句点属性标识来调用，如`module.attr`，而`from module import *`会将所有的属性都导入，所有的属性均可以直接使用，但是容易污染当前的命名空间，因为可能其中许多属性都用不到，而且会覆盖掉自己命名空间中同名的属性，而`import module`则不会如此。还有一个不同就是，使用`import module`导入后，每次`module.attr`时都会在module中查找attr，而通过`from module import *`再attr时则可以直接在当前的命名空间中找到(因为它是被导入进来了)，所以后者算是优化了查找过程。
<a href="https://www.cnblogs.com/yan-lei/p/7828871.html" target="_blank">参考</a><br>

## 12-4 名称空间和变量作用域有什么不同？
名称空间是纯粹意义上的名字和对象间的映射关系，而作用域还指出了从用户代码的哪些物理位置可以访问到这些名字。<br>
### Python的名称
Python 的名称(Name)是对象的一个标识(Identifier)。在 Python 里面一切皆对象，名称就是用来引用对象的，即名称是对象的引用。举个列子：`a=2`中的`2`是一个对象，而`a`是一个引用，也是一个名称。

### Python的名称空间
> 名称空间是名称到对象的映射<br>

在 Python 中，名称空间采用字典来实现。Python 的名称空间包括：
* 内置名称空间，内置名称空间包含 Python 的内置函数，如，abs()
* 模块名称空间，即全局名称空间，在模块内定义的名称
* 局部名称空间，例如，在函数（function）或者类（class）被调用时，其内部包含的名称<br>
  
不同的名称空间内的名称不会相互冲突，即使它们采用相同的名称，因为它们分属不同的名称空间。这也正是名称空间的作用。<br>
Python中一切都是object，包括function、module、class、package，这些object都在内存中真真正正的存在。每个object都有自己的namespace，并且每个object的namespace是独立的，可以通过object.name的方式访问object的namespace中的name，因此，不同的namespace中可以使用相同的name，而不会引发混淆。namespace是动态创建的，每一个namespace的生存时间也不一样。<br>
<b>注意：</b>还有一个特殊的module，一进入python解释器，就建立了一个module，这个module的namespace就是global namespace，一个全局唯一的namespace。这个module的一个内部的attribute，\_\_name\_\_等于\_\_main\_\_。如果模块是被导入的，\_\_name\_\_的值为模块的名字；如果模块是被直接执行的，\_\_name\_\_的值为’\_\_main\_\_’。

### Python变量的作用域
Python 的作用域(scope)决定了我们在程序中能否直接使用名称空间中的名称，直接访问的意思是指不需要在名称前添加名称空间的前缀。对于 Python 来说，至少存在以下三类的作用域：
* 函数作用域，包括了函数内的局部名称
* 模块作用域，包括了模块内的全局名称
* 内置作用域，包括了内置名称<br>

当在函数内部使用一个名称时，为了查找出该名称所引用的对象，Python 解释器的查找顺序为：函数名称空间 -> 模块名称空间 -> 内置名称空间.<br>
__参考博客__：<br>
<a href="https://blog.csdn.net/lihao21/article/details/79112054" target="_blank">Python 名称空间与作用域</a><br>
<a href="https://www.cnblogs.com/chenny7/p/4206497.html" target="_blank">Python 之作用域和名字空间</a><br>

## 12-5 使用__import__()
#### (a)使用__import__把一个模块导入到你的名称空间
```
Module = __import__('module')
Module.attr
```
#### (b)使用__import__从指定模块导入特定的名字
```
Module = __import__('module')
Attr = getattr(Module,'attr')
```
<a href="https://blog.csdn.net/u011010851/article/details/77948363" target="_blank">参考这里</a><br>

## 12-6 创建一个importAs()函数，这个函数可以把一个模块导入到你的名称空间，但使用你指定的名字，而不是原始的名字。
其实这就是__import__()函数的用法
```
def importAs(moduleName):
    return __import__(moduleName)
```
