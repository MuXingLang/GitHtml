# 第八章：字典

## 1. 概述

字典是另一种可变容器模型，且可存储任意类型对象，是一种可通过名称来访问其中各个值的数据结构，又被称为映射，字典是Python中唯一的内置映射类型，其中的值不按顺序排列，而是存储在键下。

字典由键和其对应值组成，一个键值对（K-V对）可称为项。字典的每个键值(key=>value)对用冒号(**:**)分割，每个对之间用逗号(**,**)分割，整个字典包括在花括号(**{})**中 ,格式如下所示： 

```
d = {key1 : value1, key2 : value2 }
```

键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例：

```
dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}
```

也可如此创建字典：

```
dict1 = { 'abc': 456 }
dict2 = { 'abc': 123, 98.6: 37 }
```

------

## 2. 访问字典里的值

把相应的键放入到方括号中，如下实例:

```python
#!/usr/bin/python3   

dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}   
print ("dict['Name']: ", dict['Name']) 
print ("dict['Age']: ", dict['Age'])
```

以上实例输出结果：

```
dict['Name']:  Runoob
dict['Age']:  7
```

如果用字典里没有的键访问数据，会输出错误如下：

```python
#!/usr/bin/python3   

dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}   
print ("dict['Alice']: ", dict['Alice'])
```

以上实例输出结果：

```
Traceback (most recent call last):
  File "test.py", line 5, in <module>
    print ("dict['Alice']: ", dict['Alice'])
KeyError: 'Alice'
```

## 3. 修改字典

向字典添加新内容的方法是增加新的键/值对，修改或删除已有键/值对如下实例:

```python
#!/usr/bin/python3   

dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}   
dict['Age'] = 8               # 更新 Age 
dict['School'] = "菜鸟教程"  # 添加信息     
print ("dict['Age']: ", dict['Age']) 
print ("dict['School']: ", dict['School'])
```

以上实例输出结果： 

```
dict['Age']:  8
dict['School']:  菜鸟教程
```

## 4. 删除字典元素

能删单一的元素也能清空字典，清空只需一项操作。

显示删除一个字典用del命令，如下实例：

```python
#!/usr/bin/python3   

dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}   
del dict['Name'] # 删除键 'Name' 
dict.clear()     # 清空字典 
del dict         # 删除字典   
print ("dict['Age']: ", dict['Age']) 
print ("dict['School']: ", dict['School'])
```

但这会引发一个异常，因为用执行 del 操作后字典不再存在：

```
Traceback (most recent call last):
  File "test.py", line 9, in <module>
    print ("dict['Age']: ", dict['Age'])
TypeError: 'type' object is not subscriptable
```

**注：**del() 方法后面也会讨论。

## 5. 字典键的特性

字典值可以是任何的 python 对象，既可以是标准的对象，也可以是用户定义的，但键不行。

两个重要的点需要记住：

1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，如下实例：

```python
#!/usr/bin/python3   

dict = {'Name': 'Runoob', 'Age': 7, 'Name': '小菜鸟'}  
print ("dict['Name']: ", dict['Name'])
```

以上实例输出结果：

```
dict['Name']:  小菜鸟
```

2）键必须不可变，所以可以用数字，字符串或元组充当，而用列表就不行，如下实例：

```python
#!/usr/bin/python3   

dict = {['Name']: 'Runoob', 'Age': 7}   
print ("dict['Name']: ", dict['Name'])
```

以上实例输出结果：

```
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    dict = {['Name']: 'Runoob', 'Age': 7}
TypeError: unhashable type: 'list'
```

## 6. 字典内置函数&方法

Python字典包含了以下内置函数：

| 序号 | 函数           | 描述                                               |
| ---- | -------------- | -------------------------------------------------- |
| 1    | len(dict)      | 计算字典元素个数，即键的总数。                     |
| 2    | str(dict)      | 输出字典，以可打印的字符串表示。                   |
| 3    | type(variable) | 返回输入的变量类型，如果变量是字典就返回字典类型。 |

Python字典包含了以下内置方法：

| 序号 | 函数                                      | 描述                                                         |
| ---- | ----------------------------------------- | ------------------------------------------------------------ |
| 1    | radiansdict.clear()                       | 删除字典内所有元素                                           |
| 2    | radiansdict.copy()                        | 返回一个字典的浅复制                                         |
| 3    | radiansdict.fromkeys()                    | 创建一个新字典，以序列seq中元素做字典的键，val为字典所有键对应的初始值 |
| 4    | radiansdict.get(key, default=None)        | 返回指定键的值，如果值不在字典中返回default值                |
| 5    | key in dict                               | 如果键在字典dict里返回true，否则返回false                    |
| 6    | radiansdict.items()                       | 以列表返回可遍历的(键, 值) 元组数组                          |
| 7    | radiansdict.keys()                        | 返回一个迭代器，可以使用 list() 来转换为列表                 |
| 8    | radiansdict.setdefault(key, default=None) | 和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default |
| 9    | radiansdict.update(dict2)                 | 把字典dict2的键/值对更新到dict里                             |
| 10   | radiansdict.values()                      | 返回一个迭代器，可以使用 list() 来转换为列表                 |
| 11   | pop(key[,default\])                       | 删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值。 |
| 12   | popitem()                                 | 随机返回并删除字典中的一对键和值(一般删除末尾对)。           |

## 7. 字典常用方法

### 7.1 clear

删除所有字典项，此操作就地执行，因此无返回值(或者说返回None)

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> d = {}
>>> d['name'] = 'Gumby'
>>> d['age'] = 42
>>> d
{'name': 'Gumby', 'age': 42}
>>> f = d		#浅拷贝
>>> f
{'name': 'Gumby', 'age': 42}
>>> f = {}		#第一种清空，不影响原字典
>>> f
{}
>>> d
{'name': 'Gumby', 'age': 42}
>>> returned_value = d.clear()	#第二种清空，原字典被操作
>>> d
{}
```

### 7.2 copy

返回一个新字典，其中包含的键值对和原来字典完全相同（该方法执行的是浅复制，即值本身还是原件，而非副本），当替换副本中的值时，原件不受影响。

```powershell
>>> x = {'username':'admin','machines':['foo','bar','baz']}
>>> y = x.copy()
>>> y
{'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
>>> y['username'] = 'mxl'
>>> y['machines'].remove('baz')
>>> y
{'username': 'mxl', 'machines': ['foo', 'bar']}
```

然而，当修改副本中的值时（仅修改值而非替换），原件也会发生变化，因为原件指向的也是被修改的值，为解决此类问题，可使用深复制，即copy模块中的函数**deepcopy**

```powershell
>>> from copy import deepcopy
>>> a = {'username':'admin','machines':['foo','bar','baz']}
>>> b = a.copy()
>>> c = deepcopy(a)
>>> b
{'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
>>> c
{'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
>>> a['machines'].remove('baz')
>>> b
{'username': 'admin', 'machines': ['foo', 'bar']}
>>> c
{'username': 'admin', 'machines': ['foo', 'bar', 'baz']}
```

### 7.3 fromkeys

创建一个新字典，其中包含指定的键，且每个键对应的值都是None：

```powershell
>>> {}.fromkeys(['name','age'])
{'name': None, 'age': None}
```

以上实例是首先创建了一个空字典，再对其调用fromkeys方法创建另一个字典，这是不符合Python精神的，可换成如下方式调用该方法（dict是所有字典所属的类型）：

```powershell
>>> dict.fromkeys(['name','age'])
{'name': None, 'age': None}
```

如果不想使用默认值None，可自定特定值：

```powershell
>>> dict.fromkeys(['name','age'],'(unknown)')
{'name': '(unknown)', 'age': '(unknown)'}
```

### 7.4 get

用于访问字典中指定的项，即时该项在指定地点中不存在，也不会报错，而是返回None,，当然，你也可以指定特定返回值，而如果字典包含该项，则get方法和普通字典查找相同。

```powershell
>>> d = {}
>>> print(d['name'])
Traceback (most recent call last):
  File "<pyshell#19>", line 1, in <module>
    print(d['name'])
KeyError: 'name'
>>> print(d.get('name'))
None
>>> print(d.get('name','N/A'))
N/A
>>> d['name'] = 'Bob'
>>> d.get('name')
'Bob'
```

### 7.5 items

返回一个包含所有字典项的列表，其中每个元素都为（key,value）的形式，字典项在列表中的排列顺序不确定,返回值属于一种字典视图的特殊类型，可用于迭代，还可以确认其长度以及成员资格检查。

```powershell
>>> x = {'username':'admin','machines':['foo','bar','baz']}
>>> it = x.items()
>>> it
dict_items([('username', 'admin'), ('machines', ['foo', 'bar', 'baz'])])
>>> len(it)
2
>>> ('username','admin') in it
True
```

视图的一个优点是不复制，其始终指向底层字典的反应，即便你修改了底层字典：

```powershell
>>> x = {'username':'admin','machines':['foo','bar','baz']}
>>> it = x.items()
>>> ('username','root') in it
False
>>> x['username'] = 'root'
>>> ('username','root') in it
True
```

### 7.6 keys

返回一个字典视图，其中包含指定字典的键。

### 7.7 pop

可用于获取与指定键相关联的值，并将该键值对从字典中删除

```powershell
>>> d = {'a':1,'b':'2'}
>>> d.pop(b)
Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    d.pop(b)
TypeError: unhashable type: 'dict'
>>> d.pop('b')
'2'
>>> d
{'a': 1}
```

### 7.8 popitem

类似于list.pop,但list.pop弹出列表中最后一个元素，而popitem是随机弹出一个字典项。如果你要以高效的方式逐个删除并处理所有字典项，可使用该方法而不需要先获取键列表。

### 7.9 setdefault

有点像get，因为它也获取与指定键相关联的值，而当字典中不包含该项时，会在字典中添加指定的键值对，如果存在该键值对，则返回该键值对。

```powershell
>>> d = {}
>>> d.setdefault('name','N/A')
'N/A'
>>> d
{'name': 'N/A'}
>>> d.setdefault('name','???')
'N/A'
```

### 7.10 update

使用一个字典的项来更新另一个字典

```powershell
>>> a = {'a':1,'b':2}
>>> b= {'b':3}
>>> a
{'a': 1, 'b': 2}
>>> b
{'b': 3}
>>> a.update(b)
>>> a
{'a': 1, 'b': 3}
```

### 7.11 values

返回一个字典中值组成的字典视图，不同于keys,此方法可能会返回重复值，因为键在字典中是唯一的，而值不是。

