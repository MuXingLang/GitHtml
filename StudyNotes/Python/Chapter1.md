# 第一章：基础知识

## 一：编码格式

​		默认情况下，Python 3 源码文件以 **UTF-8** 编码，所有字符串都是 unicode 字符串。

​		当然你也可以为源码文件指定特定的编码：

```python
# -*- coding:utf-8 -*-
```

​		一般情况下，推荐python文件第一行中指定编码格式

## 二：python保留字

​		保留字即关键字，我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

## 三：标识符

- 第一个字符必须是字母表中字母或下划线 **_** 。
- 标识符的其他的部分由字母、数字和下划线组成。
- 标识符对大小写敏感。

## 四：注释

Python中单行注释以 # 开头，实例如下： 

```python
#!/usr/bin/python3
 
# 第一个注释
print ("Hello, Python!") # 第二个注释

'''
这是多行注释，用三个单引号
这是多行注释，用三个单引号 
这是多行注释，用三个单引号
'''

"""
这是多行注释，用三个双引号
这是多行注释，用三个双引号 
这是多行注释，用三个双引号
"""
```
特殊注释：

 1. 指定编码格式

    ```python
    # -*- coding:utf-8 -*-
    ```
    
 2. 指定python版本

    ```python
    #!/usr/bin/python3
    ```

## 五：行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号 **{}** 。

缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。实例如下： 

```python
if True:
    print ("True")
else:
    print ("False")
```

## 六：多行语句

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠(\)来实现多行语句，例如：

```python
total = item_one + \
        item_two + \
        item_three
```

在  [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)，例如： 

```python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

## 七：空行

​		函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

​		空行与代码缩进不同，空行并不是Python语法的一部分。书写时不插入空行，Python解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

​		**记住：**空行也是程序代码的一部分。

## 八：同一行显示多条语句

Python可以在同一行中使用多条语句，语句之间使用分号(;)分割，以下是一个简单的实例：

```python
#!/usr/bin/python3   
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

## 九：多个语句构成代码组

​		缩进相同的一组语句构成一个代码块，我们称之代码组。

​		像if、while、def和class这样的复合语句，首行以关键字开始，以冒号( : )结束，该行之后的一行或多行代码构成代码组。

​		我们将首行及后面的代码组称为一个子句(clause)。

​		如下实例：

```python
if expression : 
   suite
elif expression : 
   suite 
else : 
   suite
```

## 十：print输出

### 1.一般打印

print 默认输出是换行的，如果要实现不换行可自定义结束字符串，以替换默认的换行符。例如：如果将结束字符串指定为空字符串，以后就可以继续打印当前行，即在变量末尾加上 **end=""**：

```python
#!/usr/bin/python3   
x="a" 
y="b" 
# 换行输出 
print( x ) 
print( y )   
print('---------') 

# 不换行输出 
print( x, end=" " ) 
print( y, end=" " ) 
print()
```

以上实例执行结果为：

```
a
b
---------
a b
```

### 2.原样输出

Python打印所有字符串时，都会用引号将其括起。这是因为Python在打印值时，会保留其在代码中的样子，而不是你希望用户看到的样子。但如果你使用print时，例子如下：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> 'hello,world!'
'hello,world!'
>>> print('hello,world!')
hello,world!
>>> 'hello,\nworld!'
'hello,\nworld!'
>>> print('hello,\nworld!')
hello,
world!
>>> 
```

你还可以使用函数str（一个类）和repr（一个函数）实现类似的效果，如下所示：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> s = 'hello,\nworld!'
>>> print(str(s))
hello,
world!
>>> print(repr(s))
'hello,\nworld!'
>>> 
```

### 3.多表达式打印

Python支持打印多个表达式，条件是使用逗号分割他们：

```powershell
>>> print('Age:',24)
Age: 24
```

如你所在，在参数之间插入一个空格字符。在你要合并文本和变量值，而又不想使用字符串穿功能时，会更方便：

```powershell
>>> name = 'Gumby'
>>> salutation = 'Mr.'
>>> greeting = 'Hello,'
>>> print(greeting, salutation, name)
Hello, Mr. Gumby
```

如果变量greeting中不包含逗号，该怎么添加逗号？

```powershell
>>> print(greeting， '， ',salutation, name)
SyntaxError: invalid character in identifier
```

显然上述方法是不可行，因为这将在逗号前添加一个空格，以下是一种解决方案：

```powershell
>>> print(greeting + '， ',salutation, name)
Hello，  Mr. Gumby
```



## 十一：import 与 from...import

可将模块视为扩展，通过将其导入以扩充Python功能

在 python 用 **import** 或者 **from...import** 来导入相应的模块。 

 将整个模块(somemodule)导入，格式为： **import somemodule**

 从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

 从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

 将某个模块中的全部函数导入，格式为： **from somemodule import \***

**1.  导入 sys 模块**

```python
import sys 
print('================Python import mode=========================='); 
print ('命令行参数为:') 
for i in sys.argv:     
	print (i) 
print ('\n python 路径为',sys.path)
```

**2. 导入 sys 模块的 argv,path 成员**

```python
from sys import argv,path  #  导入特定的成员   
print('================python from import===================================') print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path
```

如果有两个模块，它们都包含一个函数open，可在导入两模块后通过模块名加点加open即以下格式调用：

```
module1.open(...)
module2.open(...)
```

还有另一种方法，在导入语句末尾添加as子句并指定别名，例子如下：

```powershell
>>> import math as foobar
>>> foobar.sqrt(4)
2.0
```

又或者这样：

```powershell
>>> from math import sqrt as foobar
>>> foobar(4)
2.0
```

## 十二：builtins

内建命名空间，不需要引入，其中小写字母开头的是BIF(built-in functions)内置函数

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> dir(__builtins__)
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'WindowsError', 'ZeroDivisionError', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
```

## 十三：语句和表达式

表达式作为程序的一部分，结果是一个值。

语句是让计算机执行特定操作的指示。

通俗的讲：表达式是一些东西，而语句是做一些事情。所有的语句都有一个根本特征：执行修改操作。比如，赋值语句改变变量，输出语句更改屏幕显示内容，例子如下：

```python
x = 1		#赋值语句
y = 2		#赋值语句
z = x + y	#表达式
print(z)	#输出语句
```

## 十四：获取用户输入

如何获取用户的输入，Python提供了内置函数input来简单实现此功能。例子如下：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> input('please input : ')
please input : 20190902
'20190902'
>>> x = input('x : ')
x : 12
>>> y = input('y : ')
y : 34
>>> print(int(x) * int(y))
408
>>> 
```

## 十五：赋值魔法

### 1. 序列解包

Python可同时（并行）给多个变量赋值

```powershell
>>> x,y,z = 1,2,3
>>> print(x,' ',y,' ',z)
1   2   3
```

使用这种方法还可以交换多个变量的值：

```powershell
>>> x,y = y,x
>>> print(x,y,z)
2 1 3
```

实际上，这里的操作称为**序列解包**（或**可迭代对象解包**）：将一个序列（或任意可迭代对象）解包，并将得到的值存储到一系列变量中：

```powershell
>>> values = 1,2,3
>>> values
(1, 2, 3)
>>> x,y,z = values
>>> print(x,y,z)
1 2 3
```

### 2. 链式赋值

链式赋值是一种快捷方式，用于将多个变量关联到一个值，有点像之前的并行赋值，但只涉及到一个值。

```powershell
#链式赋值
>>> a=b=2
#等价方式
>>> a=2
>>> a=b
#不等价方式
>>> a=2
>>> b=2
```

### 3.增强赋值

可以不编写代码**x = x  + 1**，而将右边表达式中的运算符**( + )**移到赋值运算符**( = )**的前面，从而写成**x += 1**，这样在python中被称为**增强赋值**，适用于所有标准运算符。

```powershell
>>> x = 2
>>> x += 1
>>> x *= 4
>>> x /= 2
>>> x
6.0
```

增强赋值同样适用于其他数据类型（只要使用的双目运算符可用于这些数据类型）

```powershell
>>> fnord = 'foo'
>>> fnord += ' bar'
>>> fnord *= 2
>>> fnord
'foo barfoo bar'
```

通过使用增强赋值，可让代码更紧凑，更简洁，同样在很多情况下可读性更强。

## 十六：运行Python脚本

### 1. 命令提示符运行Python脚本

在Window平台下打开DOS窗口输入如下命令可执行脚本：

```
C:\>python hello.py
```

在UNIX下通过打开shell输入如下命令可执行脚本：

```
$ python hello.py
```

在linux下命令也类似

```
$ python3 hello.py
```

### 2. 作为普通程序运行Python

在UNIX系统中，添加如下代码在Python脚本的第一行即可轻松运行脚本：

```
#！/usr/bin/env python
```

该语句从**#！**，后跟python的绝对路径，最后是指定对于脚本进行解释的工具。这样无论Python库在什么地方，这都可以让你能够像运行普通程序一样运行Python脚本。

要想像普通程序一样运行脚本，还必须将其变为可执行的：

```
$ chmod a+x hello.py
```

现在，可以像下面这样运行它（当前目录包含在执行路径中）：

```
$ hellp.py
```

如果以上命令不起作用，那么可以试试这个：

```
$ ./hello.py
```

当然，您也可以对文件进行重命名并删除扩展名**.py**,看起来会更像普通程序

在Windows中，扩展名**.py**是让Python脚本像普通程序一样运行的关键所在。如果当前计算机正确安装并配置了Python环境，点击hello.py文件会触发一个DOS窗口，如果没有输入项之类的逻辑，窗口会很快一闪而过，这是因为程序结束后窗口会立即关闭，为了防止这种情况，可在代码末尾添加1如下代码阻止关闭窗口：

```
input("Press <enter>")
```

