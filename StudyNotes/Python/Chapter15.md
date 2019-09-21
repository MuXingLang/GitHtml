# 第十五章：异常

​		作为 Python 初学者，在刚学习 Python 编程时，经常会看到一些报错信息，在前面我们没有提及，这章节我们会专门介绍。Python 有两种错误很容易辨认：语法错误和异常。

## 1. 语法错误

​		Python 的语法错误或者称之为解析错，是初学者经常碰到的，如下实例

```powershell
>>>while True print('Hello world')
  File "<stdin>", line 1, in ?
    while True print('Hello world')
                   ^
SyntaxError: invalid syntax
```

​		这个例子中，函数 print() 被检查到有错误，是它前面缺少了一个冒号（:）。

​		语法分析器指出了出错的一行，并且在最先找到的错误的位置标记了一个小小的箭头。

## 2. 异常

​		即便Python程序的语法是正确的，在运行它的时候，也有可能发生错误。运行期检测到的错误被称为异常。异常可能是错误（如视图除以零），也可能是通常不会发生的事。

​		Python使用异常对象来表示异常对象，并且在遇到错误时引发异常。大多数的异常都不会被程序处理，都以错误信息的形式展现在这里:

```powershell
>>>10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
ZeroDivisionError: division by zero
>>> 4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: Can't convert 'int' object to str implicitly
```

​		异常以不同的类型出现，这些类型都作为信息的一部分打印出来: 例子中的类型有 `ZeroDivisionError`，`NameError` 和` TypeError`。 

​		错误信息的前面部分显示了异常发生的上下文，并以调用栈的形式显示具体信息。 如果异常只用来显示错误信息，就没太大意思。但实际上，每个异常都是某个类的实例。你能以各种方式引发和捕获这些实例，从而逮住错误并采取措施，而不是放任整个程序失败。

## 3. 让事情沿着你指定的轨道出错

​		正如你所见，出现问题时，将自动引发异常。先来看看如何自主引发异常，还有如何创建异常，然后再学习如何处理这些异常。

### 3.1 raise语句

​		要引发异常，要使用raise语句，并将一个类（必须是Exception的子类）或实例作为参数，将类作为参数时，将会自动创建一个实例。

```powershell
>>> raise Exception
Traceback (most recent call last):
  File "<pyshell#14>", line 1, in <module>
    raise Exception
Exception
>>> raise Exception('hyperdriver overload')
Traceback (most recent call last):
  File "<pyshell#15>", line 1, in <module>
    raise Exception('hyperdriver overload')
Exception: hyperdriver overload
```

​		以上例子中，第一个引发的是通用异常，没有指出出现了什么错误。而在第二个示例中，添加了错误消息。Python中有很多内置的异常类，下表中将介绍最重要的几个：

| 类名              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| Exception         | 几乎所有的异常类都是从它派生出来的                           |
| AttributeError    | 引用属性或给它赋值失败时引发                                 |
| OSError           | 操作系统不能执行指定的任务（如打开文件）时引发，有多个子类   |
| IndexError        | 使用序列中不存在的索引时引发，为LookError的子类              |
| KeyError          | 使用映射中不存在的键时引发，为LookError的子类                |
| NameError         | 找不到名称（变量）时引发                                     |
| SyntaxError       | 代码不正确时引发                                             |
| TypeError         | 将内置操作或函数用于类型不正确的对象时引发                   |
| ValueError        | 将内置操作或函数用于这样的对象（其类型正确但包含的值不合适）时引发 |
| ZeroDivisionError | 在除法或求模运算中第二个参数为0时触发                        |

### 3.2 自定义的异常类

​		虽然内置异常涉及的范围很广，能够满足很多需求，但有时候你可能想自己创建异常类。你可以通过创建一个新的异常类来拥有自己的异常。异常类继承自 Exception 类，可以直接继承，或者间接继承，例如:

```powershell
>>>class MyError(Exception):
        def __init__(self, value):
            self.value = value
        def __str__(self):
            return repr(self.value)
   
>>> try:
        raise MyError(2*2)
    except MyError as e:
        print('My exception occurred, value:', e.value)
   
My exception occurred, value: 4
>>> raise MyError('oops!')
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
__main__.MyError: 'oops!'
```

​		在这个例子中，类 Exception 默认的 `__init__()` 被覆盖。

​		当创建一个模块有可能抛出多种不同的异常时，一种通常的做法是为这个包建立一个基础异常类，然后基于这个基础类为不同的错误情况创建不同的子类:

```powershell
class Error(Exception):
    """Base class for exceptions in this module."""
    pass
 
class InputError(Error):
    """Exception raised for errors in the input.
 
    Attributes:
        expression -- input expression in which the error occurred
        message -- explanation of the error
    """
 
    def __init__(self, expression, message):
        self.expression = expression
        self.message = message
 
class TransitionError(Error):
    """Raised when an operation attempts a state transition that's not
    allowed.
 
    Attributes:
        previous -- state at beginning of transition
        next -- attempted new state
        message -- explanation of why the specific transition is not allowed
    """
 
    def __init__(self, previous, next, message):
        self.previous = previous
        self.next = next
        self.message = message
```

​		大多数的异常的名字都以"Error"结尾，就跟标准的异常命名一样。

## 4.捕获异常

​		前面说过，异常比较有趣的地方是可对其进行处理，通常称之为捕获异常。为此，可使用try/catch语句。假设你创建了一个简单程序：让用户输入两个数，再让它们相除。

```powershell
>>> x = int(input('Enter the first number: '))
Enter the first number: 12
>>> y = int(input('Enter the second number: '))
Enter the second number: 13
>>> print(x/y)
0.9230769230769231
```

​		这个程序可以在用户输入合法输入时正常运行，但是，显然，当用户第二个参数输入0的时候，会引发异常，为了捕获这种异常并对错误进行处理（这里只是打印一条对用户更友好的错误消息），可如下重新编写程序：

```python
try:
	x = int(input('x = '))
	y = int(input('y = '))
	print(x/y)
except ZeroDivisionError:
	print('The Second number can not be zero!')
```

​		就本例而言，可能使用if语句来检查y的值好像更简单些。然而，如果使用这个程序执行的除法运算更多，怎每一个除法运算都需要一条if语句，二而使用try/catch的话，只需要一条错误处理程序，当然，这只是其中一条好处罢了。

​		异常从函数向外传播到调用函数的地方。如果此处也没有被捕获异常，则异常会向顶层传播。这意味着你可以使用try/catch来捕获他人所编写函数引发的异常。

### 4.1 不提供参数

​		捕获异常后，如果要重新引发它（即继续向上传播），可调用raise且不提供任何参数（当然也可以显示的额提供捕获到的异常）。

​		为说明这很有用，来看一条能抑制`ZeroDivisionError`的计算器类。如果启用了这种功能，计算器将打印一条错误消息，而不继续让异常传播。在与用户交互的会话中使用这个计算器时，抑制异常很有用，而在程序内部使用时，引发异常是更佳的选择。

```python
class MuffledCalculator:
    muffled = False
    def calc(self,expr):
        try:
            return eval(expr)
        except ZeroDivisionError:
            if(self.muffled):
                print('Division by zero is illegal')
            else:
                raise

```

> 注意：发生零整除行为时，如果启用了“抑制”功能，犯法calc会隐式的返回None。换而言之，如果启用了抑制功能，就不应该依赖返回值。

​		下面将演示这个类的用法（包括启用和关闭了抑制功能的情形）：

```
>>> calculator = MuffledCalculator()
>>> calculator.calc('10/2')
5.0
>>> calculator.calc('10/0')
Traceback (most recent call last):
  File "<pyshell#29>", line 1, in <module>
    calculator.calc('10/0')
  File "<pyshell#26>", line 5, in calc
    return eval(expr)
  File "<string>", line 1, in <module>
ZeroDivisionError: division by zero
>>> calculator.muffled = True
>>> calculator.calc('10/0')
Division by zero is illegal
```

​		如你所见，关闭抑制功能时，捕获了异常ZeroDivisionError，但继续向上传播它。

​		如无法处理异常，在except子句中使用不带参数的raise通常是不错的选择，但有时你可能引发别的异常。在这种情况下，导致进入except的语句将作为异常上下文存储起来，并出现在最终的错误消息中，如下所示：

```powershell
>>> try:
    1/0
except ZeroDivisionError:
    raise ValueError

Traceback (most recent call last):
  File "<pyshell#39>", line 2, in <module>
    1/0
ZeroDivisionError: division by zero

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<pyshell#39>", line 4, in <module>
    raise Valu
```

​		在处理上述异常时，引发了一个新的异常，你可以使用raise...from000语句来提供自己的异常上下文，也可以使用None来禁用上下文。

```powershell
>>> try:
    1/0
except ZeroDivisionError:
    raise ValueError from None

Traceback (most recent call last):
  File "<pyshell#41>", line 4, in <module>
    raise ValueError from None
ValueError
```

### 4.2 多个except语句

​		如果你运行前一节的程序，并在提示时输入一个非数字值，将引发另一种异常`TypeError`。

​		由于之前的程序except子句只捕获`ZeroDivisionError`，所以`TypeError`异常就成了漏网之鱼，从而导致程序终止。为同时捕获这种异常，可在try/catch语句中再添加一个except子语句：

```
try:
	x = int(input('x = '))
	y = int(input('y = '))
	print(x/y)
except ZeroDivisionError:
	print('The Second number can not be zero!')
except TypeError:
	print('That is not a number,was it? ')
```

​		现在想使用if语句来处理会更加困难。如何检查一个值是否能用于除法运算？方法有很多，而最佳的方法就是尝试将两个数相除，看看是否可行。

​		另外，注意到异常处理并不会导致代码混乱，而添加大量if语句用来检查各种可能的错误状态会导致代码可读性极差。

### 4.3 一箭双雕

​		如果要使用一个except子句捕获多种异常，可在一个元组指定这些异常，如下所示：

```python
try:
	x = int(input('x = '))
	y = int(input('y = '))
	print(x/y)
except (ZeroDivisionError,TypeError,NameError):
	print('Your number were bogus!')
```

​		在上述语句中，无论是用户输入字符串，或其他非数字，或输入的第二个数为0，都会打印同样的错误提示消息而不是报错。当然，打印出错误消息本身帮助并不大。另一种解决方案是不断的要求用户重新输入数字，直至能够执行除法运算为止。

​		在`except`语句中，异常两边的圆括号很重要。一种常见的错误是省略这些括号，这可能导致你不想看到的结果。

### 4.4 捕获对象

​		要在`except`语句中访问异常对象本身，可使用两个而不是一个参数。（请注意，即便是在你捕获多个异常时，也只向`except`提供一个参数--一个元组）需要让程序继续运行并记录错误（可能只是向用户显示）时，这很有用。以下是一个简单例子：

```python
try:
	x = int(input('x = '))
	y = int(input('y = '))
	print(x/y)
except (ZeroDivisionError,TypeError,NameError) as e:
	print(e)
```

​		在这个小程序中，`except`捕获多种异常，但是由于你同时显式地捕获了异常对象本身，因此可以将异常对象打印出来，让用户知道发生了什么情况。

### 4.5 一网打尽

​		即使程序处理了好几种异常，还是可能有一些漏网之鱼。例如，对于前面执行除法运算的程序如果用户在提示时不输入任何内容就按回车键，将会出现一条错误消息，还有一些相关问题出现在什么地方的信息（栈跟踪），如下所示：

```
Trace (most recent call last):
...
ValueError:invalid literal for int() with base 10:''
```

​		这种异常未被try/except语句捕获，这理所当然，因为你没有预测到这种问题，也没有采取相应的措施。在这些情况下，与其他使用并非要捕获这些异常的try/except语句将它们隐藏起来，还不如让程序马上崩溃，因为这样你就知道什么地方出错了。

​		然而，如果你就是要使用一段代码捕获所有的异常，只需要在except语句中不指定任何异常类即可。

```python
try:
	x = int(input('x = '))
	y = int(input('y = '))
	print(x/y)
except:
	print('something wrong happened...')
```

​		现在，用户想怎么做都可以。然而像这种捕获所有的异常很危险，因为这不仅会隐藏你有心理准备的错误，还会隐藏你没有考虑过的错误。这还将捕获用户时使用`Ctrl+C`终止执行的企图，调用函数`sys.exit`来终止执行的企图等。在大多数情况下，更好的选择是使用except Exception as e并对异常对象进行检查。这样做将让不是从`Exception`派生而来的为数不多的异常成为漏网之鱼，其中包括`SystemExit`和`KeyboardInterrupt`，因为它们是从`BaseException`（`Exception`的超类）派生而来的。

### 4.6 万事大吉时

​		在有些情况下，有没有出现异常时执行一个代码块很有用。为此，可像条件语句和循环一样，给try/except语句添加一个else语句：

```python
try:
	print('A simple task')
except:
	print('What?Something went wrong?')
else:
	print('Ah ... It went as planned.')
```

​		如果你运行这些代码，输出将如下：

```
A Simple task
Ah ... It went as planned
```

​		通过使用else语句，可以实现循环输入：

```python
while True:
	try:
		x = int(input('x = '))
		y = int(input('y = '))
		print(x/y)
	except:
		print('Invalid input,Please try again!')
	else:
		break
```

​		以上代码仅在没有引发异常的情况下才会跳出循环（通过由else子语句中的break语句实现）。换而言之，只要出现错误，程序就会要求用户重新输入。

​		前面说过，一种更佳的替代方案是使用空的except子句来捕获所有属于类`Exception`（或其子类）的异常。你不能完全确认这将捕获所有异常，因为try/except语句中的代码可能使用旧式的字符串异常或引发并非从Exception派生出来的的异常。然而，如果使用except Exception as e，则可以实现如下功能：

```python
while True:
	try:
		x = int(input('x = '))
		y = int(input('y = '))
		value = x / y
		print('x/y is ',value)
	except Exception as e:
		print('Invalid input:',e)
		print('Please try again!')
	else:
		break
```

### 4.7 最后

​		最后，还有`finally`子句，可用于在发生异常时执行清理工作。这个子句是与try子句配套的。

```python
x = None
try:
	x = 1/0
finally:
	print('cleaning up ...')
	del x
```

​		在上述实例中，不管try子句中发生什么异常，都将会执行`finally`子句。为何在try子句之前初始化x呢？因为不这样做，`ZeroDivisionError`将导致根本没有机会给它赋值，进而导致`finally`子句中对其执行del时引发未捕获的异常。

​		如果运行以上错误，它将在执行清理工作后崩溃。虽然使用del来删除变量是相当愚蠢的清理措施，但`finally`子句非常适合用于确保文件或网络套接字等得以关闭。以下是一个完整例子：

```python
try:
	1/0
except NameError:
	print('Unknown variable')
else:
	print('That went well!')
finally:
	print('Cleaning up')
```

## 5. 异常处理

​		以下例子中，让用户输入一个合法的整数，但是允许用户中断这个程序（使用 `Control-C `或者操作系统提供的方法）。用户中断的信息会引发一个 `KeyboardInterrupt` 异常。 

```
>>>while True:
        try:
            x = int(input("Please enter a number: "))
            break
        except ValueError:
            print("Oops!  That was no valid number.  Try again   ")
```

​		try语句按照如下方式工作；

- 首先，执行try子句（在关键字try和关键字except之间的语句）
- 如果没有异常发生，忽略except子句，try子句执行后结束。
- 如果在执行try子句的过程中发生了异常，那么try子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的except子句将被执行。最后执行 try 语句之后的代码。
- 如果一个异常没有与任何的except匹配，那么这个异常将会传递给上层的try中。

  ​	一个 try 语句可能包含多个except子句，分别来处理不同的特定的异常。最多只有一个分支会被执行。

  ​	处理程序将只针对对应的try子句中的异常进行处理，而不是其他的 try 的处理程序中的异常。

  ​	一个`except`子句可以同时处理多个异常，这些异常将被放在一个括号里成为一个元组，例如: 

```
except (RuntimeError, TypeError, NameError):         
	pass
```

​		最后一个`except`子句可以忽略异常的名称，它将被当作通配符使用。你可以使用这种方法打印一个错误信息，然后再次把异常抛出。

```python
import sys
 
try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error: {0}".format(err))
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexpected error:", sys.exc_info()[0])
    raise
```

​		try    except 语句还有一个可选的else子句，如果使用这个子句，那么必须放在所有的except子句之后。这个子句将在try子句没有发生任何异常的时候执行。例如: 

```python
for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except IOError:
        print('cannot open', arg)
    else:
        print(arg, 'has', len(f.readlines()), 'lines')
        f.close()
```

​		使用 else 子句比把所有的语句都放在 try 子句里面要好，这样可以避免一些意想不到的、而`except`又没有捕获的异常。

异		常处理并不仅仅处理那些直接发生在try子句中的异常，而且还能处理子句中调用的函数（甚至间接调用的函数）里抛出的异常。例如:

```python
>>>def this_fails():
        x = 1/0
   
>>> try:
        this_fails()
    except ZeroDivisionError as err:
        print('Handling run-time error:', err)
   
Handling run-time error: int division or modulo by zero
```

## 6. 抛出异常

​		Python 使用 `raise` 语句抛出一个指定的异常。例如:

```powershell
>>>raise NameError('HiThere')
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
NameError: HiThere
```

​		`raise` 唯一的一个参数指定了要被抛出的异常。它必须是一个异常的实例或者是异常的类（也就是 `Exception` 的子类）。

​		如果你只想知道这是否抛出了一个异常，并不想去处理它，那么一个简单的 `raise` 语句就可以再次把它抛出。

```powershell
>>>try:
        raise NameError('HiThere')
    except NameError:
        print('An exception flew by!')
        raise
   
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in ?
NameError: HiThere
```

## 7. 定义清理行为

​		try 语句还有另外一个可选的子句，它定义了无论在任何情况下都会执行的清理行为。 例如:

```
>>>try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
... 
Goodbye, world!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
KeyboardInterrupt
```

​		以上例子不管 try 子句里面有没有发生异常，finally 子句都会执行。

​		如果一个异常在 try 子句里（或者在 except 和 else 子句里）被抛出，而又没有任何的 except 把它截住，那么这个异常会在 finally 子句执行后被抛出。

​		下面是一个更加复杂的例子（在同一个 try 语句里包含 except 和 finally 子句）:

```
>>>def divide(x, y):
        try:
            result = x / y
        except ZeroDivisionError:
            print("division by zero!")
        else:
            print("result is", result)
        finally:
            print("executing finally clause")
   
>>> divide(2, 1)
result is 2.0
executing finally clause
>>> divide(2, 0)
division by zero!
executing finally clause
>>> divide("2", "1")
executing finally clause
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
  File "<stdin>", line 3, in divide
TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

## 8. 预定义的清理行为

​		一些对象定义了标准的清理行为，无论系统是否成功的使用了它，一旦不需要它了，那么这个标准的清理行为就会执行。

​		这面这个例子展示了尝试打开一个文件，然后把内容打印到屏幕上: 

```
for line in open("myfile.txt"):     
	print(line, end="")
```

​		以上这段代码的问题是，当执行完毕后，文件会保持打开状态，并没有被关闭。

​		关键词 with 语句就可以保证诸如文件之类的对象在使用完之后一定会正确的执行他的清理方法:

```
with open("myfile.txt") as f:     
	for line in f:         
		print(line, end="")
```

​		以上这段代码执行完毕后，就算在处理过程中出问题了，文件 f 总是会关闭。 

## 9. 异常和函数

​		异常和函数有着天然的联系。如果不处理函数中引发的异常。它将向上传播到调用函数的地方。如果此处还未得到处理，异常将会继续传播，直至到达主程序（全局作用域）。如果主程序中也没有异常处理程序，程序将终止并显示栈跟踪消息。

```powershell
>>> def faulty():
		raise Exception('Something is wrong!')

>>> def ignore_exception():
		faulty()

>>> def handle_exception():
		try:
			faulty()
		except:
			print('Exception handled!')
			
			
>>> ingore_exception()
Traceback(most recent call last):
	File '<stdin>', line 1, in ?
	File '<stdin>', line 2, in ignore_exception
	File '<stdin>', line 2, in faulty
Exception:Something is wrong
>>> handle_exception()
Exception handled
```

​		如你所见，faulty中引发的异常依次从faulty和ignore_exception向外传播，最终导致显示一条栈跟踪消息。调用handle_exception时，异常最终传播到handle_exception，并被这里的try/except语句处理。

## 10. 不那么异常的情况

​		如果你只想发出警告，指出情况偏离了正规，可使用模块`warnings`中的`warn`。

```powershell
>>> from warnings import warn
>>> warn("I've got a bad feeling about this.")

Warning (from warnings module):
  File "D:/System Lib/Documents/2019-09-06/MuffledCalculator.py", line 1
    class MuffledCalculator:
UserWarning: I've got a bad feeling about this.
```

​		警告只显示一次。如果再次运行最后一行代码，什么事都不会发生。

​		如果其他代码在使用你的模块，可使用模块`warnings`中的函数`filterwarnings`来抑制你发出的警告（或特定类型的警告），并指出要采取的措施，如“error”或“ignore”。

```powershell
>>> from warnings import filterwarnings
>>> filterwarnings("ignore")
>>> warn('Anyone out there?')
>>> filterwarnings("error")
>>> warn("Something is very wrong!")
Traceback (most recent call last):
  File "<pyshell#48>", line 1, in <module>
    warn("Something is very wrong!")
```

​		如你所见，引发的异常为UserWarning。发出警告时，可指定要引发的异常（即警告类别），但必须是`Warning`的子类。如果将警告转换为错误，将使用你指定的异常。另外，还可根据错误来过滤特定类型的警告。

```powershell
>>> filterwarnings('error')
>>> warn("This function is really old...", DeprecationWarning)
Traceback (most recent call last):
  File "<pyshell#50>", line 1, in <module>
    warn("This function is really old...", DeprecationWarning)
DeprecationWarning: This function is really old...
>>> filterwarnings('ignore', category=DeprecationWarning)
>>> warn("Another deprecation warning",DeprecationWarning)
>>> warn('Something else.')
Traceback (most recent call last):
  File "<pyshell#53>", line 1, in <module>
    warn('Something else.')
UserWarning: Something else.
```

​		除以上基本用途外，模块warnings还提供一些高级功能。如果你对此感兴趣，请参阅参考手册。