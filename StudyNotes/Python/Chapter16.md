# 第十六章：开箱即用

​		Python不仅语言核心十分强大，还提供了其他工具以供使用。标准安装包含一组称为标注库（standard library）的模块，你见过其中的一些（如math和cmath），但还有其他很多。

## 1. 模块

​		你已经知道如何创建和执行程序（或脚本），还知道如何使用import将函数从外部模块导入到程序中。

```powershell
>>> import math
>>>	math.sin(0)
0.0
```

​		下面来看看如何编写自己的模块

### 1.1 模块就是程序

​		任何Python程序都可作为模块导入。假设你编写了一个简单的`helloworld`程序，并将其保存在文件`hello.py`中，这个文件的名称（不包含扩展名.py）将成为模块的名称。

```
#hello.py
print("Hello,World!")
```

​		文件的存储位置也很重要。这里假设这个文件存储在目录`C:\python(Windows)~/python`(UNIX/macOS)中。要告诉解释器去哪里查找这个模块，可执行如下命令（以Windows目录为例）：

```
>>> import sys
>>> sys.path.append('C:/python')
```

> **提示**：在UNIX中，不能直接将字符串'~/python'附加到sys.path末尾，而必须使用完成的路径（如’/home/yourusername/python‘）。如果你要自动创建完整的路径，可使用sys.path.expanduser('~/python')。

​		这告诉解释器，除了通常将查找的位置外，还应到目录C:\python中查找这个模块。这样做后，就可以导入这个模块（它存储在文件C:\python\hello.py）。

```powershell
>>> import hello
Hello,World!
```

> **注意**：当你导入模块时，可能会发现其所在目录中除源代码文件外，还新建了一个名为_ _ pycache _ _的子目录（在旧版本的Python中，是扩展名为.pyc的文件）。这个目录包含处理过后的文件，Python能够更高效地处理它们。以后再导入这个模块时，如果.py文件未发生变化，Python将导入处理后的文件，否则将重新生成处理后的文件。删除目录_ _ _ pacache _  _不会有任何坏处，因为必要时会重新创建它。

​		如你所示，导入这个模块时，执行了其中的代码。但是如果再次导入它，什么事情都不会发生。为何没有再执行其中的代码呢？因为模块并不是用来执行操作（如打印文本）的，而是用于**定义**变量，函数，类等。鉴于定义只需要做一次，因此导入模块多次和导入一次的效果相同。

## 1.2 为何只导入一次

​		在大多数情况下，只导入一次是重要的优化，且在下述特殊情况下显得尤为专业：两个模块彼此导入对方。

​		在很多情况下，你可能编写两个这样的模块：需要彼此访问对方的函数和类才能正确的发挥作用。例如，你可能创建了两个模块clientdb和billing，分别包含客户数据库和记账系统的代码。客户数据库可能包含对记账系统的调用（如每月自动向客户发送账单），而记账系统可能需要访问客户数据库的功能才能正确的完成记账。

​		在这里，如果每个模块都可导入多次，就会出现问题。模块clientdb导入binding，而binding又导入clientdb，结果可想而知：最终将形成无穷的导入循环（还记得无穷递归么？）。然而，由于第二次导入什么都不会发生，这种循环被打破。

​		而如果一定要重新加载模块，可使用模块importlib中的函数reload，它接受一个此参数（要重新加载的模块），并返回重新加载的模块。如果在程序运行时修改了模块，并希望这种修改反映到程序中，这将很有用。要重新加载前述简单的模块hello，可像下面这样做：

```powershell
>>> import importlib
>>> hello = importlib.reload(hello)
Hello,world!
```

​		这里假设hello已导入（一次）。通过将函数reload的结果赋给hello，用重新加载的版本替换了以前的版本。由于打印出了问候语，说明这里确实是导入了这个模块。

​		通过实例化模块bar中的类Foo创建对象x后，如果重新加载模块bar，并不会重新重新创建x指向的对象，即x依然是（来自旧版bar的）旧版Foo的对象。要让x指向基于重新加载的模块中的Foo创建的对象，需要重新创建它。

## 1.3 模块是用来下定义的

​		模块是首次导入程序时执行。这看似有点用，但是用处不大。让模块值得被创建的原因在于它们像类一样，有自己的作用域。这意味着模块中定义的类和函数以及对其进行赋值的变量都将成为模块的属性。

### 1.3.1 在模块中定义函数

​		假设你编写了一个简单的模块，并将其存储在文件`hello2.py`中。另外，假设你将这个文件放在Python解释器能够找到的地方。模块代码如下：

```
#hello2.py
def hello():
	print('Hello,World!')
```

​		现在可以像下面这样导入它：

```
>>>import hello2
```

​		这将会执行这个模块，也就是说这个模块的作用域内定义函数hello，因此可以像下面这样访问这个函数：

```
>>>hello2.hello()
Hello,World!
```

​		在模块的全局作用域内定义的名称都可像上面这样访问。为何要这样做呢？为何不在主程序中定义一切呢？主要还是为了**代码重用**。通过将代码放到模块中，就可在多个程序中使用它们。这意味着如果你编写了一个出色的客户端数据库，并将其放在模块clientdb中，就可在记账时以及任何需要访问客户数据的程序中使用它。如果没有放在独立的模块中，就需要每个这样的程序中重新编写它。因此，要让代码是可重用的，务必将其模块化，这也与抽象密切相关。

### 1.3.2 在模块中添加测试代码

​		模块用于定义函数和类等，但是有些情况下（实际上是经常），添加一些测试代码来检查情况是否符合预期且很有用。例如，如果要确认函数hello管用，你可能将模块hello2重写为代码：

```python
#hello3.py
def hello():
	print('Hello,World!')
	
# 测试
hello()
```

​		这看似合理：如果将这个模块作为普通程序运行，将发现它运行正常。然而，如果在另一个程序中将其作为模块导入，以便能够使用函数hello，也将默认执行该测试代码，就像第一个hello模块一样。

```powershell
>>>import hello3
Hello,World!
>>>hello3.hello()
Hello,World!
```

​		这样的效果一般不是预期的结果。要避免这种行为，关键在于检查模块是作为程序运行还是被导入另一个程序。为此，需要使用变量`__name__`。

```powershell
>>> __name__
'__main__'
>>>hello3.__name__
'hello3'
```

​		如你所见，在主程序中（包括解释器的交互式提示符），变量`__name__ `的值是`__main __`，而在导入的模块中，这个变量被设置为该模块的名称。因此，要让模块中的测试代码的行为更合理，可将其放在一条if语句中，一个包含有条件地执行地测试代码的模块如下所示：

```python
# hello4.py

def hello():
	print('Hello,World!')
	
def test():
	hello()

if __name__ == '__main__':
	test()
```

​		如果将这个模块作为程序运行，将执行函数hello；如果导入它，，其行为将更像普通模块一样。

```powershell
>>>import hello4
>>>hello4.hello()
Hello,World!
```

​		如你所见，我将测试代码放在了test中。原本可以将这些代码直接放在if语句中但通过将其放在一个独立的测试函数中，可在程序中导入模块并对其进行测试。

```powershell
>>>hello4.test()
Hello,World!
```

​		如果要编写更详尽的测试代码，将其放在一个独立的程序可能是个不错的主意。

### 1.3.3 让模块可用

​		在前面的示例中，我修改了`sys.path`。`sys.path`包含了一个目录（表示为字符串）列表，解释器将在这些目录中查找模块。然而，通常你不想这样做。最理想的情况是`sys.path`一开始就包含正确的目录（你的模块所在的目录）。为此有两种办法：1，将模块放在正确的位置；2，告诉编译器到哪里去查找。接下来的两节将分别讨论这两种解决方案。如果要让别人能够轻松使用你的模块，那就是另外一码事了。Python打包技术一度日益复杂，各自为政，尽管现已被Python Packaging Authority控制并简化，但需要学习的还是有很多。这里不深入介绍这个棘手的主题，建议参阅“Python打包用户指南”：`packaging.python.org`。

#### 1.3.3.1 将模块放在正确的位置

​		将模块放在正确的位置很容易，只需要找出Python解释器到哪里去查找模块，再将文件放在这些地方即可。在你使用的计算机中，如果Python解释器是管理员安装的，而你没有管理员权限，就可能无法将模块保存在Python使用的目录中，则需要使用方法二。

​		你可能还记得，可在模块sys的变量python中找到目录列表（即搜索路径）。

```powershell
>>> import sys,pprint
>>> pprint.pprint(sys.path)
['',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36\\Lib\\idlelib',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36\\python36.zip',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36\\DLLs',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36\\lib',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36',
 'C:\\Users\\AppData\\Local\\Programs\\Python\\Python36\\lib\\site-packages']
```

> **提示**：如果要打印的数据结构太大，一行容不下，可使用模块pprint中的函数pprint（而不是普通的print语句）。pprint是一个卓越的打印函数，能够更妥善的打印输出。

​		当然，你得到的打印结果可能与这里显示的不完全相同。这里的要点是，每个字符串都表示一个位置，如果要让解释器找到这些模块，可将其放在任何一个位置中。虽然放在这里显示的任何一个位置都行，但目录site-packages`是最佳的选择，因为它就是用来放置模块的。请在计算机中查看`sys.path`，找到目录`site-packages，并将模块保存在这里。

​		只要模块位于类似于`site-packages`这样的地方，所有程序就都能导入它。

#### 1.3.3.2 告诉解释器去哪里查找

​		将模块放在正确的位置可能不是合适的解决方案，其中原因有很多：

- 不希望Python解释器的目录中充斥着你编写的模块；

- 没有必要的权限，无法将文件保存在Python解释器的目录中；

- 像将模块放在其他地方；

​		最重要的是，如果将模块放在其他地方，就必须告诉解释器去哪里查找。前面说过，要告诉解释器去哪里去查找模块，办法之一是直接修改`sys.path`，但这种做法不常见。标准做法是将模块所属的目录包含在环境变量`PYTHONPATH`中。

​		环境变量PYTHONPATH中的内容随操作系统而异，但它基本上类似于sys.path，也是一个目录列表。环境变量并不是Python解释器的一部分，而是操作系统的一部分。大致而言，它们类似于Python变量，但是在Python解释器外面设置的。

​		如果你使用的是`bash shell`（在大多数类UNIX系统，macOS和较新的Windows版本中都有），就可以使用如下命令将'~/python'附加到环境变量PYTHONPATH末尾：

```powershell
export PYTHONPATH=$PYTHONPATH:~/python
```

​		如果要对所有启动的shell都执行这个命令，可将其添加到主目录中的.bashrc文件中。

​		除使用环境变量PYTHONPATH外，还可以使用路径配置文件，这些文件的扩展名为.pth，位于一些特殊目录中，包含在要添加到sys.path中的目录。

### 1.3.4 包

​		为组织模块，可将其编组为包（package）。包其实就是另一种模块，但有趣的是它们可包含其他的模块。模块存储在扩展名为.py的文件中，而包则是一个目录。要被Python视为包，目录必须包含文件`__init__.py`。如果像普通模块一样导入包，文件`__init__.py`的内容就将是包的内容。例如，如果有一个名为constants的包，而文件`constants/__init__.py`包含语句PI = 3.14，就可以像下面这样做：

```python
import constants
print(constants.PI)
```

​		要将模块加入包中，只需将模块文件放在包目录中即可。你还可以在包中嵌入其他包。例如，要创建一个名为`drawing`的包，其中包含模块`shapes`和`colors`，需要创建如下所示的文件和目录（UNIX路径）：

|            文件/目录             |        描  述         |
| :------------------------------: | :-------------------: |
|            ~/python/             |  PYTHONPATH中的目录   |
|        ~/python/drawing/         |  包目录（包drawing）  |
| ~/python/drawing/_ _ init _ _.py | 包代码（模块drawing） |
|    ~/python/drawing/colors.py    |      模块colors       |
|    ~/python/drawing/shapes.py    |      模块shapes       |

​		完成以上这些准备工作后，下面的语句都是合法的：

```python
import drawing				#（1）导入drawing包
import drawing.colors		#（2）导入drawing包中的模块colors
from drawing import shapes	#（3）导入模块shapes
```

​		执行第一条语句后，便可以使用目录drawing中文件`__init__.py`的内容，但是不能使用模块shapes和colors的内容。执行第二条语句后便可以使用模块colors，但只能通过全限定名drawing.colors来使用。执行第三条语句，便可以使用简化名（即shapes）来使用模块shapes。

​		请注意，这些语句只是实例，并不用像这里做的那样，先导入包再导入其中的模块，换而言之，完全可以只是单独使用第二条或第三条语句。

## 2. 探索模块

​		介绍一些标准库模块之前，先说说如何探索这些模块，这是一个很有用的技能，因为在你的Python程序员职业生涯中，将遇到很多很有用的模块，而这里无法一一介绍。当前的标准库很大，足以编写专著来论述（市场上也的确有这样的专著），而且还在不断增大。

​		每个新Python版本都新增了模块，通常还会对一些既有模块进行细微的修改和改进。另外，你在网上肯定会找到一些有用的模块。如果能快速而轻松地理解它们，编程工作将有趣的多。

### 2.1 模块包含什么

​		要探索模块，最直接地方法就是使用Python解释器进行研究。为此，首先需要将模块导入。假设你听说有一个名为copy的标准模块。

```
>>>import copy
```

​		没有引发异常，说明的确有这样的模块。但这个模块是做什么用的？它都包含些什么？

#### 2.1.1 使用dir

​		要查明模块包含哪些东西，可使用函数dir，它列出对象的所有属性（对于模块，它列出所有的函数，类，变量等）。如果将`dir(copy)`的结果打印出来，将是一个很长的名称列表。而在这些名称中，有几个以下划线打头。根据约定，这意味着它们并非供外部使用。有鉴于此，我们使用一个简单的列表推导将这些名称过滤掉。

```powershell
>>> import copy
>>> dir(copy)
['Error', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_copy_dispatch', '_copy_immutable', '_deepcopy_atomic', '_deepcopy_dict', '_deepcopy_dispatch', '_deepcopy_list', '_deepcopy_method', '_deepcopy_tuple', '_keep_alive', '_reconstruct', 'copy', 'deepcopy', 'dispatch_table', 'error']
>>> [n for n in dir(copy) if not n.startswith('_')]
['Error', 'copy', 'deepcopy', 'dispatch_table', 'error']
```

​		如上所示，结果包含dir（copy）返回的不以下划线打头的名称，这笔完整清单要好懂的多。

#### 2.1.2 变量`__all__`

​		在前一节中，使用了简单的列表推导来猜测可在模块copy中看到哪些内容，然而可直接查询这个模块来获得正确的答案。你可能注意到了，在`dir(copy)`返回的完整清单中，包含名称`__all__`。这个变量包含一个列表，它与前面使用列表推导创建的列表类似，但是在模块内部设置的。下面看看这个列表包含的内容：

```
>>> copy.__all__
['Error', 'copy', 'deepcopy']
```

​		前面的猜测不算太离谱，只是多了几个并非供用户使用的名称。这个`__all__`列表是怎么来的呢？为何要提供他呢？第一个问题很容易解决：它是在模块copy中像下面这样设置的（以下代码是直接从`copy.py`复制而来的）。 

```python
__all__ = ['Error', 'copy', 'deepcopy']
```

​		为何要提供它呢？旨在定义模块的共有接口。具体来说，它告诉解释器从这个模块导入所有的名称意味着什么。因此，如果你使用如下代码：

```python
from copy import *
```

​		将只能得到变量`__all__`列出的3个函数。要导入`PyStringMap`，必须显式地：导入`copy`并使用`copy.PyStringMap`；或者使用`from copy import PyStringMap`。

​		编写模块时，像这样设置`__all__` _很有用。因为模块可能包含大量其他程序不需要地变量，函数和类，比较周全地做法是把它们过滤掉。如果不设置_ `__all__`，则会在以`import *`方式导入时，导入所有不以下划线打头的全局名称。

### 2.2 使用help获取帮助

​		前面一直巧妙地利用你熟悉的各种Python函数和特殊属性来探索模块copy。对于这种探索来说，交互式解释器是一个强大的工具，因为使用它来探索模块时，探索的深度仅受限于你对Python语言的掌握程度。然而，有一个标准函数可提供你通常需要的所有信息，它就是help。下面来尝试使用它获取有关copy的信息：

```powershell
>>> help(copy.copy)
Help on function copy in module copy:

copy(x)
    Shallow copy operation on arbitrary Python objects.
    
    See the module's __doc__ string for more info.

>>> 
```

​		上述帮助信息指出，函数copy只接受一个参数x，且执行的是浅复制。在帮助信息中，还提到了模块的`__doc__`字符串。`__doc__` 字符串是什么呢？还记得文档字符串么？文档字符串就是在函数开头编写的字符串，用于对函数进行说明，而函数的属性`__doc__ `可能包含这个字符串。从前面的帮助信息可知，模块也可能有文档字符串（它们位于模块的开头）而类也可能如此，即位于类的开头。

​		实际上，前面的帮助信息是从函数copy的文档字符串中提取的：

```powershell
>>> print(copy.copy.__doc__)
Shallow copy operation on arbitrary Python objects.

    See the module's __doc__ string for more info.
    
```

​		相比于直接查看文档字符串，使用help的优点是可以获取到更多信息，如函数的特征标（即它接收的参数）。请尝试对模块copy本身调用help，看看将显示哪些信息。这将打印大量的信息，包括对copy和deepcopy之间差别的详细讨论。

​		大致而言，`deepcopy(x)`创建x的属性的副本并依此类推；而`copy(x)`只复制x，并将副本的属性关联到x的属性值。

### 2.3 文档

​		显然，文档是有关模块信息的自然来源。之所以到现在才讨论文档，是因为查看模块本身要快得多。例如，你可能还想知道range的参数是什么？在这种情况下，与其在Python图书或标准Python文档中查找对range的描述，不如直接检查这个函数。

```powershell
>>> print(range.__doc__)
range(stop) -> range object
range(start, stop[, step]) -> range object

Return an object that produces a sequence of integers from start (inclusive)
to stop (exclusive) by step.  range(i, j) produces i, i+1, i+2, ..., j-1.
start defaults to 0, and stop is omitted!  range(4) produces 0, 1, 2, 3.
These are exactly the valid indices for a list of 4 elements.
When step is given, it specifies the increment (or decrement).
```

​		这样就获得了函数range的准确描述。另外，由于通常在编程时想了解函数的功能，而且此时Python解释器很可能正在运行，因此获取这些信息只需几秒钟。

​		然而，并非所有模块和函数都有详尽的文档字符串（虽然应该如此），且有时需要有关工作原理的更详尽描述。从网上下载的大多数模块都有对应的文档。就学习Python编程而言，最有用的文档是Python库参考手册，它描述了标准库中的所有模块。在需要熟悉一些有关Python的事实时，十有八九都能在这里找到。

[“Python库参考手册”]: https://docs.python.org/library	"可在线浏览和下载"

​		其他标准文档，如“Python入门指南”和“Python语言参考手册”也是如此。所有的文档都可在Python网站（https://docs.python.org）上找到。

### 2.4 使用源代码

​		在大多数情况下，前面讨论的探索技巧都够用了。但要真正理解Python语言，可能需要了解一些不阅读源代码就无法了解的事情。事实上，要学习Python，阅读源代码是除动手编写代码外的最佳方式。

​		实际上，阅读源代码应该不成问题，但源代码在哪里呢？假设你要阅读标准模块copy的代码，可以在是什么地方找到呢？一种方法是像解释器那样通过sys.path来查找，但更快捷的方式是查看模块的特性`__file__`。

```powershell
>>> print(copy.__file__)
C:\Users\AppData\Local\Programs\Python\Python36\lib\copy.py
```

​		正是如上所示，我们找到了这个.py文件的路径！这样就可以在代码编辑器（如IDLE）中打开`copy.py`，并开始研究其工作原理。如果列出的文件名以.pyc结尾，可打开以.py结尾的对应文件。

> **警告**：在文本编辑器中打开标准库文件时，存在不小心修改它的风险。这可能会破坏文件。因此关闭文件时，千万不要保存你可能对其所作的修改。

​		请注意，有些模块的源代码你完全无法读懂。它们可能时解释器的组成部分（如模块sys），还可能是使用C语言编写的。

## 3.标准库：一些深受欢迎的模块

​		在Python中，短语“**开箱即用**”（**batteries included**）最初是由**Frank Stajano**提出的，指的是Python丰富的标准库。安装Python后，你就免费获得了大量很有用的模块。鉴于有很多方式可以获取有关这些模块的详细信息，这里也不打算提供完整的参考手册，而只是描述几个作者喜欢的模块，以激发读者的探索兴趣。

### 3.1 sys

​		模块sys让你能够访问与Python解释器紧密相关的变量和函数，下表简单罗列了一些：

|  函数/变量  |                      描  述                      |
| :---------: | :----------------------------------------------: |
|    argv     |              命令行参数，包括脚本名              |
| exit([arg]) | 退出当前程序，可通过可选参数指定返回值和错误消息 |
|   modules   |        一个字典，将模块名映射到加载的模块        |
|    path     |    一个列表，包含要在其中查找模块的目录的名称    |
|  platform   |         一个平台标识符，如sunos5和win32          |
|    stdin    |        标准输入流----一个类似于文件的对象        |
|   stdout    |        标准输出流----一个类似于文件的对象        |
|   stderr    |        标准错误流----一个类似于文件的对象        |

​		变量`sys.argv`包含传递给Python解释器的参数，其中包括脚本名。

​		函数`sys.exit`退出当前程序，你还可以向它提供一个整数，指出程序是否成功，这是一种UNIX约定。在大多数情况下，使用该参数的默认值0，即成功。也可以向它提供一个字符串，这个字符串将成为错误消息，对用户找出程序终止的原因很有帮助。在这种情况下，程序退出时显示指定的错误消息以及一个表示失败的编码。

​		映射`sys.modules`将模块名映射到模块（仅限于当前已导入模块）。

​		变量`sys.path`在本章前面讨论过，它是一个字符串列表，其中都得每个字符串都是一个目录名，执行import语句时将在这些目录中查找模块。

​		变量`sys.platform`（一个字符串）是运行解释器的“平台”名称。这可能是表示操作系统的名称，也可能表示其他平台类型的名称（如Java虚拟机 java1.4.0，如果你运行的是Jython）。

​		变量`sys.stdin`, `sys.stdout`, `sys.stderr`是类似于文件的流对象，表示标准的UNIX概念：标准输入，标准输出和标注错误。简单的来说，Python从`sys.stdin`获取输入（例如用于`input`中），并将输出打印到`sys.stdout`。

​		举个例子，来看看按相反顺序打印参数的问题。从命令行调用Python脚本时，你可能指定一些参数，也就是所谓的命令行参数。这些参数将放在列表`sys.argv`中，其中`sys.argv[0]`为Python脚本名。按相反的顺序打印这些参数非常容易，如下所示：

```python
#reverseargs.py
import sys
args = sys.argv[1:]
args.reverse()
print(" ".join(args))
```

​		如你所见，我们创建了一个sys.argv的副本。也可修改`sys.argv`，但一般而言，不这样做更安全，因为程序的其他部分可能依赖于包含原始参数的`sys.argv`。另外，注意到我跳过了`sys.argv`的第一个元素，即脚本的名称。我是用`args.reverse()`反转这个列表，但不能打印这个操作的返回值，因为它就地修改列表并返回None。以下是另一种解决方案：

```
print(' '.join(reversed(sys.argv[1:])))
```

​		最后，为美化输出，这里使用了字符串的方法`join`。下面尝试运行这个程序（假设为`bash shell`）：

```bash
$ python reverseargs.py this is a test
test a is this
```

### 3.2 os

​		模块os让你能够访问多个操作系统服务。它包含的内容很多，以下会列出几个最有用的函数和变量：

|    函数/变量    |            描  述            |
| :-------------: | :--------------------------: |
|     environ     |      包含环境变量的映射      |
| system(command) | 在子shell中执行操作系统命令  |
|       sep       |      路径中使用的分隔符      |
|     pathsep     |     分割不同路径的分隔符     |
|     linesep     | 行分隔符（’\n‘,'\r','\r\n'） |
|   urandom(n)    | 返回n个字节的强加密随机数据  |

​		除此之外，os及其子模块`os.path`还包含多个查看，创建和删除目录及文件的函数，以及一些操作路径函数（例如，os.path.split和os.path.join让你在大多数情况下都可忽略`os.pathsep`）。有关这个模块的信息，可参阅标准库文档。在标准库文档中，还可以找到有关模块pathlib的描述，它提供了一个面向对象的路径操作接口。

​		映射`os.environ`包含本章前面所介绍的环境变量。例如，要访问环境变量`PYTHONPATH`，可使用表达式`os.environ('PYTHONPATH')`。这个映射也可用于修改环境变量，但并非所有的平台都支持这样做。

​		函数`os.system`用于运行外部程序。还有其他用于执行外部程序的函数，如execv和popen。前者退出Python解释器，并将控制权交给被执行的程序，而后者创建一个到程序的连接（这个连接类似于文件）。而模块subprocess则融合了模块`os.system`以及函数`execv`和`popen`功能，详情请参阅标准库文档。

​		变量`os.sep`是用于路径名的分隔符。在UNIX（或者macOS的命令行Python版本中），标准分隔符为`/`。在windows中，标准分隔符为`\\` (双反斜杠在Python语法中表示单个反斜杠)。在旧式macOS中，标准分隔符为`：`。在某些平台中，os.altsep包含代替路径分隔符，如Windows中的`/`。

​		可使用`os.pathsep`来组合多条路径，就像`PYTHONPATH`中那样。`pathsep`用于分割不同的路径名：

- UNIX/macOS中为：

- Windows中为;

​		变量`os.linesep`是用于文本文件中的行分隔符：在UNIX/OS X中为单个换行符（`\n`），在Windows中为回车和换行符（`\r\n`）。

​		函数`urandom`使用随系统而异的“真正”（至少是强加密）随机源。如果平台没有提供这样的随机源，将会引发`NotImplementedError`异常。

​		例如，看看启动Web浏览器的问题。命令system可用于执行任何外部程序，这在UNIX等环境中很有用，因为你可从命令行执行程序或命令来列出目录的内容，发送电子右键等等，它还可以启动图形用户界面程序，如Web浏览器。

​		在UNIX中，假设`/usr/bin/firefox`处有浏览器，可像下面这样做：

```python
os.system('/usr/bin/firefox')
```

​		在Windows中，可以像这样做：

```python
os.system(r'C:\"Program Files(x86)"\"Mozilla Firefox"\firefox.exe')
```

​		请注意，这里用引号将`Program Files(x86)`和`Mozilla Firefox`括起来。如果这里不这样做，底层shell将受阻于空白处（对于`PYTHONPATH`的路径，也必须这样做）。另外，这里必须使用反斜杠，因为Windows shell无法识别反斜杠。

​		如果你执行这个命令，将发现浏览器试图打开名为Files“/Mozilla”（空白后面的命令部分）的网站。另外，如果你在IDLE视图执行这个命令，将出现一个DOS窗口，待这个窗口关闭浏览器会随后启动。这样的流程看起来并不是那么理想。

​		显然，另一个函数更适合完成这项任务，它就是Windows特有的函数`os.startfile`。

```python
os.startfile(r'C:\Program Files(x86)\Mozilla Firefox\firefox.exe')
```

​		如你所见，`os.startfile`接收一个普通路径，即便该路径包含空白也没关系（无需像`os.system`示例那样用引号括起来）。请注意，在Windows中，使用`os.system`或`os.startfile`启动外部程序后，当前Python程序将继续运行；而在UNIX中，当前Python程序将等待命令os.system结束。

​		函数`os.system`可用于完成很多任务，但就启动Web浏览器这项任务而言，有一种更佳的解决方案：使用模块webbrowser。这个模块包含一个名为open的函数，让你能够启动Web浏览器并打开指定的URL。例如，要让程序在Web浏览器中打开Python官网，可以这样做：

```powershell
>>> import webbrowser
>>> webbrowser.open("https://www.python.org")
True
```

### 3.3 fileinput

​		模块fileinput让你能够轻松地迭代一系列文本文件中地所有行。使用UNIX命令行，你可以这样调用脚本：

```bash
$ python some_script.py file1.txt file2.txt file3.txt
```

​		就能够依次迭代文件file1.txt到file3.txt中的行。你还可以在UNIX管道中对使用UNIX标准命令cat提供标准输入（`sys.stdin`）的行进行迭代。