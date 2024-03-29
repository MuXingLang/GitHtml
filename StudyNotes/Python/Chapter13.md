# 第十三章：抽象和函数

​		本章节将介绍如何将语句组成函数，这让你能够告诉计算机如何完成任务，且只需要说一次，无需反复向计算机传达详细指令。

## 1. 懒惰是一种美德

​		前面编写的程序都很小，但如果编写大型程序，你很快就会遇到麻烦。例如：计算斐波那契数列

```powershell
>>> fibs = [0,1]
>>> for i in range(8):
	fibs.append(fibs[-2] + fibs[-1])

	
>>> fibs
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

​		如果你仅想计算前十个斐波那契额数列，则上述代码正好可以满足该需求，你甚至可以微调代码用来控制其处理数据的范围，即让用户指定最终要得到的序列的长度：

```
>>> fibs = [0,1]
>>> num = int(input("How many Fibonacci numbers do you want?"))
How many Fibonacci numbers do you want?12

>>> num
12
>>> for i in range(num-2):
	fibs.append(fibs[-2] + fibs[-1])

	
>>> fibs
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

​		然而真正的程序员并不会这样做。真正的程序员很懒，这里的懒并不是一个贬义词，而是指不做无谓的工作。那么真正的程序员会怎样做？让程序更抽象：

```python
num = int(input("How many Fibonacci numbers do you want?"))
print(fibs(num))
```

​		其中`fibs`就是一个函数，函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。

​		函数能提高应用的模块性，和代码的重复利用率。你已经知道Python提供了许多内建函数，比如`print()`。但你也可以自己创建函数，这被叫做用户自定义函数。

------

## 2. 自定义函数

​		抽象可以节省人力，但实际上还有更重要的优点：抽象时程序能够被人理解的关键所在。无论是编写程序还是阅读程序来说，这都至关重要。计算机通常喜欢具体而明确的指令而人通常不会是这样。

​		组织计算机程序时，你也可以采取类似的方式。程序应该非常的抽象，如下载网页，计算使用频率，打印每个单词的使用频率。下面将前述简单描述转换为一个Python程序：

```
page = download_page()
freqs = compute_frequencies(page)
for word,freq in freqs:
	print(word,freq)
```

​		看到这些代码，任何人都知道这个程序是做什么的。然而，至于具体该如何做，你未置一词。至于具体的操作细节，可以在其他地方（独立的函数定义）中给出。

​		函数执行特定的一系列操作并返回一个值，你可以通过函数名和若干参数（也可能美哟u参数）调用它。一般而言，要判断某个对象是否可被调用，可以使用内置函数`callable`。

```powershell
>>> import math
>>> x = 1
>>> y = math.sqrt
>>> callable(x)
False
>>> callable(y)
True
```

​		函数是结构化编的核心，你可以定义一个由自己想要功能的函数，以下是简单的规则：

- 函数代码块以 **def** 关键词开头，后接函数标识符名称和圆括号 **()**。
- 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。
- 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
- 函数内容以冒号起始，并且缩进。
- **return [表达式]** 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

  ​	Python 定义函数使用 `def` 关键字，一般格式如下：

```
def 函数名（参数列表）:
    函数体
```

​		默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。让我们使用函数来输出"Hello World！"：

```powershell
>>>def hello() :    
	print("Hello World!")     
    
    
>>> hello() 
Hello World! 
>>>
```

​		更复杂点的应用，函数中带上参数变量:

```python
#!/usr/bin/python3
 
# 计算面积函数
def area(width, height):
    return width * height
 
def print_welcome(name):
    print("Welcome", name)
 
print_welcome("Runoob")
w = 4
h = 5
print("width =", w, " height =", h, " area =", area(w, h))
```

​		以上实例输出结果：

```
Welcome Runoob
width = 4  height = 5  area = 20
```

------

## 3. 函数调用

​		定义一个函数：给了函数一个名称，指定了函数里包含的参数，和代码块结构。

​		这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接从 Python 命令提示符执行。

​		如下实例调用了 **printme()** 函数：

```python
#!/usr/bin/python3
 
# 定义函数
def printme( str ):
   # 打印任何传入的字符串
   print (str)
   return
 
# 调用函数
printme("我要调用用户自定义函数!")
printme("再次调用同一函数")
```

​		以上实例输出结果：

```
我要调用用户自定义函数!
再次调用同一函数
```

​		为了在协作开发中让他人更方便的调用函数，我们要给函数编写文档，已确保他人能够理解。可添加单行注释（即以#开头的内容），也可以添加独立字符串（即使用引号括起来的内容，包括单引号，双引号和三引号）。

​		在有些地方，如def语句之后，以及模块和类的开头等等，添加这样的字符串很有用。放在函数开头的字符串被称为文档字符串，将作为函数的一部分存储起来，可以通过函数的`__ doc __`属性或者特殊的内置函数`help`来获取有关函数的信息。

```powershell
>>> form math import sqrt
>>> sqrt.__doc__
'sqrt(x)\n\nReturn the square root of x.'
>>> help(sqrt)
Help on built-in function sqrt in module math:

sqrt(...)
    sqrt(x)
    
    Return the square root of x.
```

## 3. 参数传递

​		在 python 中，类型属于对象，变量是没有类型的：

```
a=[1,2,3]

a="Runoob"
```

​		以上代码中，**[1,2,3]** 是 List 类型，**"Runoob"** 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是指向 List 类型对象，也可以是指向 String 类型对象。

### 3.1 可更改(mutable)与不可更改(immutable)对象

​		在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。

- **不可变类型：**变量赋值 **a=5** 后再赋值 **a=10**，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。
- **可变类型：**变量赋值 **la=[1,2,3,4]** 后再赋值 **la[2]=5** 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

python 函数的参数传递：

- **不可变类型：**类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。
- **可变类型：**类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响

  ​	python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

#### 3.1.1 不可变对象实例

```python
#!/usr/bin/python3
 
def ChangeInt( a ):
    a = 10
 
b = 2
ChangeInt(b)
print( b ) # 结果是 2
```

​		实例中有 int 对象 2，指向它的变量是 b，在传递给 ChangeInt 函数时，按传值的方式复制了变量 b，a 和 b 都指向了同一个 Int 对象，在 a=10 时，则新生成一个 int 值对象 10，并让 a 指向它。

#### 3.1.2 可变对象实例

​		可变对象在函数里修改了参数，那么在调用这个函数的函数里，原始的参数也被改变了。例如：

```python
#!/usr/bin/python3
 
# 可写函数说明
def changeme( mylist ):
   "修改传入的列表"
   mylist.append([1,2,3,4])
   print ("函数内取值: ", mylist)
   return
 
# 调用changeme函数
mylist = [10,20,30]
changeme( mylist )
print ("函数外取值: ", mylist)
```

​		传入函数的和在末尾添加新内容的对象用的是同一个引用。故输出结果如下：

```
函数内取值:  [10, 20, 30, [1, 2, 3, 4]]
函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
```

------

## 4. 参数

​		以下是调用函数时可使用的正式参数类型：

- 必需参数
- 关键字参数
- 默认参数
- 不定长参数

### 4.1 必需参数

​		必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

​		调用 `printme()` 函数，你必须传入一个参数，不然会出现语法错误：

```python
#!/usr/bin/python3
 
#可写函数说明
def printme( str ):
   "打印任何传入的字符串"
   print (str)
   return
 
# 调用 printme 函数，不加参数会报错
printme()
```

​		以上实例输出结果：

```
Traceback (most recent call last):
  File "test.py", line 10, in <module>
    printme()
TypeError: printme() missing 1 required positional argument: 'str'
```

### 4.2 关键字参数

​		关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值有助于在调用参数时澄清各个参数的作用，虽然输入量更多了，但表达更明确更易阅读。

​		使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。

​		以下实例在函数 printme() 调用时使用参数名：

```python
#!/usr/bin/python3
 
#可写函数说明
def printme( str ):
   "打印任何传入的字符串"
   print (str)
   return
 
#调用printme函数
printme( str = "菜鸟教程")
```

​		以上实例输出结果： 

```
菜鸟教程
```

​		以下实例中演示了函数参数的使用不需要使用指定顺序：

```python
#!/usr/bin/python3
 
#可写函数说明
def printinfo( name, age ):
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return
 
#调用printinfo函数
printinfo( age=50, name="runoob" )
```

​		以上实例输出结果：

```
名字:  runoob
年龄:  50
```

​		更厉害的是，你可以结合位置参数和关键字参数，但必须先指定所有的位置参数，否则解释器可能无法知道它们究竟是哪个参数（即不知道参数对应对应什么位置，所以一般不建议同时使用位置参数和关键字参数）。

### 4.3 默认参数

​		调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 `age` 参数，则使用默认值：

```python
#!/usr/bin/python3
 
#可写函数说明
def printinfo( name, age = 35 ):
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return
 
#调用printinfo函数
printinfo( age=50, name="runoob" )
print ("------------------------")
printinfo( name="runoob" )
```

​		以上实例输出结果：

```
名字:  runoob
年龄:  50
------------------------
名字:  runoob
年龄:  35
```

### 4.4 不定长参数

​		你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述 2 种参数不同，声明时不会命名。基本语法如下：

```
def functionname([formal_args,] *var_args_tuple ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

​		加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。

```python
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vartuple)
 
# 调用printinfo 函数
printinfo( 70, 60, 50 )
```

​		以上实例输出结果：

```
输出: 
70
(60, 50)
```

​		如果在函数调用时没有指定参数，它就是一个空元组。我们也可以不向函数传递未命名的变量。如下实例：

```python
#!/usr/bin/python3
 
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   for var in vartuple:
      print (var)
   return
 
# 调用printinfo 函数
printinfo( 10 )
printinfo( 70, 60, 50 )
```

​		以上实例输出结果：

```
输出:
10
输出:
70
60
50
```

​		还有一种就是参数带两个星号 `**`基本语法如下：

```
def functionname([formal_args,] **var_args_dict ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

​		加了两个星号 `**` 的参数会以字典的形式导入。

```powershell
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, **vardict ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vardict)
 
# 调用printinfo 函数
printinfo(1, a=2,b=3)
```

​		以上实例输出结果：

```
输出: 
1
{'a': 2, 'b': 3}
```

​		声明函数时，参数中星号 `* `可以单独出现，例如:

```
def f(a,b,*,c):
    return a+b+c
```

​		如果单独出现星号 `*` 后的参数必须用关键字传入。

```
>>> def f(a,b,*,c):
...     return a+b+c
... 
>>> f(1,2,3)   # 报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() takes 2 positional arguments but 3 were given
>>> f(1,2,c=3) # 正常
6
>>>
```

------

## 5. 匿名函数

​		python 使用 `lambda` 来创建匿名函数。

​		所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。

- lambda 只是一个表达式，函数体比 def 简单很多。 
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。 
- lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
- 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。

  ​	`lambda` 函数的语法只包含一个语句，如下：

```
lambda [arg1 [,arg2,.....argn]]:expression
```

​		如下实例：

```python
#!/usr/bin/python3
 
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2
 
# 调用sum函数
print ("相加后的值为 : ", sum( 10, 20 ))
print ("相加后的值为 : ", sum( 20, 20 ))
```

​		以上实例输出结果：

```
相加后的值为 :  30
相加后的值为 :  40
```

------

## 6. return语句

​		**return [表达式]** 语句用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。之前的例子都没有示范如何返回数值，以下实例演示了 return 语句的用法：

```python
#!/usr/bin/python3
 
# 可写函数说明
def sum( arg1, arg2 ):
   # 返回2个参数的和."
   total = arg1 + arg2
   print ("函数内 : ", total)
   return total
 
# 调用sum函数
total = sum( 10, 20 )
print ("函数外 : ", total)
```

​		以上实例输出结果：

```
函数内 :  30
函数外 :  30
```

------

## 7.作用域

​		变量究竟是什么，可将其视为指向值的名称。因此，当执行赋值语句x=1之后，名称x指向值1.这看起来几乎和字典中的键值对完全一样，而之所以你无法察觉，只是你使用的是“看不见”的字典。实际上，这种解释已经离真相不再遥远了。Python中有一个名为vars的内置函数，它返回这个不可见字典：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> x = 1
>>> scope = vars()
>>> scope['x']
1
```

​		一般而言，不应该修改vars返回的字典，因为根据Python官方文档的说法，这样做的结果是不确定的。这种不确定的字典被称为命名空间或者作用域，除全局作用域外，每个函数调用都会创建一个作用域：

```powershell
>>> def foo(): x = 42

>>> x
1
>>> foo()
>>> x
1
```

​		以上例子中，函数foo重新定义了x的值，但当你再次查看时，它的值并没有发生改变。这是因为你在调用foo函数时，新的命名空间已经创建这个字典中值仅供当前foo函数使用，赋值语句在这个内部作用域（即局部变量）中执行是不会影响外部（全局作用域）的x。

​		那个既然两个同名变量互不影响，那么如何区分你调用了哪一个？这里推荐命名最好不要相同。怎样在函数中访问全局变量呢？如果名称不同，读取全局变量通常不会有问题，但还是存在出问题的可能性，比如之前的同名问题，因此，务必慎用全局变量。

## 8. 变量作用域

​		Python 中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。

​		变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。Python的作用域一共有4种，分别是：

- L （Local）   局部作用域
- E （Enclosing） 闭包函数外的函数中
- G （Global） 全局作用域
- B （Built-in） 内置作用域（内置函数所在模块的范围）

  ​	以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。

```
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

​		内置作用域是通过一个名为 builtin 的标准模块来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须导入这个文件才能够使用它。在Python3.0中，可以使用以下的代码来查看到底预定义了哪些变量:

```
>>> import builtins
>>> dir(builtins)
```

​		Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：

```
>>> if True:
...  msg = 'I am from Runoob'
... 
>>> msg
'I am from Runoob'
>>> 
```

​		实例中 msg 变量定义在 if 语句块中，但外部还是可以访问的。

​		如果将 msg 定义在函数中，则它就是局部变量，外部不能访问：

```
>>> def test():
...     msg_inner = 'I am from Runoob'
... 
>>> msg_inner
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'msg_inner' is not defined
>>> 
```

​		从报错的信息上看，说明了 msg_inner 未定义，无法使用，因为它是局部变量，只有在函数内可以使用。

------

### 7.1 全局变量和局部变量

​		定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

​		局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```python
#!/usr/bin/python3
 
total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```

​		以上实例输出结果：

```
函数内是局部变量 :  30
函数外是全局变量 :  0
```

### 7.2 global 和 nonlocal关键字 

​		当内部作用域想修改外部作用域的变量时，就要用到`global`和`nonlocal`关键字了。

​		以下实例修改全局变量 `num`：

```powershell
#!/usr/bin/python3
 
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)
```

​		以上实例输出结果：

```
1
123
123
```

​		如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 `nonlocal` 关键字了，如下实例：

```powershell
#!/usr/bin/python3
 
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()
```

​		以上实例输出结果：

```
100
100
```

​		另外有一种特殊情况，假设下面这段代码被运行：

```
#!/usr/bin/python3
 
a = 10
def test():
    a = a + 1
    print(a)
test()
```

​		以上程序执行，报错信息如下：

```
Traceback (most recent call last):
  File "test.py", line 7, in <module>
    test()
  File "test.py", line 5, in test
    a = a + 1
UnboundLocalError: local variable 'a' referenced before assignment
```

​		错误信息为局部作用域引用错误，因为 test 函数中的 a 使用的是局部，未定义，无法修改。

​		修改 a 为全局变量，通过函数参数传递，可以正常执行输出结果为：

```
#!/usr/bin/python3
 
a = 10
def test(a):
    a = a + 1
    print(a)
test(a)
```

​		执行输出结果为：

```
11
```

## 9.  递归

​		前面主要介绍了如何创建和调用函数。我们知道，函数可以调用另一个函数，然而实际上，函数还可以调用自己。如果你以前没有遇到这种情况，可能会迷惑递归是什么意思？简单而言，递归意味着引用（这里指调用）自身。以下是一个递归式函数的定义：

```python
def recursion():
	pass
	return recursion()
```

​		这个定义显然，什么都没有做，只是不停的调用自身，就像一个死循环一样，你将发现运行一段时间后，这个程序可能会崩溃（引发异常）。每次调用函数，都会消耗一些内存当函数的调用到达一定次数，将消耗所有可用的额内存空间，导致程序被终止并显示错误消息“超过最大递归深度”。

​		这个函数中的递归又被称之为**无穷递归**，因为它理论上说永远不会结束。程序员想要的是有用的递归函数（不是空耗内存），这样的递归函数通常包含以下两部分：

- 基线条件（针对最小的问题）：满足某种条件时函数会直接返回一个值。
- 递归条件：包含一个或多个调用，这些调用旨在解决问题的一部分。

  ​	递归解决问题的关键是将问题分解成较小的部分。

### 9.1 阶乘和幂

​		本节讨论两个经典的递归函数。首先，假设你要计算数字n的阶乘，比如你要计算将n个人排成一队共有多少种方式，可使用循环来实现：

```python
def factorial(n):
	result = n
	for i in range(1,n):
		result *= i
	return reault
```

​		这种实现方式无疑是可行的，而且直截了当。大致而言，程序的流程是这样的：

1. 先将result的值设置为n;
2. 再将result依次乘以1到n-1之间的每一个数字；
3. 最后返回result。

   ​	在数学方面，阶乘的定义可表述如下：

4. 1的阶乘等于1
5. 对于大于1的数字n，其阶乘为n-1的阶乘再乘以n

   ​	那么，我们可以定义如下的函数：

```python
def factorial(n):
    if(n == 1):
        return 1
    else:
        return factorial(n-1) * n

print(factorial(5))
```

​		再来看一个实例，假设你要计算幂，就像内置函数pow和运算符**所作的那样定义一个power：

```python
def power(x,n):
    if(n = 0):
    	return 1
    else:
    	result = 1
    	for i in range(n):
    		result *= x
    	return result
```

​		这是一个非常简单的小型函数，但依然可以将其改为递归式的

- 对于任何数字x,power（x,1）都为1
- 当n>0时，power(x,n)为power(x,n-1)与x的乘积

```python
def power(x,n):
    if(n = 0):
    	return 1
    else:
    	return power(x,n-1) * x 
```

​		那么使用递归有什么意义呢？同样的效果使用循环也可以实现。而且在大多数情况下，使用循环的效率可能更高。但是，在很多时候，使用递归的可阅读性更高，在你理解函数的递归式定义尤其如此。另外，虽然你完全可以避免使用递归机型编程，但作为程序员必须能够读懂他人编写的递归算法和函数。

### 9.2 二分查找

首先分解以下二分查找的实现：

- 如果查找上限和下限相同，则其均指向查找元素，直接返回该元素
- 如果上下限并不一致，则取上下限平均值的中间值，再确认查找元素时在上半部分还是下半部分，然后再继续在新的范围内查找

对于数字的二分查找，需要先排序，以下为一个二分查找数字的简单例子：

```python
def search(squence, number, lower, upper):
	if(lower == upper):
		assert number = squence[upper]
		return upper
	else:
		middle = (lower + upper)/2
		if(number > middle):
			return search(squence, number, middle, upper)
		else:
			return search(squence, number, lower, middle)
```

实际上，python中bisect模块提供了标准的二分查找实现。