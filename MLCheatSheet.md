[TOC]

# Python

## 输入输出



### 在一行输入多个值

```python
a, b, c = map(int,input().split(' '))
```

## 列表和元组

### 声明列表

```python
a = [1,2,3,4]
```

### 列表切片

```python
numbers[7:10] #左开右闭
numbers[7:-1] #切到倒数第二个
numbers[-3:] #切到最后
numbers[0:10:2] #指定步长
numbers[8:3:-1] #反向
numbers[::4] #每四个取一个
numbers[:5:-2] #步长为负数时，从终点移到起点
```



### 序列运算

```python
[1,2,3] + [4,5,6] 
[42] * 10
sequence = [None] * 10
```



### 成员资格：in

```python
>>> users = ['mlh', 'foo', 'bar']
>>> input('Enter your user name: ') in users
Enter your user name: mlh
True
```

### 长度、最小值、最大值

```python
len(numbers)
max(numbers)
min(numbers)
```

### 删除元素

```python
del names[2]
```

### 给切片赋值

```python
>>> name = list('Perl')
>>> name[1:] = list('ython') #与原来长度不同

>>> numbers = [1, 5]
>>> numbers[1:1] = [2, 3, 4] #插入新元素
```

### 列表方法

```python
lst.append(4)
lst.clear()
b = lst #别名
a = 1st.copy() #复制 也可a[:] list(a)
['to', 'be', 'or', 'not', 'to', 'be'].count('to') #计数
a.extend(b) #拼接
knights.index('who') #查询索引
numbers.insert(3, 'four') #插入
x.pop(0) #从列表中删除一个元素，并返回这一元素
x.remove('be')  #删除第一个为指定值的元素
x.reverse() #反向
x.sort() #排序
x.sort(key=len, reverse=True) #按什么排序/是否反向
```

### 元组

```python
>>> 3 * (40 + 2,) 
(42, 42, 42)
>>> tuple([1, 2, 3]) (1, 2, 3)
>>> tuple('abc') ('a', 'b', 'c')
>>> tuple((1, 2, 3)) (1, 2, 3)
>>> x = 1, 2, 3 >>> x[1]
2
>>> x[0:2]
(1, 2)
```

## 字符串

- 字符串是不可变的，所以元素赋值和切片赋值非法

### 字符串原始形态

使用repr()函数打印字符串在代码中的样子，str()打印希望用户看到的样子。

### 代码换行

在代码或字符串中加上反斜杠就可以在代码中换行。

### 设置字符串格式

```python
>>> format = "Hello, %s. %s enough for ya?" >>> values = ('world', 'Hot')
>>> format % values
'Hello, world. Hot enough for ya?'
>>> "{}, {} and {}".format("first", "second", "third") 'first, second and third'
>>> "{0}, {1} and {2}".format("first", "second", "third") 'first, second and third'
>>> "{3} {0} {2} {1} {3} {0}".format("be", "not", "or", "to") 'to be or not to be'
>>> from math import e
>>> f"Euler's constant is roughly {e}."
"Euler's constant is roughly 2.718281828459045."
```

### 数字输出格式

```python
>>> "The number is {num:f}".format(num=42)
```

| 类型 | 含义                       |
| :--- | -------------------------- |
| b    | 二进制                     |
| c    | unicode                    |
| d    | 十进制数                   |
| e    | 科学计数法（e表示指数）    |
| E    | 科学计数法（E表示指数）    |
| f/F  | 定点数                     |
| g/G  | 自动选择定点数和科学计数法 |
| o    | 八进制数                   |
| s    | 默认                       |
| x/X  | 十六进制                   |
| %·   | 百分比值                   |

### 宽度 精度 千位分隔符

```python
>>> "{num:10}".format(num=3)
' 3'
>>> "{name:10}".format(name="Bob") 
'Bob '
>>> "{pi:10.2f}".format(pi=pi) 
' 3.14'
>>> 'One googol is {:,}'.format(10**100)
'One googol is 10,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,00 0,000,000,000,000,000,000,000,000,000,000,000,000,000,000'
```

### 符号，对齐，用0填充

```python
>>> print('{0:<10.2f}\n{0:^10.2f}\n{0:>10.2f}'.format(pi)) 
3.14
	3.14 
		3.14 #左对齐居中右对齐
```

### 字符串居中

```python
>>> "The Middle by Jimmy Eat World".center(39)
'    The Middle by Jimmy Eat World    '
```

### 查找和替换子串

```python
>>> 'With a moo-moo here, and a moo-moo there'.find('moo', 0, 16) #可指定起点和终点 左闭右开 
7 #返回子串第一个字符的索引，否则返回-1
>>> 'This is a test'.replace('is', 'eez') 
'Theez eez a test'
#单个替换
>>> table = str.maketrans('cs', 'kz')
>>> 'this is an incredible test'.translate(table)
'thiz iz an inkredible tezt'
#同时替换多个字符
```

### 在序列的间隔插入字符join

```python
>>> dirs = '', 'usr', 'bin', 'env'
>>> '/'.join(dirs)
'/usr/bin/env'
```

### 把字符串拆分成序列

```python
>>> '1+2+3+4+5'.split('+') 
['1', '2', '3', '4', '5']
```

### 大小写

lower 变为小写

title 变为词首大写

upper 变为大写

### 判断字符串是否满足条件

isalnum、isalpha、isdecimal、isdigit、isidentifier、islower、isnumeric、 isprintable、isspace、istitle、isupper

## 字典

### 创建字典

```python
phonebook = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}
>>> items = [('name', 'Gumby'), ('age', 42)] >>> d = dict(items)
d = dict(name='Gumby', age=42)
```

### 字典方法

```python
x.clear()
y = x.copy()
>>> {}.fromkeys(['name', 'age']) 
{'age': None, 'name': None} #创建值为空的字典
>>> print(d.get('name'))
None #查找元素 没找到返回None(而不是发生错误)
d.items() #返回一个字典项的列表
>>> d = {'x': 1, 'y': 2} >>> d.pop('x')
1
>>> d
{'y': 2}
>>> d = {'url': 'http://www.python.org', 'spam': 0, 'title': 'Python Web Site'} >>> d.popitem()
('url', 'http://www.python.org')
>>> d
{'spam': 0, 'title': 'Python Web Site'}
>>> d = {}
>>> d.setdefault('name', 'N/A') 'N/A'
>>> d
{'name': 'N/A'} #找不到就插入
d.update(x) #用一个字典更新另一个字典
```

## 条件、循环及其他语句

### 序列解包

```python
>>> x, y, z = 1, 2, 3
>>> x, y = y, x
#用信号运算符收集多余的值
>>> a, b, *rest = [1, 2, 3, 4] 
>>> rest
[3, 4]
>>> name = "Albus Percival Wulfric Brian Dumbledore" >>> first, *middle, last = name.split()
>>> middle
['Percival', 'Wulfric', 'Brian']
```

### elif

```python
num = int(input('Enter a number: ')) if num > 0:
print('The number is positive') elif num < 0:
print('The number is negative') else:
print('The number is zero')
```



### 链式比较

python允许链式比较：1 <= number <= 10



### 断言assert

```python
>>> age = 10
>>> assert 0 < age < 100
```

### 迭代字典

```python
d = {'x': 1, 'y': 2, 'z': 3} 
for key in d:
		print(key, 'corresponds to', d[key])
```

### 迭代时获取索引

```python
for index, string in enumerate(strings): 
		if 'xxx' in string:
				strings[index] = '[censored]'
```

### sorted和reversed：返回修改过的结果

```python
>>> sorted([4, 3, 6, 8, 3])
[3, 3, 4, 6, 8]
>>> sorted('Hello, world!')
[' ', '!', ',', 'H', 'd', 'e', 'l', 'l', 'l', 'o', 'o', 'r', 'w'] 
>>> list(reversed('Hello, world!'))
['!', 'd', 'l', 'r', 'o', 'w', ' ', ',', 'o', 'l', 'l', 'e', 'H'] 
>>> ''.join(reversed('Hello, world!'))
'!dlrow ,olleH'
```

### 循环else

```python
from math import sqrt
for n in range(99, 81, -1):
		root = sqrt(n)
		if root == int(root):
				print(n)
				break 
else:
		print("Didn't find it!") #没有break才执行
```

### 简单推导

```python
>>> [x*x for x in range(10) if x%3 == 0] 
[0, 9, 36, 81]
```

### 什么都不做 pass

```python
if name == 'Ralph Auldus Melish': 	
		print('Welcome!')
elif name == 'Enid': # 还未完成......
		pass
elif name == 'Bill Gates':
		print('Access Denied')
```

### del

```python
>>> x = 1
>>> del x
```

### 执行代码字符串exec

```python
>>> from math import sqrt
>>> scope = {} #安全起见可以设一个命名空间
>>> exec('sqrt = 1', scope)
>>> sqrt(4)
2.0
>>> scope['sqrt'] 
1
```

### 执行代码表达式eval

```python
>>> eval(input("Enter an arithmetic expression: ")) 
Enter an arithmetic expression: 6 + 18 * 2
42
```

## 抽象

### 给函数编写文档

```python
def square(x):
'Calculates the square of the number x.' return x * x
#可以像下面这样访问文档字符串:
>>> square.__doc__
'Calculates the square of the number x.'
```

### 关键词参数

```python
>>> store(patient='Mr. Brainsample', hour=10, minute=20, day=13, month=5)
```

### 作用域嵌套

```python
def foo():
		def bar():
				print("Hello, world!") 
    bar()
```

## 魔法

```python
class Bird:
		def __init__(self):
				self.hungry = True 
    def eat(self):
				if self.hungry: 
        		print('Aaaah ...') 
            self.hungry = False
				else:
						print('No, thanks!')

class SongBird(Bird): 
		def __init__(self):
				super().__init__()
				self.sound = 'Squawk!' 
		def sing(self):
				print(self.sound)
```



## 模块

### 集合

```python
>>> set(range(10))
{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> {0, 1, 2, 3, 0, 1, 2, 3, 4, 5}
{0, 1, 2, 3, 4, 5}
>>> a = {1, 2, 3} 
>>> b = {2, 3, 4} 
>>> a.union(b) {1, 2, 3, 4}
>>> a | b {1, 2, 3, 4}
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
>>> a ^ b {1, 4}
>>> a.copy()
{1, 2, 3}
>>> a.copy() is a 
False
```

### Heap 堆

| 函数                 | 描述                          |
| -------------------- | ----------------------------- |
| heappush(heap, x)    | 将x压入堆中                   |
| heappop(heap)        | 从堆中弹出最小的元素          |
| heapify(heap)        | 让列表具备堆特征              |
| heapreplace(heap, x) | 弹出最小的元素，并将x压入堆中 |
| nlargest(n, iter)    | 返回iter中n个最大的元素       |
| nsmallest(n, iter)   | 返回iter中n个最小的元素       |

### collections 双端队列

```python
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

### time 时间

| 索引 | 字段   | 值       |
| ---- | ------ | -------- |
| 0    | 年     |          |
| 1    | 月     |          |
| 2    | 日     |          |
| 3    | 时     |          |
| 4    | 分     |          |
| 5    | 秒     |          |
| 6    | 星期   |          |
| 7    | 儒略日 | 1~366    |
| 8    | 夏令时 | 0、1或-1 |

### time中的重要函数

| 函数                       | 描述                                      |
| -------------------------- | ----------------------------------------- |
| asctime([tuple])           | 将时间元组转换为字符串                    |
| localtime([secs])          | 将秒数转换为表示当地时间的日期元组        |
| mktime(tuple)              | 将时间元组转换为当地时间                  |
| sleep(secs)                | 休眠(什么都不做)secs秒                    |
| strptime(string[, format]) | 将字符串转换为时间元组                    |
| time()                     | 当前时间(从新纪元开始后的秒数，以UTC为准) |

### random 

| 函数                             | 描述                                         |
| -------------------------------- | -------------------------------------------- |
| random()                         | 返回一个0~1(含)的随机实数                    |
| getrandbits(n)                   | 以长整数方式返回n个随机的二进制位            |
| uniform(a, b)                    | 返回一个a~b(含)的随机实数                    |
| randrange([start], stop, [step]) | 从range(start, stop, step)中随机地选择一个数 |
| choice(seq)                      | 从序列seq中随机地选择一个元素                |
| shuffle(seq[, random])           | 就地打乱序列seq                              |
| sample(seq, n)                   | 从序列seq中随机地选择n个值不同的元素         |

### re

| 函数                                 | 描述                                               |
| ------------------------------------ | -------------------------------------------------- |
| compile(pattern[, flags])            | 根据包含正则表达式的字符串创建模式对象             |
| search(pattern, string[, flags])     | 在字符串中查找模式                                 |
| match(pattern, string[, flags])      | 在字符串开头匹配模式                               |
| split(pattern, string[, maxsplit=0]) | 根据模式来分割字符串                               |
| findall(pattern, string)             | 返回一个列表，其中包含字符串中所有与模式匹配的子串 |
| sub(pat, repl, string[, count=0])    | 将字符串中与模式pat匹配的子串都替换为repl          |
| escape(string)                       | 对字符串中所有的正则表达式特殊字符都进行转义       |



## 内置函数

### 打印

```python
print('Hello,', end='') #指定end 不自动换行
```

### 缝合两个序列zip

```python
>>> list(zip(range(5), range(100000000))) 
[(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)]
```



### 计算相关

```python
pow() #乘方
abs() #绝对值
round() #浮点数最近整数
```

## 模块

### 复数处理

```python
import cmath
```

### 绘图小海龟

```python
from turtle import *
forward(100)
left(120)
penup()
pendown()
```



# Numpy

## 数组

```python
# 1D Array
a = np.array([0, 1, 2, 3, 4])
b = np.array((0, 1, 2, 3, 4))
c = np.arange(5)
d = np.linspace(0, 2*np.pi, 5)

# MD Array,
a = np.array([[11, 12, 13, 14, 15],
              [16, 17, 18, 19, 20],
              [21, 22, 23, 24, 25],
              [26, 27, 28 ,29, 30],
              [31, 32, 33, 34, 35]])

np.full((3, 3), True, dtype=bool) #默认值
```

### 数组的信息

```python
# Array properties
a = np.array([[11, 12, 13, 14, 15],
              [16, 17, 18, 19, 20],
              [21, 22, 23, 24, 25],
              [26, 27, 28 ,29, 30],
              [31, 32, 33, 34, 35]])

print(type(a)) # >>><class 'numpy.ndarray'>
print(a.dtype) # >>>int64
print(a.size) # >>>25
print(a.shape) # >>>(5, 5)
print(a.itemsize) # >>>8
print(a.ndim) # >>>2
print(a.nbytes) # >>>200
```



### 数组运算

```python
# Basic Operators
a = np.arange(25)
a = a.reshape((5, 5))

b = np.array([10, 62, 1, 14, 2, 56, 79, 2, 1, 45,
              4, 92, 5, 55, 63, 43, 35, 6, 53, 24,
              56, 3, 56, 44, 78])
b = b.reshape((5,5))

print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a ** 2)
print(a < b) 
print(a > b)
print(a.dot(b)) #点积
```



### 数组特殊运算符

```python
# dot, sum, min, max, cumsum
a = np.arange(10)

print(a.sum()) # >>>45
print(a.min()) # >>>0
print(a.max()) # >>>9
print(a.cumsum()) #前缀和
# >>>[ 0  1  3  6 10 15 21 28 36 45] 
```

## 索引

### 花式索引

```python
# Fancy indexing
a = np.arange(0, 100, 10)
indices = [1, 5, -1]
b = a[indices]
print(a) # >>>[ 0 10 20 30 40 50 60 70 80 90]
print(b) # >>>[10 50 90]
```

### 布尔屏蔽

```python
# Boolean masking
import matplotlib.pyplot as plt

a = np.linspace(0, 2 * np.pi, 50)
b = np.sin(a)
plt.plot(a,b)
mask = b >= 0
plt.plot(a[mask], b[mask], 'bo')
mask = (b >= 0) & (a <= np.pi / 2)
plt.plot(a[mask], b[mask], 'go')
plt.show()
```

### 缺省索引

```python
# Incomplete Indexing
a = np.arange(0, 100, 10)
b = a[:5]
c = a[a >= 50]
print(b) # >>>[ 0 10 20 30 40]
print(c) # >>>[50 60 70 80 90]
```

### where函数

返回一个使得条件为真的列表

```python
# Where
a = np.arange(0, 100, 10)
b = np.where(a < 50) 
c = np.where(a >= 50)[0]
print(b) # >>>(array([0, 1, 2, 3, 4]),)
print(c) # >>>[5 6 7 8 9]
```

## 数据分析练习

原文：https://www.numpy.org.cn/article/advanced/numpy_exercises_for_data_analysis.html



# Pandas

## 三种数据结构

*Pandas*处理以下三个数据结构 - 

- 系列(`Series`)
- 数据帧(`DataFrame`)
- 面板(`Panel`)

### Series

均匀数据的一维数据结构。

### DataFrame

具有异构数据的二维数组。

### Panel

三维数据结构，DataFrame的容器。

## Series

```python
s = pd.Series([1,3,5,np.nan,6,8])

dates = pd.date_range('20170101', periods=7)
print(dates)

print("--"*16)
df = pd.DataFrame(np.random.randn(7,4), index=dates, columns=list('ABCD'))
print(df)
```

