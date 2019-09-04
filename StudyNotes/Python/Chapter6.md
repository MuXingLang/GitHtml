# 第六章：列表

## 1. 概述

序列是Python中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置，或索引，第一个索引是0，第二个索引是1，依此类推。

 Python有6个序列的内置类型，但最常见的是列表和元组。

序列都可以进行的操作包括索引，切片，加，乘，检查成员。

此外，Python已经内置确定序列的长度以及确定最大和最小的元素的方法。

列表是最常用的Python数据类型，它可以作为一个方括号内的逗号分隔值出现。

列表的数据项不需要具有相同的类型

创建一个列表，只要把逗号分隔的不同的数据项使用方括号括起来即可。如下所示：

```
list1 = ['Google', 'Runoob', 1997, 2000]; 

list2 = [1, 2, 3, 4, 5 ]; list3 = ["a", "b", "c", "d"];
```

与字符串的索引一样，列表索引从0开始。列表可以进行截取、组合等。

## 2. 访问列表中的值

使用下标索引来访问列表中的值，同样你也可以使用方括号的形式截取字符，如下所示：

```python
#!/usr/bin/python3   

list1 = ['Google', 'Runoob', 1997, 2000]; 
list2 = [1, 2, 3, 4, 5, 6, 7 ];   
print ("list1[0]: ", list1[0]) 
print ("list2[1:5]: ", list2[1:5])
```

以上实例输出结果：

```
list1[0]:  Google
list2[1:5]:  [2, 3, 4, 5]
```

------

## 3. 更新列表

你可以对列表的数据项进行修改或更新，你也可以使用append()方法来添加列表项，如下所示： 

```python
#!/usr/bin/python3   

list = ['Google', 'Runoob', 1997, 2000]   
print ("第三个元素为 : ", list[2]) 

list[2] = 2001 
print ("更新后的第三个元素为 : ", list[2])
```

**注意：**我们会在接下来的章节讨论append()方法的使用

以上实例输出结果：

```
第三个元素为 :  1997
更新后的第三个元素为 :  2001
```

------

## 4. 删除列表元素

可以使用 del 语句来删除列表的的元素，如下实例：

```python
#!/usr/bin/python3   

list = ['Google', 'Runoob', 1997, 2000]   
print ("原始列表 : ", list) 
del list[2] 
print ("删除第三个元素 : ", list)
```

以上实例输出结果：

```
原始列表 :  ['Google', 'Runoob', 1997, 2000]
删除第三个元素 :  ['Google', 'Runoob', 2000]
```

**注意：**我们会在接下来的章节讨论 remove() 方法的使用

------

## 5. 列表脚本操作符

列表对 + 和  * 的操作符与字符串相似。+ 号用于组合列表，* 号用于重复列表。

如下所示：

| Python 表达式                         | 结果                         | 描述                 |
| ------------------------------------- | ---------------------------- | -------------------- |
| len([1, 2, 3])                        | 3                            | 长度                 |
| [1, 2, 3] + [4, 5, 6]                 | [1, 2, 3, 4, 5, 6]           | 组合                 |
| ['Hi!'] * 4                           | ['Hi!', 'Hi!', 'Hi!', 'Hi!'] | 重复                 |
| 3 in [1, 2, 3]                        | True                         | 元素是否存在于列表中 |
| for x in [1, 2, 3]: print(x, end=" ") | 1 2 3                        | 迭代                 |

------

## 6. 列表截取与拼接

Python的列表截取与字符串操作类型，如下所示：

```python
L=['Google', 'Runoob', 'Taobao']
```

操作：

| Python 表达式 | 结果                 | 描述                                               |
| ------------- | -------------------- | -------------------------------------------------- |
| L[2]          | 'Taobao'             | 读取第三个元素                                     |
| L[-2]         | 'Runoob'             | 从右侧开始读取倒数第二个元素: count from the right |
| L[1:]         | ['Runoob', 'Taobao'] | 输出从第二个元素开始后的所有元素                   |

```
>>L=['Google', 'Runoob', 'Taobao']
>>L[2]
'Taobao'
>>L[-2]
'Runoob'
>>L[1:]
['Runoob', 'Taobao']
```

列表还支持拼接操作：

```powershell
>>squares = [1, 4, 9, 16, 25]
>>squares += [36, 49, 64, 81, 100]
>>squares
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

详细的例子：

```powershell
>>> numbers = [1,2,3,4,5,6,7,8,9,10]
>>> numbers[3:6]
[4, 5, 6]
>>> numbers[0:1]
[1]
>>> numbers[7:10] #这里的索引10指的是第十一个元素，虽然不曾存在第十一个元素，但已读到此位置
[8, 9, 10]
>>> numbers[-3:-1]
[8, 9]
>>> numbers[-3:0] #起止点无交叉（起点索引大于终点索引），得到的切片为空
[]
>>> numbers[-3:]
[8, 9, 10]
>>> numbers[:3]
[1, 2, 3]
>>> numbers[:]
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> 
```

切片获取域名

```python
url = input('Please input a URL: ')
domain = url[11:-4]
print('url <' + url + '> , domain is ' + domain)
```

运行结果如下：

```
Please input a URL: http://www.baidu.com
url <http://www.baidu.com> , domain is baidu
>>> 
```

关于步长，进行切片时，一般都会显式或隐式的指定起点和终点，还有另一个参数：步长，而一般其默认为1，这意味着从一个元素移动到下一个元素，具体例子如下：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> numbers = [1,2,3,4,5,6,7,8,9,10]
>>> numbers[0:10:2]
[1, 3, 5, 7, 9]
>>> numbers[::3]
[1, 4, 7, 10]
>>> numbers[0:10:-1]
[]
>>> numbers[0:9:-1]
[]
>>> numbers[10:0:-1]
[10, 9, 8, 7, 6, 5, 4, 3, 2]
>>> numbers[::-2]
[10, 8, 6, 4, 2]
>>> numbers[5::-2]
[6, 4, 2]
>>> numbers[:5:-2]
[10, 8]
>>> 
```

## 7. 嵌套列表

使用嵌套列表即在列表里创建其它列表，例如：

```powershell
>>a = ['a', 'b', 'c']
>>n = [1, 2, 3]
>>x = [a, n]
x
[['a', 'b', 'c'], [1, 2, 3]]
>>x[0]
['a', 'b', 'c']
>>x[0][1]
'b'
```

## 8. 加法&乘法

相同类型的序列可以使用加法拼接：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> a = [1,2,3]
>>> b = [4,5,6]
>>> a + b
[1, 2, 3, 4, 5, 6]
>>> c = 'hello'
>>> a + c
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    a + c
TypeError: can only concatenate list (not "str") to list
>>> 
```

将序列和数X相乘，将重复这个序列x次来创建一个新的序列：

```powershell
Python 3.6.3 (v3.6.3:2c5fed8, Oct  3 2017, 18:11:49) [MSC v.1900 64 bit (AMD64)] on win32
Type "copyright", "credits" or "license()" for more information.
>>> 'python ' * 5
'python python python python python '
>>> [10] * 5
[10, 10, 10, 10, 10]
>>> sequence = [None] * 10 #空列表初始化
>>> sequence
[None, None, None, None, None, None, None, None, None, None]
>>> 
```



## 9. 列表函数&方法

Python包含以下函数:

| 序号 | 函数                                                      |
| ---- | --------------------------------------------------------- |
| 1    | len(list) 列表元素个数       |
| 2    | max(list) 返回列表元素最大值 |
| 3    | min(list) 返回列表元素最小值 |
| 4    | list(seq) 将元组转换为列表  |

Python包含以下方法:

| 序号 | 方法                                                         |
| ---- | ------------------------------------------------------------ |
| 1    | list.append(obj) 在列表末尾添加新的对象 |
| 2    | list.count(obj) 统计某个元素在列表中出现的次数 |
| 3    | list.extend(seq) 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| 4    | list.index(obj) 从列表中找出某个值第一个匹配项的索引位置 |
| 5    | list.insert(index, obj) 将对象插入列表 |
| 6    | list.pop([index=-1\]) 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| 7    | list.remove(obj) 移除列表中某个值的第一个匹配项 |
| 8    | list.reverse() 反向列表中元素 |
| 9    | 	list.sort( key=None, reverse=False) 对原列表进行排序 |
| 10   | list.clear() 清空列表         |
| 11   | list.copy() 复制列表           |

