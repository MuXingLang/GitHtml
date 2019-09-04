# 第十一章：循环语句


本章节将为大家介绍Python循环语句的使用。

Python中的循环语句有 for 和 while。

## 1. while 循环 

Python中while语句的一般形式：

```
while 判断条件：
    语句
```

执行 Gif 演示：

![img](../images/Python/006faQNTgw1f5wnm06h3ug30ci08cake.gif)

同样需要注意冒号和缩进。另外，在 Python 中没有 do..while 循环。 

以下实例使用了 while 来计算 1 到 100 的总和：

```python
#!/usr/bin/env python3
 
n = 100
 
sum = 0
counter = 1
while counter <= n:
    sum = sum + counter
    counter += 1
 
print("1 到 %d 之和为: %d" % (n,sum))
```

执行结果如下：

```
1 到 100 之和为: 5050
```

## 2. 无限循环

我们可以通过设置条件表达式永远不为 false 来实现无限循环，实例如下：

```python
#!/usr/bin/python3
 
var = 1
while var == 1 :  # 表达式永远为 true
   num = int(input("输入一个数字  :"))
   print ("你输入的数字是: ", num)
 
print ("Good bye!")
```

执行以上脚本，输出结果如下：

```
输入一个数字  :5
你输入的数字是:  5
输入一个数字  :
```

你可以使用  **CTRL+C** 来退出当前的无限循环。

无限循环在服务器上客户端的实时请求非常有用。

## 3. while 中使用 else 语句

在 while … else 在条件语句为 false 时执行 else 的语句块：

```python
#!/usr/bin/python3
 
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```

执行以上脚本，输出结果如下：

```
0  小于 5
1  小于 5
2  小于 5
3  小于 5
4  小于 5
5  大于或等于 5
```

## 4. 简单语句组

类似if语句的语法，如果你的while循环体中只有一条语句，你可以将该语句与while写在同一行中， 如下所示：

```python
#!/usr/bin/python   

flag = 1   

while (flag): 
	print ('欢迎访问菜鸟教程!')   
print ("Good bye!")
```

**注意：**以上的无限循环你可以使用 CTRL+C 来中断循环。

执行以上脚本，输出结果如下(注意可能造成计算机卡死)：

```
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
……
```

------

## 5. for 语句

Python for循环可以遍历任何序列的项目，如一个列表或者一个字符串。

for循环的一般格式如下：

```
for <variable> in <sequence>:     
	<statements> 
else:     
	<statements>
```

Python loop循环实例：

```powershell
>>>languages = ["C", "C++", "Perl", "Python"]  
>>> for x in languages: 
     print (x)
C 
C++ 
Perl 
Python 
>>>
```

以下 for 实例中使用了 break 语句，break 语句用于跳出当前循环体：

```python
#!/usr/bin/python3   

sites = ["Baidu", "Google","Runoob","Taobao"] 
for site in sites:     
	if site == "Runoob":         
		print("菜鸟教程!")         
		break     
	print("循环数据 " + site) 
else:     
	print("没有循环数据!") 
print("完成循环!")
```

执行脚本后，在循环到 "Runoob"时会跳出循环体：

```
循环数据 Baidu
循环数据 Google
菜鸟教程!
完成循环!
```

------

## 6. range()函数

​		基本上，可迭代对象是可使用for循环进行遍历的对象。而基于迭代（也就是遍历）特定范围内的数是一种常见任务，python提供有一个创建范围的内置函数range。

如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列，例如: 

```powershell
>>>for i in range(5): 
     print(i)  
0 
1 
2 
3 
4
```

你也可以使用range指定区间的值：

```powershell
>>>for i in range(5,9) :     
	print(i)        
5 
6 
7 
8 
>>>
```

也可以使range以指定数字开始并指定不同的增量(甚至可以是负数，有时这也叫做'步长'):  

```powershell
>>>for i in range(0, 10, 3) :     
	print(i)        
0 
3 
6 
9 
>>>
```

负数：

```powershell
>>>for i in range(-10, -100, -30) :     
	print(i)        
-10 
-40 
-70 
>>>
```

您可以结合range()和len()函数以遍历一个序列的索引,如下所示:

```powershell
>>>a = ['Google', 'Baidu', 'Runoob', 'Taobao', 'QQ'] 
>>> for i in range(len(a)):      
	print(i, a[i])   
0 Google 
1 Baidu 
2 Runoob 
3 Taobao 
4 QQ 
>>>
```

还可以使用range()函数来创建一个列表：

```powershell
>>>list(range(5)) 
[0, 1, 2, 3, 4] 
>>>
```

## 7. 跳出循环 

​		break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。 实例如下： 

```python
#!/usr/bin/python3
 
for letter in 'Runoob':     # 第一个实例
   if letter == 'b':
      break
   print ('当前字母为 :', letter)
  
var = 10                    # 第二个实例
while var > 0:              
   print ('当期变量值为 :', var)
   var = var -1
   if var == 5:
      break
 
print ("Good bye!")
```

执行以上脚本输出结果为：

```
当前字母为 : R
当前字母为 : u
当前字母为 : n
当前字母为 : o
当前字母为 : o
当期变量值为 : 10
当期变量值为 : 9
当期变量值为 : 8
当期变量值为 : 7
当期变量值为 : 6
Good bye!
```

continue语句被用来告诉Python跳过当前循环块中的剩余语句，然后继续进行下一轮循环。 

```python
#!/usr/bin/python3
 
for letter in 'Runoob':     # 第一个实例
   if letter == 'o':        # 字母为 o 时跳过输出
      continue
   print ('当前字母 :', letter)
 
var = 10                    # 第二个实例
while var > 0:              
   var = var -1
   if var == 5:             # 变量为 5 时跳过输出
      continue
   print ('当前变量值 :', var)
print ("Good bye!")
```

执行以上脚本输出结果为：

```
当前字母 : R
当前字母 : u
当前字母 : n
当前字母 : b
当前变量值 : 9
当前变量值 : 8
当前变量值 : 7
当前变量值 : 6
当前变量值 : 4
当前变量值 : 3
当前变量值 : 2
当前变量值 : 1
当前变量值 : 0
Good bye!
```

​		循环语句可以有 else 子句，它在穷尽列表(以for循环)或条件变为 false (以while循环)导致循环终止时被执行,但循环被break终止时不执行。 

如下实例用于查询质数的循环例子:

```python
#!/usr/bin/python3
 
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, '等于', x, '*', n//x)
            break
    else:
        # 循环中没有找到元素
        print(n, ' 是质数')
```

执行以上脚本输出结果为：

```
2  是质数
3  是质数
4 等于 2 * 2
5  是质数
6 等于 2 * 3
7  是质数
8 等于 2 * 4
9 等于 3 * 3
```

## 8. 迭代

### 8.1 迭代字典

要遍历字典的所有关键字，可像遍历序列那样使用普通的for语句。

```
>>> d = {'x':1,'y':2,'z':3}
>>> for key in d:
	print(key,' corresponds to ',d[key])

	
x  corresponds to  1
y  corresponds to  2
z  corresponds to  3
```

​		也可以使用keys等字典方法来获取所有的键。如果只对值感兴趣，可使用d.values。你可能还记得，d.items以元组的形式返回键值对。for循环的优点之一，是可在其中使用序列解包。

### 8.2 迭代工具

Python提供了很多可帮助迭代序列（或其他可迭代对象）的函数，这里仅作简单介绍：

#### 8.2.1 并行迭代

有时候，你可能想同时迭代两个序列。假设有两个列表如下：

```
name = ['anne','beth','george','damon']
ages = [12,45,32,102]
```

如何打印对应的名称和年龄，目前能做到的方法如下所示：

```powershell
>>> for i in range(len(name)):
	print(i, name[i], 'is', ages[i], 'years old.')

	
0 anne is 12 years old.
1 beth is 45 years old.
2 george is 32 years old.
3 damon is 102 years old.
```

​		其中i是用于循环索引的变量的标准名称。Python里有一个很有意思的并行迭代工具是内置函数zip，可以将两个序列“缝合”起来，并返回一个由元组组成的序列。返回值是一个适合迭代的对象，如果要查看其内容，可使用list将其转换为列表：

```powershell
>>> list(zip(name,ages))
[('anne', 12), ('beth', 45), ('george', 32), ('damon', 102)]
```

而缝合之后，可在循环中将元组解包：

```powershell
>>> for name,age in zip(name,ages):
	print(name, 'is', age, 'years old!')

	
anne is 12 years old!
beth is 45 years old!
george is 32 years old!
damon is 102 years old!
```

​		函数zip可以用于“缝合”任意数量的序列。需要注意的是，当序列的长度不一致时，函数会将最短的序列用完后停止“缝合”，即木桶效应。

#### 8.2.2 迭代时获取索引

​		有些情况下，你可能需要在迭代对象序列时同时获取当前对象的索引。例如，你可能想替换一个字符串列表里所有包含字串"XXX"的字符串：

```python
for string in strings:
	if('XXX' in string):
		index = strings.index(string)	#在字符串列表中查找字符串
		strings[index] = '[censored]'
```

​		这种方法是可行的，但替换前的搜索好像没有必要。而且，如果没有替换，则搜索返回的索引可能不对，即返回的是该字符串首次出现处的索引，以下有更好的解决办法：

```python
index = 0
for string in strings:
	if('xxx' in string):
		strings[index] = '[censored]'
	index += 1
```

这种解决方案虽然可以接受，但看起来也有点笨拙，还有一种解决方案是使用内置函数enumerate：

```python
for index,string in enumerate(strings):
	if('xxx' in string):
		strings[index] = '[censored]'
```

这个函数让你能够迭代索引-值对，其中索引是自动提供的。

#### 8.2.3 反向迭代和排序后迭代

这里介绍另外两个很有用的函数：reversed和sorted。它们类似于列表方法reverse和sort，但是可用于任何序列和可迭代的对象，且不立刻修改对象，而是返回反转和排序后的版本。

```powershell
>>> sorted([4,3,6,8,3])
[3, 3, 4, 6, 8]
>>> sorted('Hello,World!')
['!', ',', 'H', 'W', 'd', 'e', 'l', 'l', 'l', 'o', 'o', 'r']
>>> list(reversed('Hello,World!'))
['!', 'd', 'l', 'r', 'o', 'W', ',', 'o', 'l', 'l', 'e', 'H']
>>> '@'.join(reversed('Hello,World!'))
'!@d@l@r@o@W@,@o@l@l@e@H'
>>> '_'.join(reversed('Hello,World!'))
'!_d_l_r_o_W_,_o_l_l_e_H'
```

​		请注意，sorted返回一个列表，而reversed则像zip那样返回一个更神秘的可迭代对象。你无需关心这意味着什么，只管在for循环或join等方法中使用它。而不会有任何问题。只是你不能对它进行索引或切片操作，也不能对它调用列表的方法。如果想要执行这些方法，则需要先使用list对返回的对象进行转换。

要按照字母排序，可先转换为小写。为此，可将sort或sorted的key参数设置为str.lower。例如：

```powershell
>>> sorted('aBc',key = str.lower)
['a', 'B', 'c']
>>> sorted('aBc')
['B', 'a', 'c']
```

## 9. 三人行

这里大致讲一下另外三条语句：pass，del和exec。

### 9.1 什么都不做

Python pass是空语句，是为了保持程序结构的完整性（Python中代码块不能为空）。

pass 不做任何事情，一般用做占位语句，如下实例

```powershell
>>>while True:      
	pass  # 等待键盘中断 (Ctrl+C)
```

最小的类:

```
>>>class MyEmptyClass:      
	pass
```

以下实例在字母为 o 时 执行 pass 语句块:

```python
#!/usr/bin/python3   

for letter in 'Runoob':     
	if letter == 'o':       
		pass       
		print ('执行 pass 块')    
	print ('当前字母 :', letter)   
print ("Good bye!")
```

执行以上脚本输出结果为：

```
当前字母 : R
当前字母 : u
当前字母 : n
执行 pass 块
当前字母 : o
执行 pass 块
当前字母 : o
当前字母 : b
Good bye!
```

### 9.2 使用del删除

对于你不再使用的对象，Python通常会将其删除（因为没有任何变量和1数据结构成员指向它）。

```powershell
>>> scoundrel = {'age':42,'first name':'Robin','last name':'of Locksley'}
>>> robin = scoundrel
>>> scoundrel
{'age': 42, 'first name': 'Robin', 'last name': 'of Locksley'}
>>> robin
{'age': 42, 'first name': 'Robin', 'last name': 'of Locksley'}
>>> scoundrel = None
>>> scoundrel
>>> robin
{'age': 42, 'first name': 'Robin', 'last name': 'of Locksley'}
```

​		最初，两个变量指向同一个字典，因此将None赋值给其中一个变量，依然可以通过另一个变量访问这个字典。但是如果将另一个变量也置为None，则该字典就漂浮在计算机内存中，没有任何名称与之相关联，也再也无法获取或使用它。因此，python会直接将其删除，这又被称为**垃圾收集**。

​		另一种办法使用del语句，这样不仅会删除该对象的引用，还会删除名称本身。这样看似简单，但有时还是难以理解，例如当两个名称指向同一个列表时，使用del删除其中一个却并不会影响另一个，你仅仅时删除了一个访问该列表的名称而已。而实际上，在Python中，根本没有办法删除值，而且你也不需要这么做，因为对于你而言，你不再使用的值，Python解释器会自动将其删除（立刻）。

### 9.3 使用exec和eval执行字符串及计算其结果

有时候，你可能想动态的编写Python代码，并将其作为语句进行执行或作为表达式进行计算。

#### 9.3.1 exec

函数exec将字符串作为代码执行

```powershell
>>> exec('print("Hello,World!")')
Hello,World!
```

​		然而，调用函数时只给它提供一个参数绝非好事。在大多数情况下，还应向它传递一个命名空间--用于放置变量的的地方，否则代码将可能污染你的命名空间，即修改你的变量。例如，你在代码中使用了名称sqrt，会出现什么样的结果：

```powershell
>>> from math import sqrt
>>> sqrt(4)
2.0
>>> exec('sqrt = 1')
>>> sqrt(4)
Traceback (most recent call last):
  File "<pyshell#13>", line 1, in <module>
    sqrt(4)
TypeError: 'int' object is not callable
```

​		既然如此，为什么还要将字符串作为代码执行呢？函数exec主要用于动态地创建代码字符串。如果这些字符串来自其他地方（比如说用户），就几乎无法确认它将包含什么内容。因此为了安全起见，需要提供一个字典充当命名空间。为此，你添加第二个参数--字典，用作代码字符串的命名空间。

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> from math import sqrt
>>> scope = {}
>>> exec('sqrt = 1', scope)
>>> sqrt(4)
2.0
>>> scope['sqrt']
1
```

​		如你所见，可能带来破坏的代码并非覆盖函数sqrt。函数sqrt该怎样还怎样，而通过exec执行赋值语句创建的变量位于scope中。

​		请注意，如果你尝试将scope打印出来，将发现它包含很多内容，这是因为自动在其中添加了包含所有内置函数和值的字典**_ _buildins_ _**

#### 9.3.2 eval

​		eval是一个类似于exec的内置函数。exec执行一系列Python语句。而eval计算用字符串表示的python表达式的值，并返回结果（exec什么都不返回，因为它本身就是条语句）。例如，你可以使用如下代码创建一个Python计算器：

```powershell
>>> eval(input('Enter an arithmetic expression:'))
Enter an arithmetic expression:6 + 18 * 2
42
```

与exec一样，eval可以提供一个命名空间，虽然表达式通常不会像语句那样给变量重新赋值