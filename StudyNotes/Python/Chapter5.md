# 第五章：字符串

## 1. 概述

字符串是 Python 中最常用的数据类型。我们可以使用引号( ' 或 " )来创建字符串。

创建字符串很简单，只要为变量分配一个值即可。例如：

var1 = 'Hello World!' var2 = "Runoob"

## 2. 访问字符串中的值

Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用。

Python 访问子字符串，可以使用方括号来截取字符串，如下实例：

```python
#!/usr/bin/python3
 
var1 = 'Hello World!'
var2 = "Runoob"
 
print ("var1[0]: ", var1[0])
print ("var2[1:5]: ", var2[1:5])
```

以上实例执行结果：

```
var1[0]:  H
var2[1:5]:  unoo
```

## 3.  字符串更新

你可以截取字符串的一部分并与其他字段拼接，如下实例：

```
#!/usr/bin/python3   

var1 = 'Hello World!'   
print ("已更新字符串 : ", var1[:6] + 'Runoob!')
```

以上实例执行结果

```
已更新字符串 :  Hello Runoob!
```

## 4. 转义字符和原始字符串

在需要在字符中使用特殊字符时，python用反斜杠(\)转义字符。如下表：

| 转义字符    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| \(在行尾时) | 续行符                                                       |
| \\          | 反斜杠符号                                                   |
| \'          | 单引号                                                       |
| \"          | 双引号                                                       |
| \a          | 响铃                                                         |
| \b          | 退格(Backspace)                                              |
| \000        | 空                                                           |
| \n          | 换行                                                         |
| \v          | 纵向制表符                                                   |
| \t          | 横向制表符                                                   |
| \r          | 回车                                                         |
| \f          | 换页                                                         |
| \oyy        | 八进制数，**yy** 代表的字符，例如：**\o12** 代表换行，其中 o 是字母，不是数字 0。 |
| \xyy        | 十六进制数，yy代表的字符，例如：\x0a代表换行                 |
| \other      | 其它的字符以普通格式输出                                     |

如果在字符串前加一个字母r即表示**原始字符串**，表示遇到转义符不进行转义

```python
str = r'C:/Users'
```

如你所见，原始字符串用前缀r表示，看起来原始字符串包含任何字符，但仍存在例外，引号仍需要通过转义符转义，而且用于转义的转义符反斜杠因为前缀r的存在，也会显示在最终的字符串中。

```powershell
>>> print(r'let\'s go!')
let\'s go!
>>> 
```

另外，原始字符串不能以单个反斜杠结尾。换而言之，原始字符串的最后一个字符不能是反斜杠，除非你对其进行转义，（而如果进行转义，用于转义的反斜杠也会出现在最终的字符串中），为了解决此类问题，存在多种办法，但基本技巧都是将反斜杠单独作为一个字符串：

```powershell
>>> print(r'c;\Users\Public\Desktop' '\\')
c;\Users\Public\Desktop\
>>> 
```



## 5. 字符串运算符

下表实例变量a值为字符串 "Hello"，b变量值为 "Python"：

| 操作符 | 描述                                                         | 实例                            |
| ------ | ------------------------------------------------------------ | ------------------------------- |
| +      | 字符串连接                                                   | a + b 输出结果： HelloPython    |
| *      | 重复输出字符串                                               | a*2 输出结果：HelloHello        |
| []     | 通过索引获取字符串中字符                                     | a[1] 输出结果 **e**             |
| [ : ]  | 截取字符串中的一部分，遵循**左闭右开**原则，str[0,2] 是不包含第 3 个字符的。 | a[1:4] 输出结果 **ell**         |
| in     | 成员运算符 - 如果字符串中包含给定的字符返回 True             | **'H' in a** 输出结果 True      |
| not in | 成员运算符 - 如果字符串中不包含给定的字符返回 True           | **'M' not in a** 输出结果 True  |
| r/R    | 原始字符串 - 原始字符串：所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符。 原始字符串除在字符串的第一个引号前加上字母 r（可以大小写）以外，与普通字符串有着几乎完全相同的语法。 | `print( r'\n' ) print( R'\n' )` |
| %      | 格式字符串                                                   | 请看下一节内容。                |

```python
#!/usr/bin/python3
 
a = "Hello"
b = "Python"
 
print("a + b 输出结果：", a + b)
print("a * 2 输出结果：", a * 2)
print("a[1] 输出结果：", a[1])
print("a[1:4] 输出结果：", a[1:4])
 
if( "H" in a) :
    print("H 在变量 a 中")
else :
    print("H 不在变量 a 中")
 
if( "M" not in a) :
    print("M 不在变量 a 中")
else :
    print("M 在变量 a 中")
 
print (r'\n')
print (R'\n')
```

以上实例输出结果为：

```
a + b 输出结果： HelloPython
a * 2 输出结果： HelloHello
a[1] 输出结果： e
a[1:4] 输出结果： ell
H 在变量 a 中
M 不在变量 a 中
\n
\n
```

## 6. 字符串格式化

### 6.1 格式化概述

如何设置字符串的格式？

Python 支持格式化字符串的输出 。尽管这样可能会用到非常复杂的表达式，但基本方案都是使用字符串格式设置运算符：百分号

1. 通过转换说明符%s,指明要将值插入什么位置，其中s代表将其视为字符串进行格式设置，如果指定的值不是字符串，则可使用str将其转为字符串。
2. 使用模板字符串，这里不做详解。

在 Python 中，字符串格式化使用与 C 中 sprintf 函数一样的语法。

```python
#!/usr/bin/python3

print ("我叫 %s 今年 %d 岁!" % ('小明', 10))
```

以上实例输出结果：

```
我叫 小明 今年 10 岁!
```

详细例子：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> format = 'hello, %s. %s enough for ya?'
>>> values = ('world','Hot')
>>> format % values
'hello, world. Hot enough for ya?'
>>> from string import Template	#模板字符串
>>> tem1 = Template('Hello, $who! $what enough for ya?')
>>> tem1.substitute(who='Mars',what='Dusty')
'Hello, Mars! Dusty enough for ya?'
>>> from math import pi
>>> "{name} is approximately {value}.".format(value=pi,name='Π')
'Π is approximately 3.141592653589793.'
>>> "{name} is approximately {value:2f}.".format(value=pi,name='Π')
'Π is approximately 3.141593.'
>>> "{name} is approximately {value:.2f}.".format(value=pi,name='Π')
'Π is approximately 3.14.'
>>> from math import e
>>> "Euler's constant is rough {e}.".format(e=e)
"Euler's constant is rough 2.718281828459045."
>>> "{foo} {} {bar} {} ".format(1,2,bar=4,foo=3)
'3 1 4 2 '
>>>  "{foo} {1} {bar} {0} ".format(1,2,bar=4,foo=3)
#多了一个空格 
SyntaxError: unexpected indent
>>> "{foo} {1} {bar} {0} ".format(1,2,bar=4,foo=3)
'3 2 4 1 '
>>> 
```

python字符串格式化符号:

| 符   号 | 描述                                 |
| ------- | ------------------------------------ |
| %c      | 格式化字符及其ASCII码                |
| %s      | 格式化字符串                         |
| %d      | 格式化整数                           |
| %u      | 格式化无符号整型                     |
| %o      | 格式化无符号八进制数                 |
| %x      | 格式化无符号十六进制数               |
| %X      | 格式化无符号十六进制数（大写）       |
| %f      | 格式化浮点数字，可指定小数点后的精度 |
| %e      | 用科学计数法格式化浮点数             |
| %E      | 作用同%e，用科学计数法格式化浮点数   |
| %g      | %f和%e的简写                         |
| %G      | %f 和 %E 的简写                      |
| %p      | 用十六进制数格式化变量的地址         |

格式化操作符辅助指令:

| 符号  | 功能                                                         |
| ----- | ------------------------------------------------------------ |
| *     | 定义宽度或者小数点精度                                       |
| -     | 用做左对齐                                                   |
| +     | 在正数前面显示加号( + )                                      |
| <sp>  | 在正数前面显示空格                                           |
| #     | 在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X') |
| 0     | 显示的数字前面填充'0'而不是默认的空格                        |
| %     | '%%'输出一个单一的'%'                                        |
| (var) | 映射变量(字典参数)                                           |
| m.n.  | m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话)        |

Python2.6 开始，新增了一种格式化字符串的函数 str.format()，它增强了字符串格式化的功能。

### 6. 2 基本转换

在格式字符串中，重头戏之一是替换字段。替换字段如下部分组成：

- 字段名：索引或标识符，指出要设置哪个值的格式并使用结果来替换该字段。除指定值外，还可指定值的特定部分，如列表的元素。

- 转换标志：跟在叹号后边的单个字符。当前支持的字符包括r(表示repr),s(表示str)和a(表示ascii)。如果你指定了转换标志，将不使用对象本身的格式设置机制，而是使用指定的函数将对象转换为字符串，再做进一步的格式设置。

- 格式说明符：跟在冒号后边的表达式，详细制定最终的格式，包括格式类型（如字符串，浮点数，十六进制数等等），字段宽度，数的精度，如何显示符号和千位分隔符，以及各种对齐和填充方式等等。

在最简单的情况下，只需向format提供设置其格式的未命名参数，并在格式字符串中使用未命名字段。此时，将按照顺序将字段和参数匹配：

```powershell
>>> '{},{} and {}'.format("first",'second','third')
'first,second and third'
```

可以通过索引指定要在哪个字段中使用相应的未命名参数，这样可以不按顺序使用未命名参数：

```powershell
>>> '{0},{2} and {1}'.format("first","third","second")
'first,second and third'
```

指定要在字段中包含的值后，就可添加有关如何设置其格式的指令。首先，需要设置一个转换标志：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> print("{pi!s} {pi!r} {pi!a} ".format(pi='π'))
π 'π' '\u03c0' 	# str repr ascii
>>> num = 42
>>> "the number is {num}.".format(num=num)
'the number is 42.'			#默认十进制整数
>>> "the number is {num:f}.".format(num=num)
'the number is 42.000000.'	#浮点数
>>> "the number is {num:b}.".format(num=num)
'the number is 101010.'		#二进制
```

您还可以指定要转换的值是哪种类型，更准确的说，是要将其视为那种类型。例如，你可能要提供一个整数，但要将其作为小数处理，为此可在格式说明后使用字符f表示浮点数，完整的类型说明符如下：

| 类型 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| b    | 将整数表示为二进制数                                         |
| c    | 将整数解读为Unicode码点                                      |
| d    | 将整数视为十进制数进行处理，这是整数默认使用的说明符         |
| e    | 使用科学表示法来表示小数（用e来表示整数）                    |
| E    | 与e相同，用E来表示指数                                       |
| f    | 将小数表示为定点数                                           |
| F    | 与f相同，但对于特殊值（nan和inf）,使用大写表示               |
| g    | 自动在顶点表示法和科学表示法之间做出选择。这是默认用于小数的说明符，但在默认情况下至少由一位小数 |
| G    | 与g相同，但使用大写来表示指数和特殊值                        |
| n    | 与g相同，但插入随区域而异的数字分隔符                        |
| o    | 将整数表示为八进制数                                         |
| s    | 保持字符串的格式不变，这是默认用于字符串的说明符             |
| x    | 将整数表示为十六进制数并使用小写字母                         |
| X    | 与x相同，但使用大写字母                                      |
| %    | 将数表示为百分比值（乘以100，按说明符设置格式，再在后边加%） |

### 6.3 宽度，精度和千位分隔符

设置浮点数（或其他更具体的小数类型）的格式时，默认在小数点后显示6位小数，并根据要设置字段的宽度，而不进行任何形式的填充。

宽度是使用整数指定的：

```powershell
>>> "the number is {num:10}.".format(num=num)
'the number is         42.'	#数字右对齐
>>> "the name is {name:10}.".format(name='Bob')
'the name is Bob       .'	#字符串左对齐
```

精度也是使用整数指定的，但需要在它前面加上一个表示小数点的句点.

```powershell
>>> from math import pi		#pi常量需导入
>>> 'Pi value is {pi:.7f}.'.format(pi=pi)
'Pi value is 3.1415927.'
>>> 
```

实际上，对于其他类型也可以指定精度，一般不常见：

```powershell
>>> '{:.7}'.format("I love python!")
'I love '
>>>
```

最后，可以使用逗号来指出你要添加的**千位分隔符**：

```powershell
>>> 'One googol is {:,}.'.format(10**100)
'One googol is 10,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000.'
>>> 
```

### 6.4 符号，对齐和用0填充

有很多用于设置字符格式的机制，比如便于打印整齐的表格。在大多数情况下，只需指定宽度和精度。但是如果包含负数，原本漂亮的输出就没那么完美了。而且字符串和数字默认的对齐方式不同，如果同时包含字符串和数时，怎么应对这种不同的默认对齐方式。这里介绍一下填充。在指定宽度和精度的数前面，可添加一个标志。这个标志可以是零，加号，减号，空格，其中零即使用0来填充数字：

```powershell
>>> '{:010.2f}'.format(pi)
'0000003.14'
```

要指定对齐方式，可使用左尖括号（<）左对齐，上尖括号（^）居中，右尖括号（>）右对齐：

```powershell
>>> print('{0:<10.2f}\n{0:^10.2f}\n{0:>10.2f}'.format(pi))
3.14      
   3.14   
      3.14
```

可以使用填充字符来扩充对齐说明符，可使用指定的字符串而不是默认的空格填充：

```powershell
>>> '{:$^15}'.format(" WIN BIG ")
'$$$ WIN BIG $$$'
```

还有更具体的说明符**＝**，可指定填充字符放在符号和数字之间：

```powershell
>>> print('{0:10.2f}\n{1:10.2f}'.format(pi,-pi))
      3.14
     -3.14
>>> print('{0:10.2f}\n{1:=10.2f}'.format(pi,-pi))
      3.14
-     3.14
```

如果要给正数加上符号，可在对齐说明符后添加说明符**+**。如果将符号说明符指定为空格，会在证书前面加上空格而不是**+**：

```powershell
>>> print('{0:-.2}\n{1:-.2}'.format(pi,-pi))
3.1
-3.1
>>> print('{0:+.2}\n{1:+.2}'.format(pi,-pi))
+3.1
-3.1
>>> print('{0: .2}\n{1: .2}'.format(pi,-pi))
 3.1
-3.1
```

最后一个要介绍的是井号（#），它可以被放置在说明符和宽度之间(如果存在这两个设置)，该选项会触发另一种转换方式，转换细节随类型而异：

```powershell
>>> '{:b}'.format(42)
'101010'
>>> '{:#b}'.format(42)
'0b101010'
>>> '{:g}'.format(42)
'42'
>>> '{:#g}'.format(42)
'42.0000'
```

## 7. 长字符串

python三引号允许一个字符串跨多行，字符串中可以包含换行符、制表符以及其他特殊字符。实例如下

```python
#!/usr/bin/python3   

para_str = """这是一个多行字符串的实例 
多行字符串可以使用制表符 
TAB ( \t )。
也可以使用换行符 [ \n ]。 """ 

print (para_str)
```

以上实例执行结果为：

```
这是一个多行字符串的实例
多行字符串可以使用制表符
TAB (    )。
也可以使用换行符 [ 
 ]。
```

​		三引号让程序员从引号和特殊字符串的泥潭里面解脱出来，自始至终保持一小块字符串的格式是所谓的WYSIWYG（所见即所得）格式的。

一个典型的用例是，当你需要一块HTML或者SQL时，这时用字符串组合，特殊字符串转义将会非常的繁琐。

```
errHTML = '''
<HTML><HEAD><TITLE>
Friends CGI Demo</TITLE></HEAD>
<BODY><H3>ERROR</H3>
<B>%s</B><P>
<FORM><INPUT TYPE=button VALUE=Back
ONCLICK="window.history.back()"></FORM>
</BODY></HTML>
'''
cursor.execute('''
CREATE TABLE users (  
login VARCHAR(8), 
uid INTEGER,
prid INTEGER)
''')
```

**Tips:**常规字符串也可横跨多行。不过需要在行尾加上反斜杠，反斜杠和换行符会被转义，即忽略，例子如下：

```
#正常的打印
print('hello,\
word!')
#合法表达式
>>> 1+2+\
      3+4
10
#合法语句
>>> print\
       ('hello,world!')
hello,world!
>>> 
```

## 8. Unicode 字符串

​		在Python2中，普通字符串是以8位ASCII码进行存储的，而Unicode字符串则存储为16位unicode字符串，这样能够表示更多的字符集。使用的语法是在字符串前面加上前缀 **u**。 

​		在Python3中，所有的字符串都是Unicode字符串。 

------

## 9. 字符串内建函数

Python 的字符串常用内建函数如下：

| 序号 | 方法                                                        | 描述                                                         |
| ---- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| 1    | capitalize()                                                | 将字符串的第一个字符转换为大写。                             |
| 2    | center(width, fillchar)                                     | 返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格。 |
| 3    | count(str, beg= 0,end=len(string))                          | 返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数。 |
| 4    | bytes.decode(encoding="utf-8", errors="strict")             | Python3 中没有 decode 方法，但我们可以使用 bytes 对象的 decode() 方法来解码给定的 bytes 对象，这个 bytes 对象可以由 str.encode() 来编码返回。 |
| 5    | encode(encoding='UTF-8',errors='strict')                    | 以 encoding 指定的编码格式编码字符串，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace'。 |
| 6    | endswith(suffix, beg=0, end=len(string))                    | 检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False。 |
| 7    | expandtabs(tabsize=8)                                       | 把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 。 |
| 8    | find(str, beg=0, end=len(string))                           | 检测 str 是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1。 |
| 9    | index(str, beg=0, end=len(string))                          | 跟find()方法一样，只不过如果str不在字符串中会报一个异常。    |
| 10   | isalnum()                                                   | 如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True,否则返回 False。 |
| 11   | isalpha()                                                   | 如果字符串至少有一个字符并且所有字符都是字母则返回 True, 否则返回 False。 |
| 12   | isdigit()                                                   | 如果字符串只包含数字则返回 True 否则返回 False。             |
| 13   | islower()                                                   | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False。 |
| 14   | isnumeric()                                                 | 如果字符串中只包含数字字符，则返回 True，否则返回 False      |
| 15   | isspace()                                                   | 如果字符串中只包含空白，则返回 True，否则返回 False。        |
| 16   | istitle()                                                   | 如果字符串是标题化的(见 title())则返回 True，否则返回 False。 |
| 17   | isupper()                                                   | 如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False。 |
| 18   | join(seq)                                                   | 以指定字符串作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串。 |
| 19   | len(string)                                                 | 返回字符串长度                                               |
| 20   | ljust(width[, fillchar\])                                   | 返回一个原字符串左对齐,并使用 fillchar 填充至长度 width 的新字符串，fillchar 默认为空格。 |
| 21   | lower()                                                     | 转换字符串中所有大写字符为小写.                              |
| 22   | lstrip()                                                    | 截掉字符串左边的空格或指定字符。                             |
| 23   | maketrans()                                                 | 创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。 |
| 24   | max(str)                                                    | 返回字符串 str 中最大的字母。                                |
| 25   | min(str)                                                    | 返回字符串 str 中最小的字母。                                |
| 26   | replace(old, new [, max\])                                  | 把 将字符串中的 str1 替换成 str2,如果 max 指定，则替换不超过 max 次。 |
| 27   | rfind(str, beg=0,end=len(string))                           | 类似于 find()函数，不过是从右边开始查找。                    |
| 28   | rindex( str, beg=0, end=len(string))                        | 类似于 index()，不过是从右边开始。                           |
| 29   | rjust(width,[, fillchar\])                                  | 返回一个原字符串右对齐,并使用fillchar(默认空格）填充至长度 width 的新字符串。 |
| 30   | [rstrip()                                                   | 删除字符串字符串末尾的空格。                                 |
| 31   | split(str="", num=string.count(str)) num=string.count(str)) | 以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num+1 个子字符串。 |
| 32   | splitlines([keepends\])                                     | 按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。 |
| 33   | startswith(substr, beg=0,end=len(string))                   | 检查字符串是否是以指定子字符串 substr 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查。 |
| 34   | strip([chars\])                                             | 在字符串上执行 lstrip()和 rstrip()。                         |
| 35   | swapcase() 将字符串中大写转换为小写，小写转换为大写         | 返回"标题化"的字符串,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle())。 |
| 36   | title()                                                     | 根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 deletechars 参数中。 |
| 37   | translate(table, deletechars="")                            |                                                              |
| 38   | upper()                                                     | 转换字符串中的小写字母为大写                                 |
| 39   | zfill (width)                                               | 返回长度为 width 的字符串，原字符串右对齐，前面填充0         |
| 40   | isdecimal()                                                 | 检查字符串是否只包含十进制字符，如果是返回 true，否则返回 false。 |