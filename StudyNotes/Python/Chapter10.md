# 第十章：条件控制

​		到目前为止，你所编写的程序都是逐条顺序执行的，现在需要更进一步，让程序根据不同条件选择执行特定的语句块，这正是布尔值的用武之地。

​		Python 条件语句是通过一条或多条语句的执行结果（True 或者 False）来决定执行的代码块。

​		用作布尔表达式时，下面所有的值都会被解释器视为False:

```
False None 0 "" () [] {}
```

​		换而言之，标准值False和None,各种类型（包括浮点数，复数等）的数值为0，空序列（如空字符串，空元组，空列表）以及空映射（如空字典）都被视为False，而其他各种值包括标准值True，都被视为True。这意味着任何Python的值都可以被解释为真值。在有些语言中，标准真值为0（False）和1（True）。实际上，True和False不过是0和1的别名，知识表现形式不同：

```powershell
>>> True
True
>>> False
False
>>> True == 1
True
>>> False == 1
False
>>> False == 0
True
>>> True + False + 42
43
```

## 1. if 语句

​		Python中if语句的一般形式如下所示：

```
if condition_1:     
	statement_block_1 
elif condition_2:     
	statement_block_2 
else:     
	statement_block_3
```

- 如果 "condition_1" 为 True 将执行 "statement_block_1" 块语句
- 如果 "condition_1" 为False，将判断 "condition_2"
- 如果"condition_2" 为 True 将执行 "statement_block_2" 块语句
- 如果 "condition_2" 为False，将执行"statement_block_3"块语句

  ​	Python 中用 **elif** 代替了 **else if**，所以if语句的关键字为：**if – elif – else**。 

**注意：**

- 1、每个条件后面要使用冒号 :，表示接下来是满足条件后要执行的语句块。
- 2、使用缩进来划分语句块，相同缩进数的语句在一起组成一个语句块。
- 3、在Python中没有switch – case语句。

Gif 演示：

![img](../images/Python/006faQNTgw1f5wnm0mcxrg30ci07o47l.gif)

​		以下是一个简单的 if 实例：

```python
name = input("What's your name? ")
if(name.endswith('Gumby')):
    print('Hello,Mr.Gumby!')
```

​		这就是if语句，让你能够有条件的执行代码。以下实例演示了狗的年龄计算判断：

```python
#!/usr/bin/python3
 
age = int(input("请输入你家狗狗的年龄: "))
print("")
if age <= 0:
    print("你是在逗我吧!")
elif age == 1:
    print("相当于 14 岁的人。")
elif age == 2:
    print("相当于 22 岁的人。")
elif age > 2:
    human = 22 + (age -2)*5
    print("对应人类年龄: ", human)
 
### 退出提示
input("点击 enter 键退出")
```

​		将以上脚本保存在dog.py文件中，并执行该脚本：

```
$ python3 dog.py 
请输入你家狗狗的年龄: 1

相当于 14 岁的人。
点击 enter 键退出
```

​		`high_low.py`文件演示了数字的比较运算：

```python
#!/usr/bin/python3 
 
# 该实例演示了数字猜谜游戏
number = 7
guess = -1
print("数字猜谜游戏!")
while guess != number:
    guess = int(input("请输入你猜的数字："))
 
    if guess == number:
        print("恭喜，你猜对了！")
    elif guess < number:
        print("猜的数字小了...")
    elif guess > number:
        print("猜的数字大了...")
```

​		执行以上脚本，实例输出结果如下： 

```
$ python3 high_low.py 
数字猜谜游戏!
请输入你猜的数字：1
猜的数字小了...
请输入你猜的数字：9
猜的数字大了...
请输入你猜的数字：7
恭喜，你猜对了！
```

------

## 2. if 嵌套

​		在嵌套 if 语句中，可以把 `if...elif...else` 结构放在另外一个 `if...elif...else` 结构中。 

```
if 表达式1:
    语句
    if 表达式2:
        语句
    elif 表达式3:
        语句
    else:
        语句
elif 表达式4:
    语句
else:
    语句
```

​		实例：

```python
# !/usr/bin/python3
 
num=int(input("输入一个数字："))
if num%2==0:
    if num%3==0:
        print ("你输入的数字可以整除 2 和 3")
    else:
        print ("你输入的数字可以整除 2，但不能整除 3")
else:
    if num%3==0:
        print ("你输入的数字可以整除 3，但不能整除 2")
    else:
        print  ("你输入的数字不能整除 2 和 3")
```

​		将以上程序保存到 test_if.py  文件中，执行后输出结果为：

```
$ python3 test.py 
输入一个数字：6
你输入的数字可以整除 2 和 3
```

## 3. 更复杂的条件

​		以上时关于if的内容，接下来讲一下条件本身

### 3.1 比较运算符

​		在条件表达式中，最基本的运算符可能时比较运算符，主要用于比较：

| 表达式     | 描述                       |
| ---------- | -------------------------- |
| x == y     | x等于y                     |
| x  < y     | x小于y                     |
| x > y      | x大于y                     |
| x >= y     | x大于等于y                 |
| x <= y     | x小于等于y                 |
| x !=  y    | x不等于y                   |
| x is y     | x和y是同一个对象           |
| x is not y | x和y是不同的对象           |
| x in y     | x是容器（如序列）y的成员   |
| x not in y | x不是容器（如序列）y的成员 |

​		与赋值一样，Python也支持链式比较：可同时使用多个比较运算符，如0<age<100。

​		有些比较运算符需要特别注意，下面来详细介绍：

#### 3.1.1 相等运算符

​		要确认两个对象是否相等，可使用比较运算符，用两个等号（==）来表示，对于初学编程语言的人而言，一定要分清单个等号和双等号的区别：

- 单个等号：一般表示赋值运算符，无返回值
- 两个等号：一般表示相等运算符，返回布尔值

#### 3.1.2 相同运算符

​		这个运算符很有趣，其作用看似和==相同，但事实并非如此：

```powershell
>>> x = y = [1,2,3]
>>> z = [1,2,3]
>>> x == y
True
>>> x == z
True
>>> x is y
True
>>> x is z
False
```

​		如果还没看明白，可以看如下实例：

```powershell
>>> x = [1,2,3]
>>> y = [2,4]
>>> x is not y
True
>>> del x[2]
>>> y[1] = 1
>>> y.reverse
<built-in method reverse of list object at 0x00000264DE41C4C8>
>>> y.reverse()
>>> x == y
True
>>> x is y
False
>>> x
[1, 2]
>>> y
[1, 2]
```

​		总之，==是用于检查两个对象是否相等，而is是用来检查两个对象是否相同（即同一个对象）。这里需要注意的是，不要将is用于数和字符串等不可变的基本值，这样做在Python中可能导致不可预测的结果:

```powershell
>>> x = 1
>>> y = 1
>>> x is y
True
>>> z = 2
>>> x is y
True
>>> a = '11'
>>> b = '11'
>>> a is b
True
>>> c = '22'
>>> a is b
True
```

#### 3.1.3 成员资格运算符

​		成员资格运算符in,也可以像其他运算符一样，用于条件表达式中。

#### 3.1.4 字符串和序列比较

​		字符串是根据字符的字母排序顺序进行比较的。

```powershell
>>> 'Alice' < 'Bob'
True
```

​		虽然基于的是字母排列顺序，但字母都是Unicode字符，它们都是按照码点排列的。而实际上，字符是根据顺序值排列的。要熟悉顺序值，可使用函数ord，这个函数的作用与函数char相反

```powershell
>>> 'a' < 'b'
True
>>> ord('a')
97
>>> ord('b')
98
```

### 3.2 布尔运算符

​		至此，你已经见过很多返回真值的表达式，但你可能需要检查多个条件。假设你要编写一个程序，实现读取一个数并判断是否在1-10之间：

```python
num = input('Please input a number:')
if(num <= 10):
	if(num >= 1):
		print("Great!")
	else:
		print("Wrong!")
else:
	print('wrong!')
```

​		以上方法虽然可行，但有点笨拙，因为出现了重复代码，重复劳动并不是一件好事，以下为一个优化方案：

```python
num = input('Please input number:')
if(num <= 10 and num >= 1):
	print('Great!')
else:
	print("Wrong!")
```

​		布尔运算符有个有趣的特征：只做必要的运算。例如，仅当x和y都为真，表达式x and y才为真，而当x为假时，该表达式会直接返回假，而不会关心y的真假。这种行为又被称为**短路逻辑**（或**延迟求值**），布尔运算符又常被称为逻辑运算符。

## 4. 断言

​		if语句有个很有用的'亲戚'，其工作原理类似于下面的伪代码：

```
if not condition:
	crash program
```

​		编写类似的代码，是因为程序在错误条件出现时立即崩溃胜过以后再崩溃。基本上，你可能要求某些条件满足(如核实函数参数满足要求或为初始测试和调试提供帮助)，为此可在语句中使用关键字**assert**

```powershell
>>> age = 10
>>> assert 0 < age < 100
>>> age = 0
>>> assert 0 < age < 100
Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    assert 0 < age < 100
AssertionError
```

​		如果知道必须满足某个特定条件，程序才能正确的运行，可在程序中添加assert语句充当检查点，还可以在条件后添加一个字符串，对断言做出说明（注意逗号分隔）：

```powershell
>>> age = -1
>>> assert 0 < age < 100, 'The age must be realistic!'
Traceback (most recent call last):
  File "<pyshell#38>", line 1, in <module>
    assert 0 < age < 100, 'The age must be realistic!'
AssertionError: The age must be realistic!
```

