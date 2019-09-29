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

```bash
$ cat file.txt | python some_script.py
```

​		如果使用模块fileinput，在UNIX管道中使用`cat`调用脚本的效果将与以命令行参数的方式向脚本提供文件名一样，以下描述模块fileinput中最重要的函数：

|             函    数              |            描    述            |
| :-------------------------------: | :----------------------------: |
| input([files[,inplace[,backup]]]) |     帮助迭代多个输入流的行     |
|            filename()             |       返回当前文件的名称       |
|             lineno()              |     返回（累计的）当前行号     |
|           filelineno()            |     返回在当前文件中的行号     |
|           isfirstline()           | 检查当前行是否是文件中的第一行 |
|              isstdin              | 检查最后一行是否来自sys.stdin  |
|            nextfile()             |  关闭当前文件并移到下一个文件  |
|              close()              |            关闭序列            |

​		fileinput.input是其中最重要的函数，它返回一个可在for循环中进行迭代的对象。如果要覆盖默认行为（确认迭代哪些文件），可以以序列的方式向这个函数提供一个或多个文件名。还可以将参数inplace设置为True，即inplace=True，这样就地进行处理。对于你访问的每一行，都需要打印出替代内容，这些内容将被写入到当前输入文件。就地进行处理时，可选参数backup用于给从原始文件创建的备份问价指定扩展名。

​		函数fileinput.filename返回当前文件（即当前处理行的所属文件）的文件名。

​		函数fileinput.lineno返回当前行的编号。这个值是累积的，因此处理完一个文件并接着处理下一个文件时，不会重置行号，而是从前一个文件最后一个的对应行号开始计数。

​		函数fileinput.filelineno返回当前行在当前文件中的行号。每次处理完文件并接着处理下一个文件时，将重置这个行号并重新从1开始计数。

​		函数fileinput.isfirstline返回当前行是否为所述文件的第一行，是的话则返回True，否则返回False。

​		函数fileinput.isstdin在当前文件为sys.stdin时返回True，否则返回False。

​		函数fileinput.nextfile关闭当前文件并跳到下一个文件，且计数时忽略跳过的行。这在你知道无需继续处理当前文件时很有用。例如，如果每个文件包含的单词都是按照顺序排列的，而你要查找特定的单词，则过了这个单词所在的位置后，就可以放心的跳到下一个文件。

​		函数fileinput.close关闭整个文件链并结束迭代。

​		来看一个fileinput的使用示例。假设你编写了一个Python脚本，并想给其中的代码加上行号。鉴于你希望这样处理后程序依然能够正常运行，因此必须在每行末尾以注释的方式添加行号。为了让这些行号对齐，可使用字符串格式设置功能。假设只允许每行代码最多包含40个字符，并在第41个字符开始添加注释：

```python
# numberlines.py

import fileinput

for line in fileinput.input(inplace=true):
	line = line.rstrip()
	num = fileinput.lineno()
	print('{:<50} # {:2d}'.format(line,num))
```

​		如果想要运行以上这个程序，可像以下这样，直接将其本身当作参数传入：

```
python numberlines.py numberlines.py
```

​		运行成功后，程序本身就被修改了，而且如果运行多次此程序可能会在每一行都添加多个行号。

> **Tips**:其中rstrip是一个字符串方法，可删除指定字符串右边的空白并返回该字符串.

```python
# numberlines.py                                #  1
                                                #  2
import fileinput                                #  3
                                                #  4
for line in fileinput.input(inplace=True):      #  5
	line = line.rstrip()                        #  6
	num = fileinput.lineno()                    #  7
	print('{:<50} # {:2d}'.format(line,num))    #  8
```

> **警告**:务必慎用参数inplace，因为这很容易破坏文件。你可以在不设置inplace的情况下仔细测试程序(这将会只打印结果)，确保程序能够正确运行后再让它修改文件。

### 3.4 集合，堆和双端队列

​		有用的数据结构有很多。Python支持一些较常用的，其中的字典（散列表）和列表（动态数组）是Python语言的有效组成部分。还有一些虽然不怎么重要，但有时也能派上用场。

#### 3.4.1 集合

​		很久以前，集合是由模块sets中的Set类实现的。虽然在既有代码中可能遇到Set实例，但除非往后兼容，否则真的没有理由再使用它。在较新的版本中，集合是由内置类set实现的，这意味着你可以直接创建集合，而无需导入模块sets。

```powershell
>>> set(range(10))
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```

​		可使用序列（或其他可迭代对象）来创建集合，也可以使用花括号显式的指定。请注意，不能仅使用花括号来创建空集合，因为这将创建一个空字典。

```powershell
>>> type({})
<class 'dict'>
```

​		相反的，必须在不提供任何参数的情况下调用set。集合主要用于成员资格检测出。因此将忽略重复的元素。

```powershell
>>> {0,1,2,3,0,1,2,3,4,5}
{0, 1, 2, 3, 4, 5}
```

​		与字典一样，集合中元素的排列顺序是不确定的，因此不能依赖于这一点。

```powershell
>>> {'fie','fee','foe'}
{'foe', 'fee', 'fie'}
```

​		除成员资格检查外，还可执行各种标准集合操作，如交集和并集（你可能在数学课上学过），为此可使用对整数执行按位操作的运算符。例如，要计算两个集合的并集，可对其中一个集合调用方法`union`，也可使用按位或运算符`|`。

```powershell
>>> a = {1,2,3}
>>> b = {2,3,4}
>>> a.union(b)
{1, 2, 3, 4}
>>> a | b
{1, 2, 3, 4}
```

​		还有其他一些方法和对应的运算符，这些方法的名称清楚的指出其功能：

```powershell
>>> c = a & b
>>> c.issubset(a)
True
>>> c <= a
True
>>> c.issuperset(a)
False
>>> c >= a
False
>>> a.intersection(b)
{2, 3}
>>> a & b
{2, 3}
>>> a.difference(b)
{1}
>>> a - b
{1}
>>> a.symmetric_difference(b)
{1, 4}
>>> a ^ b
{1, 4}
>>> a.copy()
{1, 2, 3}
>>> a.copy() is a
False
```

​		另外，还有对应于各种就地操作的方法以及基本方法add和remove。有关于这些方法的详细信息，请参阅《Python库参考手册》中讨论集合类型的部分。

> **提示**：需要计算两个集合的并集的函数时，可使用set中方法union的未关联版本。这可能很有用，如与reduce一起使用。

```
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> my_sets = []
>>> for i in range(10):
	my_sets.append(set(range(i,i+5)))

	
>>> reduce(set.union,my_sets)
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    reduce(set.union,my_sets)
NameError: name 'reduce' is not defined
>>> from functools import reduce  # py3
>>> reduce(set.union,my_sets)
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13}
```

​		集合是可变的，因此不能作为集合中的键。另一个问题是，集合只能包含不可变（可散列）的值，因此不能包含其他集合。由于现实世界中经常会遇到集合中的集合，因此这可能是个问题。索性还有frozenset类型，它表示**不可变**(可散列)的集合。

```
>>> a = set()
>>> b = set()
>>> a.add(b)
Traceback (most recent call last):
  File "<pyshell#9>", line 1, in <module>
    a.add(b)
TypeError: unhashable type: 'set'
>>> a.add(frozenset(b))
>>> a
{frozenset()}
>>> print(a)
{frozenset()}
```

​		构造函数frozenset创建给定副本的脚本。在需要将集合作为另一个集合的成员或字典中的键时，frozenset很有用。

#### 3.4.2 堆

​		另一种著名的数据结构是**堆（heap）**，它是一种优先队列。优先队列让你能够以任意顺序添加对象，并随时（可能是在两次添加对象之间）找出（并删除）最小的元素。相比于列表方法min，这样做的效率要高得多。

​		实际上，Python没有独立的堆类型，而只有一个包含一些堆操作函数的模块。这个模块名为**heapq**（其中q表示队列），它包含六个函数，如下所示，其中前四个与堆操作直接相关，必须使用列表来操作对象本身。

|     函    数      |            描  述             |
| :---------------: | :---------------------------: |
| heappush(heap,x)  |          将x压入堆中          |
|   heappop(heap)   |     从堆中弹出最小的元素      |
|   heapify(heap)   |       让列表具备堆特征        |
|   heapreplace()   | 弹出最小的元素，并将x压入堆中 |
| nlargest(n,iter)  |    返回iter中n个最大的元素    |
| nsmallest(n,iter) |    返回iter中n个最小的元素    |

​		函数heappush用于在堆中添加一个元素。请注意，不能将它用于普通列表，而只能用于使用堆函数创建的列表。原因是元素的顺序很重要，虽然元素的排列顺序看起来有点随意，并没有严格地排序。

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> from heapq import *
>>> from random import shuffle
>>> data = list(range(10))
>>> shuffle(data)
>>> heap = []
>>> for n in data:
	heappush(heap,n)

	
>>> heap
[0, 1, 4, 2, 3, 8, 5, 9, 6, 7]
>>> heappush(heap,0.5)
>>> heap
[0, 0.5, 4, 2, 1, 8, 5, 9, 6, 7, 3]
>>> 
```

​		元素排列顺序并不像看起来那么随意。他们虽然不是严格排序地，但必须保证一点：位置i处的元素总是大于位置i//2的元素（反过来说就是小于位置2 * i和2 * i + 1处的元素）。这是底层堆算法的基础，成为堆特征。

​		函数heappop弹出最小的元素（总是位于索引0处），并确保剩余元素中最小的那个位于索引0处（保持堆特征）。虽然弹出列表中第一个元素的效率通常不是很高，但这不是问题，因为heapop会在幕后做出些巧妙的移位操作。

```powershell
>>> heappush(heap,0.5)
>>> heap
[0, 0.5, 4, 2, 1, 8, 5, 9, 6, 7, 3]
>>> heappop(heap)
0
>>> heappop(heap)
0.5
>>> heap
[1, 2, 4, 6, 3, 8, 5, 9, 7]
```

​		函数heapify通过执行尽可能少的移位操作将列表变成合法的堆（即具备堆特征）。如果你使用的堆不是使用heappush创建的，应在使用heappush和heappop之前使用这个函数。

```powershell
>>> heap_me = [5,8,0,3,6,7,9,1,4,2]
>>> heapify(heap_me)
>>> heap_me
[0, 1, 5, 3, 2, 7, 9, 8, 4, 6]
```

​		函数heapreplace用的没有其他函数那么多。它从堆中弹出最小的元素，再压入一个新元素。相比于依次执行函数heappop和heappush，这个函数效率更高。

```powershell
>>> heapreplace(heap_me,0.5)
0
>>> heap_me
[0.5, 1, 5, 3, 2, 7, 9, 8, 4, 6]
>>> heapreplace(heap_me,10)
0.5
>>> heap_me
[1, 2, 5, 3, 6, 7, 9, 8, 4, 10]
```

​		至此，模块heapq中还有两个函数没有介绍，nlargest(n,iter)和nsmallest(n,iter),分别用于找出迭代对象iter中最大和最小的n个元素。这种任务也可以通过先排序（如使用函数sorted）再切片来完成，但堆算法的速度更快，使用的内存更少（而且使用起来也更容易）。

#### 3.4.3 双端队列

​		在需要按添加元素的顺序进行删除时，双端队列很有用。在模块collections中，包含类型deque以及其他几个集合（collection）类型。

​		与集合（set）一样，双端队列也是从可迭代对象创建的，它包含多个很有用的方法。

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> from collections import deque
>>> q = deque(range(5))
>>> q.append(5)
>>> q.appendleft(6)
>>> q
deque([6, 0, 1, 2, 3, 4, 5])
>>> q.pop()
5
>>> q.popleft()
6
>>> q.rotate(3)
>>> q
deque([2, 3, 4, 0, 1])
>>> q.rotate(-1)
>>> q
deque([3, 4, 0, 1, 2])
```

​		双端队列很有用，因为它支持在队首（左端）高效地附加和弹出元素，而使用列表无法这样做。另外，还可搞笑的旋转元素（将元素向右或向左移，并在到达另一端时环绕到另一端）。而extendleft类似于appendleft。请注意extendleft的可迭代对象中的元素将按相反的顺序出现在双端队列中。

### 3.5 time

​		模块time包含用于获取当前时间，操作时间和日期，从字符串读取日期，将日期格式化为字符串的函数。日期可表示为实数（从“新纪元”1月1日0时起过去的秒数。“新纪元”是一个随平台而异的年份，在UNIX中为1970年），也可以表示为包含9个整数的元组。下表将解释这些整数。

​		例如：元组（2008，1，21，12，2，56，0，21，0）表示为2008年1月21日12时2分56秒。这一天是星期一，2008年的第21天（不考虑夏令时）。

| 索    引 |  字    段  |        值         |
| :------: | :--------: | :---------------: |
|    0     |     年     |  如2000，2001等   |
|    1     |     月     |     范围1-12      |
|    2     |     日     |     范围1-31      |
|    3     |     时     |     范围0-23      |
|    4     |     分     |     范围0-59      |
|    5     |     秒     |     范围0-61      |
|    6     |    星期    | 范围0-6【一到七】 |
|    7     |   儒略日   |     范围0-366     |
|          | 是否夏令时 |     -1，0，1      |

> **儒略日**（**Julian Day**）是在儒略周期内以连续的日数计算时间的计时法，主要是[天文学家](https://baike.baidu.com/item/天文学家/1242040)在使用。
>
> **儒略日数**（**Julian Day Number**，**JDN**）的计算是从[格林威治标准时间](https://baike.baidu.com/item/格林威治标准时间/4895434)的中午开始，包含一个整天的时间，起点的时间（0日）回溯至[儒略历](https://baike.baidu.com/item/儒略历/3052736)的公元前4713年1月1日中午12点（在[格里历](https://baike.baidu.com/item/格里历/7107947)是公元前4714年11月24日），这个日期是三种多年周期的共同起点，且是历史上最接近现代的一个起点。例如，2000年1月1日的[UT](https://baike.baidu.com/item/UT)12:00是儒略日2,451,545。

​		其中，秒的取值范围是0~61，这考虑到闰一秒和润两秒的情况。夏令时数字其实是一个布尔值（True[即1]或Flase[即0]），但如果使用-1，那么mktime[将时间元组转换为时间戳（从新纪元开始后的秒数）的函数]可能得到正确的值。下表描述了time模块的一些重要函数：

|         函    数          |                  描    述                   |
| :-----------------------: | :-----------------------------------------: |
|     asctime([tuple])      |           将时间元组转换为字符串            |
|     localtime([secs])     |     将秒数转换为表示当地时间的日期元组      |
|       mktime(tuple)       |          将时间元组转换为当地时间           |
|        sleep(spcs)        |          休眠（什么都不做）secs秒           |
| strptime(string[,format]) |           将字符串转换为时间元组            |
|          time()           | 当前时间（从新纪元开始后的秒数，以UTC为准） |

​		函数`time.asctime`将当前时间转换为字符串，如下所示：

```
>>> import time
>>> time.asctime()
'Thu Sep 26 16:06:35 2019'
```

​		如果不想使用当前时间，也可向它提供一个日期元组（如localtime创建的日期元组）。要设置更复杂的格式，可使用函数strftime，标准文档对此做了介绍。

​		函数`time.localtime`将一个实数（即从新纪元开始后的秒数）转换为日期元组（本地时间）。如果要转换为国际标准时间，应使用`gmtime`。

​		函数`time.mktime`将日期元组转换为从新纪元后的秒数，与`localtime`的功能恰好相反。

​		函数`time.sleep`让解释器等待指定的秒数。

​		函数`time.strptime`将一个字符串（其格式与`asctime`所返回字符串的格式相同）转换为日期元组。（可选参数`format`遵循的规则和`strftime`相同，详情请参阅标准文档。）

​		函数`time.time`返回当前的国际标注时间，以从新纪元开始的秒数表示。虽然新纪元随平台而异，但这样进行可靠的计时：存储事件（如函数调用）发生前后`time`的结果，再计算它们的差。

​		以上只介绍了模块`time`的一部分常用函数。这个模块的大部分函数执行的任务都与本节中介绍的任务类似或相关。如果要完成这里介绍的函数无法执行的任务，可参阅《Python库参考手册》中介绍函数time的部分，在那里你还可能找得到刚好能够完成这种任务的函数。

​		另外，还有两个较新的与事件相关的模块：`datetime`和`timeit`，前者提供了日期和时间算术支持，而后者可帮助你计算代码的执行时间。

### 3.6 random

​		模块`random`包含生成伪随机数都得函数，有助于编写模拟程序或生成随机输出的程序。请注意，虽然这些函数生成的数字好像完全随机的，但它们背后的系统是可预测的。如果你要求真正的随机（如用于加密或实现与安全相关的功能），应考虑使用模块`os`中的函数`urandom`。模块`random`中的`SystemRandom`类给予的功能与`urandom`类似，可提供接近于真实的随机数据。

​		下表罗列出这个模块中的一些重要的函数：

|            函    数            |                  描    述                  |
| :----------------------------: | :----------------------------------------: |
|            random()            |       返回一个0~1（包含）的随机实数        |
|         getrandbits(n)         |     以长整数方式返回n个随机的二进制位      |
|          uniform(a,b)          |         返回一个a~b(含)的随机实数          |
| randrange([start],stop,[step]) | 从range(start,stop,step)中随机地选择一个数 |
|          choice(seq)           |       从序列seq中随机的选择一个元素        |
|     shuffle(seq[,random])      |              就地打乱序列seq               |
|         sample(seq,n)          |    从序列seq中随机地选择n个值不同的元素    |

​		函数random.random是最基本的随机函数之一，它返回一个0~1（含）的伪随机数。除非这正是你需要的，否则可能应使用其他提供了额外功能的函数。函数random.getrandbits以一个整数的方式返回指定数量的二进制位。

​		向函数`random.uniform`提供了两个数字参数a和b时，它返回一个a~b(含)的随机（均匀分布的）实数。例如，如果你需要一个随即角度，可使用`uniform(0,360)`。

​		函数`random.randrange`是生成随机整数的标准函数。为指定这个整数所在的范围，你可像调用range那样给这个函数提供参数。例如要生成一个1~10（含）的随机整数，可使用`randrange（1，11）`或`randrange(10)+1`。要生成一个小于20的随机正奇数，可使用`randrange(1,20,1)`。

​		函数`random.choice`从给定序列中随机（均匀）地选择一个元素。

​		函数`random.shuffle`随机地打乱一个可变序列中的元素，并确保每种可能地排列顺序出现的概率相同。

​		函数`random.sample`从给定序列中随机（均匀）地选择给定数量的元素，并确保所选择元素的值各不相同。

> **注意**：编写与统计相关的程序时，可使用其他类似`uniform`的函数，它们返回按各种分布随机采集的数字，如贝塔分布，指数分布，高斯分布等。

​		来看几个使用模块`random`的实例。在这些实例中，这里将使用前面介绍的模块`time`中的几个函数。首先，获取表示时间段（2019年）上限和下限的实数。为此，可使用时间元组来表示日期（将星期，儒略日和夏令时都设置为-1，让Python去计算它们的正确值），并对这些元组调用`mktime`：

```powershell
>>> from random import *
>>> from time import *
>>> date1 = (2016,1,1,0,0,0,-1,-1,-1)
>>> date1
(2016, 1, 1, 0, 0, 0, -1, -1, -1)
>>> time1 = mktime(date1)
>>> time1
1451577600.0
>>> date2 = (2019,9,9,12,12,12,-1,-1,-1)
>>> date2
(2019, 9, 9, 12, 12, 12, -1, -1, -1)
>>> time2 = mktime(date1)
>>> time2
1568002332.0
```

​		接下来，以均匀的方式生成一个位于该范围内（不包括上限）的随机数：

```powershell
>>> random_time = uniform(time1,time2)
>>> print(asctime(localtime(random_time)))
Mon Aug  6 04:37:29 2018
```

​		在接下来的实例中，我们查询用户要投掷多少的筛子，每个筛子有多少面。掷骰子的机制是使用`randrange`和`for`循环实现。

```powershell
>>> from random import randrange
>>> num = int(input('How many dice? '))
How many dice? 3
>>> sides = int(input('How many sides per die? '))
How many sides per die? 6
>>> sum = 0
>>> for i in range(num):
	sum += randrange(sides) + 1

	
>>> print('The result is ',sum)
The result is  11
```

​		现在假设你创建了一个文本文件，其中每行都包含一种运气情况（fortune），那么就可使用前面介绍的模块`fileinput`将这些情况放在一个列表中，再随机地选择一种。

```python
# fortune.py

import fileinput,random

fortunes = list(fileinput())

print(random.choice(fortunes))

```

​		再UNIX和macOS中，可使用标准字典文件`/usr/share/dict/words`来测试这个程序，这将获得一个随机地单词。

```bash
$ python fortune.py /usr/share/dict/words
dodge
```

​		来看最后一个实例。假设你要编写一个程序，在用户每次按回车键时都发给他一张牌。另外，你还要确保发给用户地每张牌都不同。为此，首先创建‘“一副牌”，也就是一个字符串列表。

```powershell
>>> values = list(range(1,11)) + 'Jack Queen King'.split()
>>> suits = 'diamonds clubs hearts spades'.split()
>>> deck = ['{} of {}'.format(v,s) for v in values for s in suits]
>>> deck
['1 of diamonds', '1 of clubs', '1 of hearts', '1 of spades', '2 of diamonds', '2 of clubs', '2 of hearts', '2 of spades', '3 of diamonds', '3 of clubs', '3 of hearts', '3 of spades', '4 of diamonds', '4 of clubs', '4 of hearts', '4 of spades', '5 of diamonds', '5 of clubs', '5 of hearts', '5 of spades', '6 of diamonds', '6 of clubs', '6 of hearts', '6 of spades', '7 of diamonds', '7 of clubs', '7 of hearts', '7 of spades', '8 of diamonds', '8 of clubs', '8 of hearts', '8 of spades', '9 of diamonds', '9 of clubs', '9 of hearts', '9 of spades', '10 of diamonds', '10 of clubs', '10 of hearts', '10 of spades', 'Jack of diamonds', 'Jack of clubs', 'Jack of hearts', 'Jack of spades', 'Queen of diamonds', 'Queen of clubs', 'Queen of hearts', 'Queen of spades', 'King of diamonds', 'King of clubs', 'King of hearts', 'King of spades']
```

​		刚才创建的这副牌并不太适合玩游戏。不过这么看可能不太明显，可以使用pprint来查看：

```powershell
>>> from pprint import pprint
>>> pprint(deck[:12])
['1 of diamonds',
 '1 of clubs',
 '1 of hearts',
 '1 of spades',
 '2 of diamonds',
 '2 of clubs',
 '2 of hearts',
 '2 of spades',
 '3 of diamonds',
 '3 of clubs',
 '3 of hearts',
 '3 of spades']
```

​		这样可以明显的看出，四个人的牌完全一致，十分规律，以下是一种修复方法：

```powershell
>>> from random import shuffle
>>> shuffle(deck)
>>> pprint(deck[:12])
['4 of hearts',
 'King of hearts',
 'Jack of hearts',
 '6 of spades',
 '3 of clubs',
 '9 of spades',
 '8 of spades',
 'Queen of clubs',
 '7 of hearts',
 '5 of spades',
 'King of clubs',
 '4 of spades']
```

​		这里只打印了前12张牌，旨在节省篇幅。

​		最后，要让Python在用户每次按回车键时都给他发一张牌，直到牌发完为止，只需创建一个简单的while循环。如果将创建整副牌的代码放在一个程序文件中，那么只需在这个文件末尾添加如下代码即可：

```
while deck:
	input(deck.pop())
```

​		请注意，如果在交互式解释器中尝试运行这个while循环，那么每当你按回车键时都将打印一个空字符串。这是因为input返回你输入的内容，即使什么都没有，然后这些内容还会被打印出来。

​		在普通程序中，将忽略input返回的值。要在交互式解释器中也忽略input返回的值，只需要将其赋值给一个你不再理会的变量，并将这个变量命名为ignore。

### 3.7 shelve和json

​		如何将数据存储到文件中，如果需要的是简单的存储方案，模块`shelve`可替你完成大部分工作，而你只需要提供一个文件名即可。对于模块`shelve`，你唯一感兴趣的是函数`open`。这个函数将一个文件名作为参数，并返回一个shelf对象，供你用来存储数据。你可像操作普通字典那样操作它（只是键必须为字符串），操作完毕（且将所作的修改存储到设备中），可调用其方法`close`。

#### 3.7.1 一个潜在的陷阱

​		至关重要的一点是认识到shelve.open返回的对象并非普通映射，如下所示：

```powershell
>>> import shelve
>>> s = shelve.open('test.dat')
>>> s['x'] = ['a','b','c']
>>> s['x'].append('d')
>>> s['x']
['a', 'b', 'c']
```

​		嗯？`D`去哪里了？解释是这个样子的：当你查看shelf对象中的元素时，将使用存储版重建该对象，而当你将一个元素赋给键时，该元素将被存储。在上述示例中，具体发生的事情如下：

- 列表`['a','b','c']`被存储到s的`‘x’`键下

- 获取存储的表示，并使用它创建一个新列表，再将`'d'`附加到这个新列表末尾，但这个修改后的版本没有被存储。

- 最后，再次获取原来的版本，其中没有`'d'`。

  ​	要正确地修改使用模块shelve存储的对象，必须将获取的副本赋给一个临时变量，并在修改这个副本后再次存储：

  ```powershell
  >>> temp = s['x']
  >>> temp.append('d')
  >>> s['x'] = temp
  >>> s['x']
  ['a', 'b', 'c', 'd']
  ```

  ​		还有一个避免这种问题的方法：将函数open的参数writeback设置为True。这样，从shelf对象读取或赋给它的所有数据结构都保持到内存（缓存存）中，并等到你关闭shelf对象时才将它们写入硬盘中。

  ​		如果你处理的数据不多，且不想操心这些问题，将参数writeback设置为True可能是个不错的主意。在这种情况下，你必须确保在处理完毕后将shelf关闭。为此，一种方法时像处理打开的文件那样，将shelf对象作为上下文管理器。

#### 3.7.2 一个简单的数据库示例

```python
# database.py

import sys,shelve

def store_person(db):
    """
    让用户输入数据并将其存储到shelf对象中
    :param db:
    :return:
    """
    pid = input('Enter unique ID numer: ')
    person = {}
    person['name'] = input("Enter name: ")
    person['age'] = input("enter age: ")
    person['phone'] = input("Enter phone number: ")
    db[pid] = person

def lookup_person(db):
    """
    让用户输入ID和所需的字段，并从shelf对象中获取相应的数据
    :param db:
    :return:
    """
    pid = input('Enter ID number: ')
    field = input("What would you like to know ?[name/age/phone]:")
    field = field.strip().lower()

    print(field.capitalize() + " : ",db[pid][field])

def print_help():
    """
    打印帮助
    :return:
    """
    print('There available commands are: ')
    print('store:Store information about a person')
    print('lookup:Looks up a person from ID number')
    print('quit:Save change and exit')
    print('?:Prints this message')

def enter_command():
    """
    输入命令
    :return:
    """
    cmd = input("Enter command(? for help):")
    cmd = cmd.strip().lower()
    return cmd

def main():
    """
    main方法
    :return:
    """
    database = shelve.open('./database.dat')
    try:
        while True:
            cmd = enter_command()
            if cmd == 'store':
                store_person(database)
            elif cmd == 'lookup':
                lookup_person(database)
            elif cmd == '?':
                print_help()
            elif cmd == 'quit':
                return
            else:
                print_help()
    finally:
        database.close()

if __name__ == '__main__':
    main()
```

​		以上代码所示的程序有几个有趣的特征：

- 所有的代码都放在函数中，这提高了程序的结构化程度（一个可能的改进是将这些函数作为一个类的方法）。

- 主程序位于函数main中，这个函数仅在`__name__ == '__main__'`时才会被调用。这意味着可在另一个程序作为模块导入，再调用函数main。
- 在函数main中打开了一个数据库（shelf），再将其作为参数传递给其他需要它的函数。由于程序很小，我原来可以使用一个全局变量，但在大多数情况下，最好不要使用全局变量，除非你有理由这样做。
- 读入一些值后，再调用strip（去空格）和lower（转小写）方法来格式化输入内容，这样就不必提供的键和存储的键完全大小写相同且不用关注输入前后的空格。另外，注意到打印字段名时使用了`capitalize`。
- 为确保数据库得以妥善的关闭，我使用了try和finally，不知道什么时候就可能会常出现问题，进而引发异常。如果程序终止时未妥善关闭数据库，数据库文件可能受损，变得毫无价值。因此通过finally以避免这种情况发生。

我们可以去试试这个数据库。下面是一个示例交互过程。

```
Enter command(? for help):?
There available commands are: 
store:Store information about a person
lookup:Looks up a person from ID number
quit:Save change and exit
?:Prints this message
Enter command(? for help):store
Enter unique ID numer: 0
Enter name: root
enter age: 1
Enter phone number: 1
Enter command(? for help):lookup
Enter ID number: 0
What would you like to know ?[name/age/phone]:name
Name :  root
Enter command(? for help):quit
```

​		这个程序的交互过程并不是那么有意思。这里也可以使用普通字典（而不是shelf对象）来完成这个任务。退出这个程序后，假设我们之后某一时间（例如第二天）再次运行这个程序：

```
Enter command(? for help):lookup
Enter ID number: 0
What would you like to know ?[name/age/phone]:name
Name :  root
Enter command(? for help):
```

​		如你所见，这个程序会读取之前运行时创建的文件，该文件存储了之前保存的值。有兴趣的话，可以尝试能否将其扩展其他功能并让它对用户更友好。或许能设计出一个个性化版本。

> **提示**：如果要以这样的格式保存数据，也就是让其他语言编写的程序能够轻松的读取它们，可以考虑使用json格式(Python中提供了对应模块json)。

### 3.8 re

​		模块re提供了都对正则表达式的支持。如果你听说过正则表达式，就知道它有多厉害；如果没有，就等着大吃一惊吧。

​		然而，需要指出的是，要掌握正则表达式有点难。关键是每次学一点点：只考虑完成特定任务所需的知识。预先将所有的知识牢记在心中显得太浪费。

#### 3.8.1 什么是正则表达式

​		正则表达式是可以匹配文本片段的模式。最简单的正则表达式未普通字符串，与它自己匹配。换而言之，正则表达式`'python'`与字符串`'python'`匹配。你还可以使用这种匹配行为来完成如下工作：

​		在文本中查找模式，将特定的模式转换为计算得到的值，以及将文件切割成片段。

##### 3.8.1.1 通配符

​		正则表达式可与多个字符串匹配，你可使用特殊字符串来创建这种正则表达式。例如，句点与换行符外的其他字符匹配，因此正则表达式`'.ython'`与字符串`'python'`和`'jython'`都匹配，但不与`'jPython'`和`'ython'`等字符串匹配，因为句点只和一字符串匹配。

​		句点与换行符外的任何字符都匹配，因此被称为**通配符(wildcard)**。

##### 3.8.1.2 特殊字符转义

​		普通字符只和自己匹配，但特殊字符的情况完全不同。例如，假设要完全匹配字符串`'python.org'`，还可以直接时使用模式`'python.org'`么？可以是可以的，因为句点与换行符外的任何字符都匹配，所以匹配的结果可能并不是你想要的结果。要让特殊字符的行为与普通字符一样，可对其进行转义（即在前面加一个反斜杠），可使用模式`'python\\.org'`，它只和`'python.org'`匹配。

​		请注意，为表示模块re要求的单个斜杠，需要在字符串中书写两个反斜杠，让解释器对其进行转义。换而言之，这里包含两层转义：解释器执行的转义和模块re执行的转义。实际上，在某些情况下也可以使用单个反斜杠，让解释器自动对其进行转义。但最好不要这样依赖解释器，可使用原始字符串，如`r'python\.org'`。

##### 3.8.1.3 字符集

​		匹配任何字符很有用，但有时你需要更细致的额控制。为此，可以用方括号将一个子串括起，创建一个所谓的字符集。这样的字符集与其包含的字符都匹配，例如`'[pj]ython'`和`'python'`和`'jython'`都匹配，但不能与其他字符串匹配。你还可以使用范围，例如`'[a-z]'`与a~z的任意字母都匹配。

​		你还可以组合多个访问，方法是依次列出它们，例如`'[a-zA-Z0-9]'`与大写字母，小写字母和数字都匹配。请注意，字符集只能匹配一个字符。要指定排除字符集，可在开头添加一个`^`字符，例如`'[^abc]'`和除a,b,c外的其他任何字符都匹配。

> **字符集中的特殊字符**：一般而言，对于诸如句号，星号和问号等特殊字符，要在模式中将其用作普通字符而不是正则表达式运算符，必须使用反斜杠对其进行转义。在字符集中，通常无需对这些字符进行转义，但进行转义也是完全合法的。然而，你应牢记如下规则：
>
> - 脱字符(^)位于字符集开头，除非要将其作为排除运算符，否则必须对其进行转义。换而言之，除非有意为之，否则不要将其放在字符集开头。
> - 同样，对于右方括号(])和连字符(-)，要么将其放在字符集开头，要么使用反斜杠对其进行转义。实际上，如果你愿意，也可以将字符放在字符集末尾。

##### 3.8.1.4 二选一和子模式

​		需要以不同的方式处理每个字符时，字符集很好，但如果只想匹配字符串`'python'`和`'perl'`，该如何办呢？使用字符集或通配符都无法指定这样的模式，而必须使用表示二选一的特殊字符：管道字符`|`，即`'python|perl'`。

​		然而，有时候你不想将二选一运算符作用于整个模式，而只想将其用于模式的一部分。为此，可将这部分作为子模式放在圆括号内。对于之前的例子，可重写为`'p(ython|erl)'`。请注意，单个字符串也可称为**子模式**。

##### 3.8.1.5 可选模式和重复模式

​		通过在子模式后加上问号，可将其指定为可选的，即可包含可不包含。例如，下面这个不太好懂的模式：

```
r'(http://)?(www\.)?python\.org'
```

​		以上模式只与下面这些字符串匹配：

```
'http://www.python.org'
'http://python.org'
'www.python.org'
'python.org'
```

​		对于这个示例，需要注意如下几点：

- 对句点进行了转义，以防它充当通配符
- 为减少所需的反斜杠数量，这里使用了原始字符串
- 每个可选的子模式都放在圆括号内
- 每个可选的子模式都可以出现或不出现

​		问号表示可选的子模式可出现一次，也可不出现。还有其他几个运算符用于表示子模式可重复多次。

- (pattern)*：pattern可重复0，1或多次

- (pattern)+：pattern可重复1或多次。

- (pattern){m,n}：模式可重复m~n次

  ​	例如，`r'w*\.python\.org'`和`'www.python.org'`匹配，也和`'.python.org'`,`'ww.python.org'`,`'wwwwwww.python.org'`等都匹配；同样，`r'w+\.python\.org'`和`'w.python.org'`匹配，但和`'.python.org'`不匹配；而`r'w{3,4}\.python\.org'`只和`'www.python.org'`,`'wwww.python.org'`匹配。

  > **注意**：在这里，属于**匹配**指的是与整个字符串匹配，而函数match只要求模式与字符串开头匹配。

##### 3.8.1.6 字符串的开头和结尾

​		到目前为止，讨论的都是模式是否与整个字符串匹配，但也可查找与模式匹配的字串，如字符串`'www.python.org'`中的子串`'www'`与模式`'w+'`匹配。像这样查找字符串时，有时在整个字符串开头或末尾查找时很有用

​		例如，你可能想确定字符串的开头是否与模式`'ht+p'`匹配，为此可使用脱字符`('^')`来指出这一点。例如，`'^ht+p'`与`'http://python.org'`和`'htttttp://python.org'`匹配，但与`'www.http.org'`不匹配。同样，要指定字符串末尾，可使用美元符号`($)`。

> **注意**：完整的正则表达式运算符清单请参阅Python库中的Regular Expression Syntax部分。

#### 3.8.2 模块re中的内容

​		如果没有用武之地，知道如何书写正则表达式也就没什么意义。模块re包含多个使用正则表达式的函数，以下描述其中最重要的一些：

|              函    数              |                      描    述                      |
| :--------------------------------: | :------------------------------------------------: |
|      compile(pattern[,flags])      |       根据包含正则表达式的字符串创建模式对象       |
|   search(pattern,string[,flags])   |                 在字符串中查找模式                 |
|   match(pattern,string[,flags])    |                在字符串开头匹配模式                |
| split(pattern,string[,maxsplit=0]) |                根据模式来分割字符串                |
|       findall(pattern,sring)       | 返回一个列表，其中包含字符串中所有与模式匹配的子串 |
|   sub(pat,repl,string[,count=0])   |     将字符串中与模式pat匹配的子串都替换为repl      |
|           escape(string)           |    对字符串中所有的正则表达式特殊字符都进行转义    |

​		函数`re.compile`将用于字符串表示的正则表达式转换为模式对象，以提高匹配效率。调用search,match等函数时，如果提供的是用字符串表示的正则表达式，都必须在内部将它们转换为模式对象。通过使用函数compile对正则表达式进行转换后，每次使用它时都无需再进行转换。

​		模式对象也有搜索/匹配方法，因此`re.search(pat,string)`（其中pat是一个使用字符串表示的正则表达式）等价于`pat.search(string)`（其中pat是使用compile创建的模式对象）。编译后的正则表达式对象也可用于模块re中的普通函数中。

​		函数`re.search`在给定字符串中查找第一个与给定正则表达式匹配的字串。如果找到这样的子串，将返回MatchObject（结果为真），否则返回None（结果为假）。鉴于这些返回值的这种特征，可在条件语句中使用这个函数，例如：

```
if re.search(pat,string):
	print('Found it!')
```

​		然而，如果你需要获悉有关于匹配的字串的详细信息，可查看返回的MatchObject.

​		函数`re.macth`尝试在给定字符串开头查找与正则表达式匹配的子串，因此`re.match('p','python')`返回真（MatchObject），而`re.match('p','www.python.org')`返回假（None）。

> **注意**：函数match在模式与字符串开头匹配时就返回True，而不要求模式与整个字符串匹配。如果要求与整个字符串匹配，需要在模式末尾加上一个美元符号。美元符号要求与字符串末尾匹配，从而将匹配检查延申到整个字符串。

​		函数`re.split`根据与模式匹配的子串来分割字符串。这类似于字符串方法split，但使用表达式来指定分割符，而不是指定固定的分割符。例如，使用字符串方法spilt时，可以以字符串`','`为分隔符来分割字符串，但使用`re.split`时，可以以空格和逗号为分隔符来分割字符串。

```powershell
>>> import re
>>> some_text = 'alpha,beta,,,,gamma    delta'
>>> re.split('[,]+',some_text)
['alpha', 'beta', 'gamma    delta']
```

> **注意**：如果模式包含圆括号，将在分割后得到的子串之间插入括号中的内容。例如，`re.split('o(o)','foobar')`的结果为`['f','o','bar']`。

​		从这个示例可知，返回值为子串列表。参数maxsplit指定最多分割多少次：

```powershell
>>> re.split('[,]+',some_text,maxsplit=2)
['alpha', 'beta', 'gamma    delta']
>>> re.split('[,]+',some_text,maxsplit=1)
['alpha', 'beta,,,,gamma    delta']
```

​		函数`re.findall`返回一个列表，其中包含所有与给定模式匹配的子串。例如，要找出字符串包含的所有单词，可像下面这样做：

```powershell
>>> pat = '[a-zA-Z]+'
>>> text = '"Hm...Err -- are you sure?"he said,sounding insecure.'
>>> re.findall(pat,text)
['Hm', 'Err', 'are', 'you', 'sure', 'he', 'said', 'sounding', 'insecure']
```

​		要查找所有的标点符号，可像下面这样做：

```powershell
>>> pat = r'[.?\-",]+'
>>> re.findall(pat,text)
['"', '...', '--', '?"', ',', '.']
```

​		请注意，这里对连接字符`(-)`进行转义，所以Python不会认为它是用来指定指定范围的（如a-z）。

​		函数`re.sub`从左往右将与模式匹配的子串替换为指定内容。例如：

```powershell
>>> pat = '{name}'
>>> text = 'Dear {name}...'
>>> re.sub(pat,'Mr.Gumby',text)
'Dear Mr.Gumby...'
```

​		`re.escape`是一个工具函数，用于对字符串的所有可能被视为正则表达式运算符的字符进行转义。使用这个函数的情况有：

- 字符串很长且包含大量特殊字符，而你不想输入大量的反斜杠

- 你从用户哪里得到一个字符串，并将其用于正则表达式中。

  ​	下面的示例说明了这个函数的工作原理：

```powershell
>>> re.escape('www.python.org')
'www\\.python\\.org'
>>> re.escape('But where is the ambiguity?')
'But\\ where\\ is\\ the\\ ambiguity\\?'
```

> **注意**：在上述函数功能表中，有些函数接受一个名为flags的可选参数。这个参数可用于修改正则表达式的解读方式。详情请参阅“Python库参考手册”中讨论模块re的部分。

#### 3.8.3 匹配对象和编组

​		在模块re中，查找与模式匹配的子串的函数都在找回时返回MatchObject对象。这些对象包含与模式匹配的子串的信息，还包含模式的哪部分与子串的哪部分匹配的信息。这些子串部分常称为**编组(group)**。

​		编组就是放在圆括号内的子模式，它们是根据左边的括号数编号的，其中编组0指的是整个模式。因此，在下面的模式中：

```
There (was a (wee)(cooper)) who (lived in Fyfe)
```

​		包含如下编组：

```
0 There was a wee cooper who lived in Fyfe
1 was a wee cooper
2 wee
3 cooper
4 lived in Fyfe
```

​		通常，编组包含诸如通配符和重复运算符等特殊字符，因此你可能想知道与给定编组匹配的内容。例如，在下面的模式中：

```
r'www\.(.+)\.com$'
```

​		编组0包含整个字符串，而编组1包含`www.`和`'.com'`之间的内容。通过创建类似于这样的模式，可提取字符串中你感兴趣的部分。

​		下表中描述了re匹配对象的一些重要方法：

|       方    法        |                           描    述                           |
| :-------------------: | :----------------------------------------------------------: |
| group（[group1,...]） |              获取与给定字符串（编组）匹配的子串              |
|    start([group])     |              返回与给定编组匹配的子串的起始位置              |
|     end([group])      | 返回与给定编组匹配的子串的终止位置（和切片一样，不包含终止位置） |
|     span([group])     |           返回与给定编组匹配的子串的起始和终止位置           |

​		方法`group`返回与模式中给定编组匹配的子串。如果没有指定编组号，则默认为0.如果只指定一个编组号（或使用默认值0），将只返回一个字符串，否则返回一个元组，其中包含与给定编组匹配的子串。

> **注意**：除整个模式（编组0）外，最多还可以有99个编组，编号为1~99。

​		方法`start`返回与给定编组（默认0即整个模式）匹配的子串的起始索引。

​		方法`end`类似于start，但返回终止索引加1。

​		方法`span`返回一个元组，其中包含与给定编组（默认为0，即整个模式）匹配的子串的起始索引和终止索引。

​		下面的示例说明了这些方法的工作原理：

```powershell
>>> m = re.match(r'www\.(.*)\..{3}','www.python.org')
>>> m.group(1)
'python'
>>> m.start(1)
4
>>> m.end(1)
10
>>> m.span(1)
(4, 10)
```

#### 3.8.4 替换中的组号和函数

​		在第一个`re.sub`使用示例中，我只是将一个子串替换为另一个。这也可使用字符串方法replace轻松完成。当然，正则表达式很有用，因为它们让你能够以更灵活的方式进行搜索，还能让你执行更复杂的替换。

​		为利用re.sub的强大功能，最简单的方式是替代字符串中使用组号。在替换字符串中，任何类似于`'\\n'`的转义序列都将被替换为与模式中编组n匹配的字符串。例如，假设要将`'*something*'`替换为`'<em>something</em>'`,其中前者是在纯文本文档（如电子邮件）中表示突出的普通方式，而后者是相应的HTML代码（用于网页中）。下面先创建一个正则表达式：

```powershell
>>> emphasis_pattern = r'\*([^\*]+)\*'
```

​		请注意，正则表达式容易变得难以理解，因此为方便其他人（也包括你自己）以后阅读代码，使用更有意义的变量名很重要。

> **提示**：要让正则表达式更容易理解，一种方法是在调用模块re中的函数时使用标志`VERBOSE`。这让你能够在模式中添加空白（空格，制表符，换行符等），而`re`将忽略它们，除非将它放在字符类中或使用反斜杠对其进行转义。在这样的正则表达式中，你还可以添加注释。下述代码创建的模式对象与`emphasis_pattern`等价，但是用了`VERBOSE`标志。
>
> ```powershell
> >>> emphasis_pattern = re.compile(r'''
> 	\*	#起始突出标志，一个星号
> 	(       #与要突出的内容匹配的编组的起始位置
> 	[^\*]+  #与除星号外的其他字符都匹配
> 	)       #编组到此结束
> 	\*	#结束突出标志
> 	''',re.VERBOSE)
> ```

​		创建模式后，就可使用re.sub来完成所需的替换了。

```powershell
>>> re.sub(emphasis_pattern,r'<em>\1</em>','Hello,*world*!')
'Hello,<em>world</em>!'
```

​		如你所见，成功的将纯文本转换为HTML代码。

​		然而，通过将函数用作替换内容，可执行更复杂的替换.这个函数将MatchObject作为唯一的参数，它返回的字符串将用作替换内容。换而言之，你可以对匹配的字符串做任何处理，并通过细致的处理来生成替换内容。你可能会问，这有何用途呢？等你开始尝试使用正则表达式后，将发现这种机制的用途非常多，随后将会介绍一个。

#### 3.8.5 贪婪和非贪婪模式

​		重复运算符默认是贪婪的，这意味着它们将匹配尽可能多的内容。例如，假设重写了前面的突出程序，在其中使用了如下模式：

```powershell
>>> emphasis_pattern = r'\*(.+)\*'
```

​		这个模式与以星号打头和结尾的内容匹配。好像很完美，不是么？但情况并非如此：

```powershell
>>> re.sub(emphasis_pattern,r'<em></em>','*This* is *it*!')
'<em></em>!'
```

​		如你所见，这个模式匹配了从第一个星号到最后一个星号的全部内容，其中包含另外两个星号！这就是贪婪的意思：能匹配多少就匹配的多少。

​		在这里，你想要的显然不是这种过度贪婪的行为。在你不知道不应该将某个特定的字符包含在内时，本章前面的解决方案（使用一个匹配任何非星号字符的字符集）很好。下面再看另一个场景：如果使用`'**something**'`来表示突出呢？在这种情况下，在要强调的内容中包含单个星号不是问题，但如何避免这种过度贪婪的匹配呢？

​		这实际上很容易，只需使用重复运算符的非贪婪版即可。对于所有的重复运算符，都可在后面加上问号来将其指定为非贪婪的。

```powershell
>>> emphasis_pattern = r'\*\*(.+?)\*\*'
>>> re.sub(emphasis_pattern,r'<em>\1</em>','**This** is **it**!')
'<em>This</em> is <em>it</em>!'
```

​		这里使用的是运算符`+?`而不是`+`。这意味着与以前一样，这个模式将匹配一个或多个通配符。但匹配尽可能少的内容，因为它是非贪婪的。因此，这个模式只匹配到下一个`'\*\*'`，即它末尾的内容。如你所见，效果很好。

#### 3.8.6 找出发件人

​		你曾将邮件保存为文本文件么？如果你这样做过，你可能注意到文件开头有大量难以理解的文本。

```
Received: from 163.com (unknown [XXX.XXX.XXX.XXX])
	by newmx33.qq.com (NewMx) with SMTP id 
	for <***@qq.com>; Fri, 27 Sep 2019 17:52:41 +0800
X-QQ-DNTY: 1
X-QQ-FEAT: OE6UcXCtsg3T8bzOktLmQo8soOtS6r78hQr7t+nNE57t4pbZFqU0TGZS/6ssJ
	JPL/MXSzHjMPDpnJaYqxUHypIy90itrKHSquYmNGmeR3uUtG/R3ML69ON12qIq3uG16ej8T
	Mjlw0MA5G1EmvuOaT3dHVCPHOcG7yVoFITEHU2FoRflobWZjHURK2d4atsuUmmbf+4DyQYi
	2+0JItBcfNmcGM4VNFE8P+hu+DYR6+nw0dtLtIqeHz+GIjpcthlZcnNfV6K6tJ+dhxMfevz
	MtJHxt0TYPQRYNm4MrMbyutbo2Bf1NX01qCtQLBl8UFtwxVDGACYEmNUPnf6XUtPMgkA==
X-QQ-MAILINFO: MfiUJyQXz1c0DZs15QYtNqC2jHC1NUvP3W/ZWvxfPFFd21JFDShWFyWem
	sBIByHE/G/51LofGclr46pHxUsLw2MljHc09ViOWZSOzyYgWWJJUETUhG5VRQ3DtainFgbI
	pJpxC+spxL+JAwdUbrExa2oBa3Sg90W3s0d/HAW3VhOicBSstS/1KCgHcy+7Bwu+nWshVXR
	VnZ3l
X-QQ-mid: mx33t1569599961tjihu999w
X-QQ-ORGSender: ******@163.com
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed; d=163.com;
	s=s110536; h=Date:From:Subject:MIME-Version:Message-ID; bh=7hPFV
	qtHV+8c8T88qa1dQHC8U0F8hEOtZX9+gFzGvA0=; b=n+rHqdHSp6YzwxX7JoHJ1
	WTOEJ2UaPQ0nu8eNLCRn4VoQ4GG8jecqd+j8JnsLukvWIxu0Xma3112+NyauUqvy
	rujSl2zyFIoXWSDl4s4gbzfdt/xaAx8nyEroqE+oB8kUqta5x4AEYGg/0PSHjqRS
	NqqAtzX/FyNWL3ky/0YkLc=
Received: from ******$163.com ( [XXX.XXX.XXX.XXX] ) by
 ajax-webmail-wmsvr209 (Coremail) ; Fri, 27 Sep 2019 17:52:41 +0800
 (GMT+08:00)
X-Originating-IP: [XXX.XXX.XXX.XXX]
Date: Fri, 27 Sep 2019 17:52:41 +0800 (GMT+08:00)
From: =?GBK?B?xMHQxw==?= <******@163.com>
To: "***@qq.com" <***@qq.com>
Subject: Hello
```

​		我们来尝试找出这个邮件的发件人。如果你仔细查看上面的文本，肯定能找出发件人（尤其是看到邮件末尾的签名时）。但你能找出其中普适的规律呢？如何提取发件人姓名（不包含邮件地址）呢？如何列出邮件头中提及的所有邮件地址呢？先来解决第一个内容。

​		以上包含发件人邮箱的位置有多个，最具代表性的是`From: =?GBK?B?xMHQxw==?= <******@163.com>`，即以From开头，并以尖括号包含邮件地址结尾，我们需要提取的是这两部分之间的文本。如果使用模块fileinput，这个任务很容易完成。

​		可像以下这样运行程序（假设电子邮件被保存在文本文件`Hello_.eml`）

```powershell
>>>python find_sender.py Hello_.eml
 =?GBK?B?xMHQxw==?=
```

​		如上所示，这里的发件人是一串乱码，因为发件人名称为中文，QQ邮箱对其进行过处理，这里只探讨正则，不进行进一步处理。

> **注意**：如果你愿意，也可以在不使用正则表达式的情况下解决这个问题。还可以使用email来解决这个问题
>
> ```python
> # find_sender2.py
> 
> import email
> 
> # 打开文件
> fp = open('./Hello_.eml', "r")
> # 创建消息对象
> msg = email.message_from_file(fp)
> # 获取发件人
> print(email.utils.parseaddr(msg.get("from"))[0])
> # 获取发件邮箱
> print(email.utils.parseaddr(msg.get("from"))[1])
> ```
>
> ​		运行效果如下所示：
>
> ```powershell
> >>>python find_sender2.py
> =?GBK?B?xMHQxw==?=
> ******@163.com
> ```

​		对于这个程序，应注意以下几点：

- 为提高处理效率，这里预先编译了正则表达式

- 将用于匹配要提取文本的子模式置于圆括号中，使其成为一个编组

- 使用了一个非贪婪模式，使其只匹配最后一对尖括号

- 使用美元符号指出要使用这个模式来匹配整行（直至行尾）

- 使用了if语句来确保匹配后才提取与特定编组匹配的内容

  ​	要列出邮件头中提及的所有邮件地址，需要创建一个只与邮件地址匹配的正则表达式，然后使用方法`findall`找出所有与之匹配的内容。为避免重复，可将邮件地址保存在之前介绍的集合中。最后，提取键，将他们排序并打印出来，请注意，排序时大写字母在小写字母之前。

```python
# find_address.py

import fileinput,re
# 这里仅匹配英文邮箱，例如Mr.Gumby@bar.baz
pat = re.compile(r'[a-zA-Z\-\.]+@[a-zA-Z\-\.]+',re.IGNORECASE)
addresses = set()
for line in fileinput.input():
	for address in pat.findall(line) :
		addresses.add(address)
for address in sorted(addresses):
	print(address)
```

> **注意**：这里并没有完全按照要求做。问题要求找出邮件头中的地址，但这个程序显然是找出了文件中所有的邮件地址（提示：这里指仅为英文且认为其符合一般邮件地址对反的所有字符串，并不一定包含所有邮箱，且包含的可能不止是邮箱）。为避免这一点，可在遇到空行后调用`fileinput.close()`，因为邮件头不可能包含空行。如果有多个文件，也可在遇到空行后调用`fileinput.nextfile()`来处理下一个文件。

#### 3.8.7 模板系统示例

​		**模板(template)**是一种文件，可在此处插入具体的值来得到最终的文本。例如，可能有一个只需要插入收件人姓名的邮件模板。Python提供了一种高级模板机制：字符串格式化设置。使用正则表达式可让这个系统更加高级。假设要把所有的`[something]`(字段)都替换为将something作为Python表达式计算得到的结果。因此，下面的字符串：

```
'The sum of 7 and 9 is [7 + 9].'
```

​		因转换为：

```
'The sum of 7 and 9 is 16.'
```

​		另外，你还希望能够在字段中进行赋值，使得下面的字符串：

```
'[name="Mr.Gumby"]Hello,[name]'
```

​		转换为：

```
'Hello,Mr.Gumby'
```

​		这看起来有点难以理解，我们来看看可供使用的工具：

- 可使用正则表达式来匹配字段并提取其内容
- 可使用`eval`来计算表达式字符串，并提供包含作用域的字典。可在`try/except`语句中执行这种操作。如果出现`SyntaxError`异常，就说明你处理的可能是语句（如赋值语句）而不是表达式，应使用`exec`来执行它
- 可使用exec来执行语句字符串（和其他语句），并将模板的作用域存储到字典
- 可使用re.sub将被处理的字符串替换为计算得到的结果。突然间，这看起来并不那么吓人，不是么？

> **提示**：如果任务看起来吓人，将其分解为较小的部分几乎总是大有脾益。另外，要对手头的工作进行评估，确定如何解决面临的问题。

```python
# template.py

import fileinput,re

# 与使用方括号括起的字段匹配
field_pat = re.compile(r'\[(.+?)\]')

# 我们将把变量收集到这里
scope = {}

# 用于调用re.sub
def replacement(match):
    code = match.group(1)
    try:
        # 如果字段为表达式，就返回其结果
        return str(eval(code,scope))
    except SyntaxError:
        # 否则在当前作用域内执行该赋值语句
		# python 2.x写法
        #exec code in scope
        #python 3.x写法
        exec(code,scope)
        # 并返回一个空字符串
        return ''

# 获取所有文本合并成一个字符串

# （还可采用其他办法完成这项任务）
lines = []
for line in fileinput.input():
    lines.append(line)
    
text = ''.join(lines)

print(field_pat.sub(replacement,text))
```

​		简而言之，这个程序做了如下事情：

- 定义一个用于匹配字符串的模式
- 创建一个用作模板作用域的字典
- 创建一个替换函数，其功能如下：
  - 从match中获取与编组1匹配的内容，并将其存储到变量code中
  - 将作用域字典作为命名空间，并尝试计算code，在将结果转换为字符串并返回它。如果成功，就说明这恶鬼字段是表达式，因此万事大吉，否则（即引发SyntaxError异常），就进入了下一步
  - 在对表达式进行求值时使用的命名空间（作用域字典）中来执行这个字段，并返回一个空字符串（因为赋值语句并没有结果）

- 使用fileinput读取所有行，并将它们放在一个列表中，在将其合并成一个大型字符串
- 调用re.sub来使用替换函数来替换所有与模式field_pat匹配的字段，并将结果打印出来。

> **注意**：在以前的Python版本中，相比于下面的说法，将文本行放在一个列表中再合并的效率要高得多：
>
> ```python
> text = ''
> for line in fileinput.input():
> 	text += line
> ```
>
> ​		上述代码虽然看起来很优雅，但每次赋值都将创建一个新的字符串（在原有字符串后面附加新字符串）。这可能很浪费资源，导致程序运行缓慢。在较旧的Python版本中，这种做法与使用`join`的区别可能很大；而在较新的版本中，使用运算符`+=`的速度可能更快。如果性能很重要，可以尝试这两种方案。

​		只用15行代码（不包含空白和注释），就创建了一个强大的模板系统。但愿你已经认识到，通过使用标准库，Python的功能变得非常强大。为结束这个示例，下面测试一下这个模板系统：

```
# 模板文件
[x=2]
[y=3]
The sum of [x] and [y] is [x + y]
# 预期输出
The sum of 2 and 3 is 5
```

​		别急，你还可以做得更好！由于使用了fileinput，因此可以处理多个文件。这意味着可以使用一个文件来定义变量的值，而使用另一个文件用作模板，以便在其中插入这些值。例如，可能有一个包含定义的文件（magnus.txt），还有一个模板文件（template.txt）：

```
# 模板定义文件
[name = 'Magnus Lie Hetland']
[email = 'magnus@foo.bar']
[language = 'python']
# 模板文件
[import time]
Dear [name],
	I would like to learn how to program.T hear you use the [language] language a lot -- is it something I should consider?
	And,by the way,is [email] you correct email address?
	Fooville,[time.asctime()]
	Oscar Frozzbozz
```

​		`import time`并非赋值语句（而是用于做准备工作的语句），但由于程序没那么挑剔（使用了一条简单的try/except语句），它支持任何使用eval和exec进行处理的表达式和语句。可像下面这样运行这个程序(这里为windows命令行)：

```powershell
>>>python template.py magnus.txt template.txt
```

​		你将看到类似于以下这样的输出：

```



Dear Magnus Lie Hetland,
        I would like to learn how to program.T hear you use the python language a lot -- is it something I should consider?
        And,by the way,is magnus@foo.bar you correct email address?
        Fooville,Sun Sep 29 12:26:33 2019
        Oscar Frozzbozz

```

​		虽然这个模板系统能够执行非常复杂的替换，但也存在一些缺陷。例如，如果能够以更灵活的方式编写定义文件就好了。如果使用`execfile`来执行它，就可使用普通Python语法了。这样还能修复输出开头包含空行的问题。

​		你还能想到其他改进这个程序的方式么？对于这个程序使用的概念，你还能想到它们的其他用途么？无论要精通哪种编程语言，最佳的方式都是尝试它 -- 找出其局限性和长处。看看你能不能重写这个程序，并让它做的更好，满足你的需求。

### 3.9 其他有趣的标准模块

​		虽然这里介绍模块内容很多，但这只是标准库的冰山一角。下面简单介绍一下其他几个很棒的库，有兴趣可以了解一下。

- `argprase`：在UNIX中运行命令行程序时常常需要指定各种选项（开关），Python解释器就是这样的典范。这些选项都包含在sys.argv中，但要正确地处理它们并不容易。而模块`argprase`使得提供功能齐备的命令行界面易如反掌。
- `cmd`：这个模块让你能编写类似于Python交互式解释器地命令行解释器。你可定义命令，让用户能够在提示符下执行它们。或许还可以使用这个模块为你编写的程序提供用户界面。
- `csv`：CSV指的是逗号分隔的值（comma-seperated values），很多应用程序（如很多电子表格程序和数据库程序）都使用这种简单格式来存储表格数据。这种格式主要用于在不同的程序之间交换数据。模块csv让你能够轻松地编写CSV文件，它还能以非常透明地方式处理CSV格式的一些棘手问题。
- `datetime`：如果模块`time`不能满足你对时间的跟踪需求，模块`datetime`很可能能够满足需求。`datetime`支持特殊的日期和期间对象，并让你能够以各种方式创建和合并这些对象。相比于模块`time`，模块`datetime`的接口在很多方面都更加直观。
- `difflab`：这个库让你能够确定两个序列的相似程度，还能让你从很多序列中找出于指定序列最为相似的序列。例如，可使用`difflab`来创建简单的搜索程序。
- `enum`：枚举类型是一种只有少数几个可能取值的类型。很多语言都内置了这样的类型，如果你在使用Python时需要这样的类型，模块`enum`可提供极大的帮助。
- `functools`：这个模块提供的功能是，让你能够在调用函数时只提供部分参数（部分求值，partial evaluation），以后再填充其他的参数。在Python3.X中，这个模块包含`filter`和`reduce`。
- `hashlab`：使用这个模块可计算字符串的小型“签名”（数）。计算两个不同字符串的签名时，几乎可以肯定得到的两个签名是不同的。你可使用它来计算大型文本文件的签名。这个模块在加密和安全领域有很多用途。
- `itertools`：包含了大量用于创建和合并迭代器（或其他可迭代对象）的工具。其中包括可以串接可迭代对象，创建返回无限连续整数的迭代器（类似于range但没有上限），反复遍历可迭代对象以及具有其他对象的函数。
- `logging`：使用print语句来确认程序中发生的情况很有用。要避免跟踪时出现大量的调试输出，可将这些信息写入日志文件中。这个模块提供了一系列标准工具，可用于管理一个或多个中央日志，它还支持多种优先级不同的日志消息。
- `statistics`：计算一组数的平均值并不那么难，但是要正确地获取中位数，以确定总体标准偏差和样本标准偏差之间地差别，即使对于偶数个元素来说，也需要费点心思，在这种情况下，不要手工计算，而应使用模块`statistics`。
- `timeit`，`profile`和`trace`：模块`timeit`（和配套地命令行脚本）是一个测试代码段执行时间地工具。这个模块暗藏玄机，度量性能时你可能应该使用它而不是模块`time`。模块`profile`（和配套模块`pstats`）可用于对代码段的效率进行更全面地分析。模块`trace`可帮助你进行覆盖率分析（即代码地哪些部分执行了，哪些部分没有执行），这在编写测试代码时很有用。