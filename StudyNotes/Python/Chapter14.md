# 第十四章：抽象和对象

​		Python从设计之初就已经是一门面向对象的语言，正因为如此，在Python中创建一个类和对象是很容易的。本章节我们将详细介绍Python的面向对象编程，以及如何创建自定义对象。 

​		如果你以前没有接触过面向对象的编程语言，那你可能需要先了解一些面向对象语言的一些基本特征，在头脑里头形成一个基本的面向对象的概念，这样有助于你更容易的学习Python的面向对象编程。 

## 1. 面向对象技术简介

​		在面向对象编程中，**对象**大致意味着一系列数据（属性）以及一套访问和操作这些数据的方法。

- **类(Class):** 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。
- **方法：**类中定义的函数。
- **类变量：**类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。
- **数据成员：**类变量或者实例变量用于处理类及其实例对象的相关的数据。
- **方法重写：**如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。
- **局部变量：**定义在方法中的变量，只作用于当前实例的类。
- **实例变量：**在类的声明中，属性是用变量来表示的。这种变量就称为实例变量，是在类声明的内部但是在类的其他成员方法之外声明的。
- **继承：**即一个派生类（derived class）继承基类（base class）的字段和方法。继承也允许把一个派生类的对象作为一个基类对象对待。例如，有这样一个设计：一个Dog类型的对象派生自Animal类，这是模拟"是一个（is-a）"关系（例图，Dog是一个Animal）。
- **实例化：**创建一个类的实例，类的具体对象。
- **对象：**通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。

和其它编程语言相比，Python 在尽可能不增加新的语法和语义的情况下加入了类机制。

​		Python中的类提供了面向对象编程的所有基本功能：类的继承机制允许多个基类，派生类可以覆盖基类中的任何方法，方法中可以调用基类中的同名方法。 

​		对象可以包含任意数量和类型的数据。 

​		使用对象而非全局变量和函数的原因有多个，下面列出了使用对象最重要的好处：

- 多态：可对不同类型的对象执行相同的操作，而这些操作就像“被施了魔法”一样能够正常运行。

- 封装：对外界隐藏有关对象工作原理的细节

- 继承：可基于通用类创建出专用类


​		在很多介绍面向对象编程的资料中，都以不同于这里的顺序介绍上述概念。一般先介绍封装和继承，再使用这些概念来模拟现实世界的对象。这没什么不好，但是在我看来，多态才是面向对象编程最有趣的事情。而一般而言，这也是让大多数人感到迷惑的特性。

## 2. 多态

​		**多态**源自于希腊语，意思是“有多种形态”。这大致意味着你即使不知道变量指向的是那种对象，也能对齐执行操作，且操作的行为将随对象所属的具体类型（类）而异。

​		一个简单的查询价格可能根据不同的查询方式和实现方法而重新设计实现，这样可能会开发多次，甚至推翻之前已有的代码。如何避免这样的问题？在面向对象编程中，我们使用多态来解决，即让对象自己处理对应的操作。无论有多少种新对象，都能够正确的获取并计算其价格返回，而用户只需要向它查询价格即可，这正是多态的用武之地。

​		你收到一个对象，却根本不知道它是如何实现的-- --他可能是众多“形态”中的任何一种。如果你知道它可以查询价格，这就够了，像这样与对象属性相关联的函数又被称为方法。

​		下面来做一个实验。标准库模块`random`中包含一个`choice`的函数，它从序列中随机选择一个元素：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> from random import choice
>>> arr = ['hello,world!',[1,2,'e','e','e']]
>>> choice(arr).count('e')
3
>>> choice(arr).count('e')
1
>>> 
```

​		以上例子表明，你无需关心随机选取的是一个什么类型的数据，你可以只关心它之中到底有几个‘e’即可，为了这一目的，你可以调count，即可得到你要的答案。

​		当你无需知道对象是什么样的就能对其执行操作，就是多态在起作用。这不仅适用于方法，我们还通过内置运算符和函数大量使用了多态。例如：

```powershell
>>> 1 + 1
2
>>> 'a ' + 'b'
'a b'
```

​		以上代码表明，加法运算符既可用于数，也可用于字符串，以及其他类型的序列。加法运算符两边的值可以是任何支持加法的对象。如果要编写一个函数，通过打印一条消息来指定对象长度，你可以这么做：

```powershell
>>> def length_message(x):
	print("The length of " + repr(x),' is ',len(x))


```

​		如你所见，这个函数还使用了`repr`。`repr`是Python中多态的集大成者之一，可以用于任对象，下面就来看看：

```powershell
>>> length_message("Fnord")
The length of 'Fnord'  is  5
>>> length_message([1,2,3])
The length of [1, 2, 3]  is  3
```

​		这里讨论的多态形式是Python编程方式的核心，有时也被称为**鸭子模型**，这个术语源自于如下说法：“如果走起来像鸭子，叫起来像鸭子，那么它就是鸭子。”

## 3.封装

​		**封装**（encapsulation）指的是向外部隐藏不必要的细节。这听起来很像多态（无需知道对象内部的细节就可以使用它）。这两个对象很像，因为它们都是抽象的原则。它们都像函数一样，可以帮助你处理程序的组成部分，让你无需关心不必要的细节。

​		但封装不同于多态。多态让你无需知道对象所属的类（对象的类型）就能调用其方法，而封装让你无需知道对象的构造就能使用它。如果还是不明白，还是来一个例子吧，假设你有一个名为OpenObject的类：

```powershell
>>>o = OpenObject()	#对象就是这么建立的
>>>o.set_name("Sir Lacelot")
>>>o.get_name()
'Sir Lacelot'
```

​		你（通过像调用对象一样调用类）创建一个对象，并将其关联到变量o,然后就可以使用方法`set_name`和`get_name`了（假设OpenObject中支持这些方法）。一切都看起来完美无缺。然而，如果将其名称存储在全局变量`global_name`中呢？

```powershell
>>> global_name
"Sir Lacelot"
```

​		这意味着使用OpenObject类的实例（对象）时，你需要考虑`global_name`的内容，事实上，必须确保无人能修改它。

```powershell
>>>global_name = 'Sir Gumby'
>>>o.get_name()
'Sir Gumby'
```

​		如果尝试创建多个OpenObject对象，将会出现问题，因为它们会公用一个变量：

```powershell
>>>o1 = OpenObject()
>>>o2 = OpenObjcet()
>>>o1.set_name('Robin Hood')
>>>o2.get_name()
'Robin Hood'
```

​		如你所见，修改一个对象的名称时，将自动设置另一个对象的名称。这可能并不是你想要的结果。基本上，你希望对象是抽象的：当调用方法时，无需操心其他的事情，如避免干扰全局变量。如何将名称“封装”在对象中？没问题，将其作为一个**属性**即可。

​		属性是归属于对象的变量，就像方法一样。实际上，方法差不多就是与函数相关联的属性。如果你使用属性而非全局变量重新编写前面的类，并将其命名为CloseObject，就可以像下面这样使用它：

```powershell
>>> c = CloseObject()
>>> c.set_name('Sir Lancelot')
>>> c.get_name()
'Sir Lancelot'
```

​		到目前为止一切顺利，但这并不能证明名称不是存储在全局变量中的。下面再来创建一个对象：

```powershell
>>> r = CloseObject()
>>> r.set_name('Sir Robin')
>>> r.get_name()
'Sir Robin'
```

​		从中可知正确的设置了新对象的名称（这可能会在你意料之中），但第一个对象现在会怎么呢？

```powershell
>>>c.get_name()
'Sir Lancelot'
```

​		其名称还在，因为这个对象有自己的状态。对象的状态由属性（如名称）描述。对象的方法可能修改这些属性，因此将一系列函数（方法）组合起来，并赋予他们访问一些变量（属性）的权限，而属性可用于两次函数调用之间存储值。

## 4.继承

​		Python 同样支持类的继承，如果一种语言不支持继承，类就没有什么意义。

​		继承另一种偷懒的方式（褒义）。程序员总是想避免多次输入同样的代码。本书前面通过创建函数来达成这个目标，但是现在要解决一个更微妙的问题。如果你已经有一个类，并且要创建一个与之很像的类（例如只是新增了几个方法），该怎么办？创建这个新类时，你不想复制旧类的代码再粘贴到新类中。

​		例如，你可能已经有了一个名为`Shape`的类，它知道如何将自己绘制到屏幕。现在你想创建一个名为`Rectangle`的类，但他不仅知道如何将自己绘制到屏幕上，而且还知道如何计算其面积。你不想重新编辑绘制方法`draw`，因为`Shape`中已经有一个这样的方法了，而且效果很好。那么能怎么办？让`Rectangle`继承`Shape`的方法，使得对`Rectangle`调用方法`draw`时，将会自动调用`Shape`父类中的对应方法。

​		派生类的定义如下所示:

```
class DerivedClassName(BaseClassName1):
    <statement-1>
    .
    .
    .
    <statement-N>
```

​		需要注意圆括号中基类的顺序，若是基类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找基类中是否包含方法。

​		BaseClassName（示例中的基类名）必须与派生类定义在一个作用域内。除了类，还可以用表达式，基类定义在另一个模块中时这一点非常有用: 

```
class DerivedClassName(modname.BaseClassName):
```

```python
#!/usr/bin/python3
 
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 
 
s = student('ken',10,60,3)
s.speak()
```

​		执行以上程序输出结果为：

```
ken 说: 我 10 岁了，我在读 3 年级
```

​		要确认一个类是否是另一个类的子类，可使用内置方法`issubcclass`。

​		如果你有一个类，并想知道它的基类，可访问其特殊属性`__bases__`。

​		同样，要确认对象是否是某特定类的实例，可使用`insinstance`。

​		如果你要获悉对象属于哪个类，可使用属性`__class__`。

## 5. 多继承

​		Python同样有限的支持多继承形式。多重继承是一个功能强大的工具，然而，不到万不得已，都应避免使用多继承，因为在有些情况下，他可能会带来意外的“并发症”。使用多继承时，有一点务必要注意：如果多个超类以不同的方式实现了同一方法（即有多个同名方法），必须在class语句中小心排列这些超类，因为位于前面的类的方法将覆盖位于后面的类的方法。多继承的类定义形如下例:

```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

​		需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

```python
#!/usr/bin/python3
 
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
#单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
#另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
 
#多重继承
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
 
test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中排前地父类的方法
```

​		执行以上程序输出结果为：

```
我叫 Tim，我是一个演说家，我演讲的主题是 Python
```

## 6. 类

​		至此，你对类是什么应该有了大体的感觉，还可能有点急不可耐，希望我马上介绍如何创建类。至此之前，我们先了解以下什么是类？

​		本书前面反复提到了类，并将其做类型的同义词。从很多方面来说，这就是类的定义--一个对象。每个对象都属于特定的类，并被称为该类的实例。

​		例如，你在窗外看到一只鸟，这只鸟就是“鸟类”的一个实例。鸟类是一个非常通用（抽象）的类，他有很多子类：你看到的那只鸟可能属于子类“云雀”。你可将“鸟类”视为由所有鸟组成的集合，而“云雀”是其中的一个子集。一个类的对象为另一个对象的子集时，前者就是后者的子类。因此，“云雀”是“鸟类”的子集，而“鸟类”是“云雀”的超类。

注意：在英语日常交谈中，是用复数来表示类，如birds(鸟类)和larks(云雀)。在Python中，约定使用单数形式且首字母大写，如Bird和Lark。

​		通过这样的描述，子类和超类就很容易理解。但在所有面向对象编程中，子类关系意味深长，因为类是由其支持的方法来定义的。类的所有实例都拥有该类的所有方法，因此所有的子类实例都拥有超类的所有方法。有鉴于此，要定义子类，只需定义多出来的方法和重写一些既有的方法。

​		例如，Bird类可以提供方法fly，而Penguin（Bird的一个子类）可能会新增方法eat_fish。创建Penguin类时，你还可能想想重写超类的方法，即方法fly。鉴于企鹅不能飞，因此在Penguin的实例中，方法fly应什么都不做或引发异常。

语法格式如下：

```
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

​		类实例化后，可以使用其属性，实际上，创建一个类之后，可以通过类名访问其属性。

​		类对象支持两种操作：属性引用和实例化。

​		属性引用使用和 Python 中所有的属性引用一样的标准语法：**obj.name**。

​		类对象创建后，类命名空间中所有的命名都是有效属性名。所以如果类定义是这样:

```python
#!/usr/bin/python3
 
class MyClass:
    """一个简单的类实例"""
    i = 12345
    def f(self):
        return 'hello world'
 
# 实例化类
x = MyClass()
 
# 访问类的属性和方法
print("MyClass 类的属性 i 为：", x.i)
print("MyClass 类的方法 f 输出为：", x.f())
```

​		以上创建了一个新的类实例并将该对象赋给局部变量 x，x 为空的对象。其中MyClass是类的名称。class创建独立的命名空间，用于在其中定义函数。一切看起来都挺好，但你可能会疑虑，参数self是什么。它指向对象本身。如果没有这个self，则所有方法都无法访问对象本身--要操作属性所属的对象。

​		执行以上程序输出结果为：

```
MyClass 类的属性 i 为： 12345
MyClass 类的方法 f 输出为： hello world
```

​		类有一个名为 `__init__()` 的特殊方法（**构造方法**），该方法在类实例化时会自动调用，像下面这样： 

```python
def __init__(self):     
	self.data = []
```

​		类定义了 `__init__()` 方法，类的实例化操作会自动调用 `__init__()` 方法。如下实例化类 MyClass，对应的 `__init__()` 方法就会被调用:

```
x = MyClass()
```

​		当然， `__init__()` 方法可以有参数，参数通过 `__init__()` 传递到类的实例化操作上。例如: 

```python
#!/usr/bin/python3
 
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
x = Complex(3.0, -4.5)
print(x.r, x.i)   # 输出结果：3.0 -4.5
```

**self代表类的实例，而非类**

​		类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的**第一个参数名称**, 按照惯例它的名称是 self。

```python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)
 
t = Test()
t.prt()
```

​		以上实例执行结果为：

```
<__main__.Test instance at 0x100771878>
__main__.Test
```

​		从执行结果可以很明显的看出，self 代表的是类的实例，代表当前对象的地址，而 self.class 则指向类。

​		self 不是 python 关键字，我们把他换成 runoob 也是可以正常执行的:

```python
class Test:
    def prt(runoob):
        print(runoob)
        print(runoob.__class__)
 
t = Test()
t.prt()
```

​		以上实例执行结果为：

```
<__main__.Test instance at 0x100771878>
__main__.Test
```

## 7. 类的方法

​		在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self, 且为第一个参数，self 代表的是类的实例。 

```python
#!/usr/bin/python3
 
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
# 实例化类
p = people('runoob',10,30)
p.speak()
```

​		执行以上程序输出结果为：

```
runoob 说: 我 10 岁。
```

​		默认情况下，可从外部访问对象的属性。有些程序员认为这没问题，但有些程序员则认为这违反了封装原则。它们认为应该对外部完全隐藏对象的状态（即不能从外部访问它们）。你可能会问，为何它们的立场如此极端呢？由每个对象管理自己的属性还不够吗？为何还要向外部隐藏属性？毕竟，如果能直接访问CloseObject（对象c所属的类）对象的属性name，就不需要创建方法setName和getName了。

​		关键在于其他的程序员可能（也不该知道）对象内部发生的情况。况且，CloseObject可能在对象修改其名称时向管理员发送电子邮件。这种功能可能包含在方法set_name中。但如果直接设置c.name，那这个逻辑可能不会触发，并不会发送电子邮件。为避免这类问题，可将属性设置为私有。私有属性不能从对象外部直接访问，而只能通过存储器（set，get）方法来访问。

​		Python没有为私有属性提供直接的支持，而是要求程序员知道在什么情况下从外部修改属性是安全的。毕竟，你必须知道如何使用对象之后才能使用它。然而，通过某些小花招，可获得类似于私有属性的效果。

​		要让方或属性成为私有的（即不能从外部直接访问），只需要让其名称以两个下划线打头即可。

```python
class Secretive:

    def __inaccessible(self):
        print("Bet you can't see me...")

    def accessible(self):
        print("The secret message is : ")
        self.__inaccessible()

s = Secretive()
s.accessible()	#可执行
s.inaccessible()	#报错
```

​		以上代码运行结果如下：

```
The secret message is : 
Bet you can't see me...
Traceback (most recent call last):
  File "D:/System Lib/Documents/2019-09-06/Secretive.py", line 12, in <module>
    s.inaccessible()
AttributeError: 'Secretive' object has no attribute 'inaccessible'
```



## 8. 方法重写

​		如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法，实例如下：

```python
#!/usr/bin/python3
 
class Parent:        # 定义父类
   def myMethod(self):
      print ('调用父类方法')
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print ('调用子类方法')
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
super(Child,c).myMethod() #用子类对象调用父类已被覆盖的方法
```

​		`super()` 函数是用于调用父类(超类)的一个方法。

​		执行以上程序输出结果为：

```
调用子类方法
调用父类方法
```

## 9. 类属性与方法

### 9.1 类的私有属性

​		**__private_attrs**：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 **self.__private_attrs**。

### 9.2 类的方法

​		在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 self，且为第一个参数，self 代表的是类的实例。

​		self 的名字并不是规定死的，也可以使用 this，但是最好还是按照约定是用 self。

### 9.3 类的私有方法

​		**__private_method**：两个下划线开头，声明该方法为私有方法，只能在类的内部调用 ，不能在类的外部调用。**self.__private_methods**。 

​		类的私有属性实例如下：

```python
#!/usr/bin/python3
 
class JustCounter:
    __secretCount = 0  # 私有变量
    publicCount = 0    # 公开变量
 
    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print (self.__secretCount)
 
counter = JustCounter()
counter.count()
counter.count()
print (counter.publicCount)
print (counter.__secretCount)  # 报错，实例不能访问私有变量
```

​		执行以上程序输出结果为：

```
1
2
2
Traceback (most recent call last):
  File "test.py", line 16, in <module>
    print (counter.__secretCount)  # 报错，实例不能访问私有变量
AttributeError: 'JustCounter' object has no attribute '__secretCount'
```

​		类的私有方法实例如下：

```python
#!/usr/bin/python3
 
class Site:
    def __init__(self, name, url):
        self.name = name       # public
        self.__url = url   # private
 
    def who(self):
        print('name  : ', self.name)
        print('url : ', self.__url)
 
    def __foo(self):          # 私有方法
        print('这是私有方法')
 
    def foo(self):            # 公共方法
        print('这是公共方法')
        self.__foo()
 
x = Site('菜鸟教程', 'www.runoob.com')
x.who()        # 正常输出
x.foo()        # 正常输出
x.__foo()      # 报错
```

​		以上实例执行结果：

![img](../images/Python/F5C2A308-3A88-42B4-B575-C719EB8F1CC4.jpg)

### 9.4 类的专有方法

- **`__init__` :** 构造函数，在生成对象时调用
- **`__del__` :** 析构函数，释放对象时使用
- **`__repr__` :** 打印，转换
- **`__setitem__` :** 按照索引赋值
- **`__getitem__`:** 按照索引获取值
- **`__len__`:** 获得长度
- **`__cmp__`:** 比较运算
- **`__call__`:** 函数调用
- **`__add__`:** 加运算
- **`__sub__`:** 减运算
- **`__mul__`:** 乘运算
- **`__truediv__`:** 除运算
- **`__mod__`:** 求余运算
- **`__pow__`:** 乘方

## 10. 运算符重载

​		Python同样支持运算符重载，我们可以对类的专有方法进行重载，实例如下：

```python
#!/usr/bin/python3
 
class Vector:
   def __init__(self, a, b):
      self.a = a
      self.b = b
 
   def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)
   
   def __add__(self,other):
      return Vector(self.a + other.a, self.b + other.b)
 
v1 = Vector(2,10)
v2 = Vector(5,-2)
print (v1 + v2)
```

​		以上代码执行结果如下所示:

```
Vector(7,8)
```