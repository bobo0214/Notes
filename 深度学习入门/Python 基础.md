# Python 廖雪峰

## 前传

**解释型**编程语言不同于**编译型**语言。在编译型语言中，代码在运行前被翻译成**机器代码**。在解释型语言中，代码**由解释器逐行执行**，解释器通常用另一种语言编写。这使得编写和测试代码更容易，但它可能比编译型语言慢。

**脚本**是包含一系列由计算机执行的命令的文件。脚本通常用于自动执行重复性任务，例如重命名文件、移动文件或处理数据。由于 Python 简单易读，因此经常用于编写脚本。

## Python 基础

### 字符类型和变量

1. `r''`表示单引号内部的 \ 失效，不执行转义
2. `''' ... '''`表示多行内容

```python
print('''line1
line2
line3''')
```
3. python中的逻辑为：`and` `or` `not`
    布尔值为：`True` `False`
    空值：`None`  但不能理解为0
4. python中变量定义为动态语言
    常量的名称为全**大写**
5. python中 `/`是精确的除法，整除使用地板除`//`



### 字符串、编码和格式化

1. ASCII Unicode UTF-8的区别与联系
计算机**内存**中统一使用`Unicode`编码，当需要**保存到硬盘** 或 需要**传输**的时候，转换为`UTF-8`
eg1. 用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件
eg2. 浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器
2. `ord()`获取字符的整数表示，`chr()`把整数编码转换为对应的字符
3. python中字符串以Unicode编码，所以对`bytes`类型的数据用`b'...'`表示
注意，虽然 'ABC' 和 b'ABC' 在内容上是相同的，但后者只占用一个字节
4. `encode()`将Unicode表示的str转化为指定的bytes
```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
```
`decode()`将bytes转换为str，若bytes中有一部分无效的字节，可以传入`errors='ignore'`
```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> >>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```
5. `len()`计算str中包含多少个字符 或 bytes中有多少个字节数
6. 1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。为避免乱码问题，应始终坚持使用UTF-8对str和bytes进行转换
所以当源码包含中文时，务必要指定保存为UTF-8编码：
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
7. 格式化：`%s`:字符串，`%d`:整数，`%f`:浮点数，`%x`:十六进制整数
一个占位符就不用打括号了
```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```
8. `format()`格式化
```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```
9. 使用以`f`开头的字符串格式化
```python
>>> r = 2.5
>>> s = 3.14 * r ** 2
>>> print(f'The area of a circle with radius {r} is {s:.2f}')
The area of a circle with radius 2.5 is 19.62
```
10. Test

```python
# -*- coding: utf-8 -*-
s1 = 72
s2 = 85
r = (s2 - s1) / s1
print(r)
print('%.1f' % r)
print('{0:.1f}'.format(r))
print(f'{r:.1f}')
```
Answer:
```
0.18055555555555555
0.2
0.2
0.2
```



### 使用list和tuple

1. 有序集合：列表list	（可以理解成数组）
   + 用`len()`获得list元素的个数
     + 记得最后一个元素的索引是`len(classmates) - 1`
   + py 可以用<font color='red'>负数下标</font>
   + `append()`追加元素到末尾
     + `insert(index,value)`把元素插入到指定位置
   + `pop()`删除list末尾的元素
     + `pop(index)`删除指定位置的元素
   + list里面元素的<font color='red'>数据类型也可以不同</font>

```python
>>> classmates = ['Michael', 'Bob', 'Tracy']
>>> len(classmates)
3

>>> classmates[-1]
'Tracy'

>>> classmates.append('Adam')
>>> classmates
['Michael', 'Bob', 'Tracy', 'Adam']
>>> classmates.insert(1, 'Jack')
>>> classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']

>>> L = ['Apple', 123, True]
>>> s = ['python', 'java', ['asp', 'php'], 'scheme']
>>> len(s)
4
>>> s[2][1]
'php'
```

2. 另一种有序列表：元组tuple	（<font color='red'>但tuple一旦初始化就不能修改</font> → 更安全）

```python
>>> classmates = ('Michael', 'Bob', 'Tracy')

>>> t = ()
>>> t
()

# 这样定义是数字1
>>> t = (1)
>>> t
1
# 这样定义才是元组1
>>> t = (1,)
>>> t
(1,)

# 可变tuple
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```



### 条件判断

`if 条件语句:`	`elif 条件语句:`	`else:`

```python
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```

+ `input()`<font color='red'>返回的数据类型是str！！！</font>

```python
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
```



### 模式匹配

`match`语句	（可以理解成switch）

```python
score = 'B'

match score:
    case 'A':
        print('score is A.')
    case 'B':
        print('score is B.')
    case 'C':
        print('score is C.')
    case _: # _表示匹配到其他任何情况
        print('score is ???.')

```

+ `match`语句除了可以匹配简单的单个值外，<font color='red'>还可以匹配多个值、匹配一定范围，并且把匹配后的值绑定到变量</font>

```python
age = 15

match age:
    case x if x < 10:	# 匹配一定范围，并且把匹配后的值绑定到变量
        print(f'< 10 years old: {x}')
    case 10:
        print('10 years old.')
    case 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18:	# 匹配多个值
        print('11~18 years old.')
    case 19:
        print('19 years old.')
    case _:
        print('not sure.')

```

```python
args = ['gcc', 'hello.c', 'world.c']
# args = ['clean']
# args = ['gcc']

match args:
    # 如果仅出现gcc，报错:
    case ['gcc']:
        print('gcc: missing source file(s).')
    # 出现gcc，且至少指定了一个文件:
    # 列表第一个字符串是'gcc'，第二个字符串绑定到变量file1，后面的任意个字符串绑定到*files
    case ['gcc', file1, *files]:
        print('gcc compile: ' + file1 + ', ' + ', '.join(files))
    # 仅出现clean:
    case ['clean']:
        print('clean')
    case _:
        print('invalid command.')

```



### 循环

1. for...in 循环

```python
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

+ `range(x)`生成从0到x-1的整数序列；`list()`将序列转换为列表

```python
>>> list(range(5))
[0, 1, 2, 3, 4]
```

2. while 循环

`while 条件语句:`

3. break语句	continue语句



### 使用dict和set

1.  dict 字典：使用键-值（key-value）存储，具有极快的查找速度，是用空间来换取时间的一种方法。

<font color='red'>dict的key必须是不可变对象</font>

```python
# 初始化时把数据放入字典
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d['Michael']
95

# 通过key放入
>>> d['Jack'] = 90
>>> d['Jack']
90
>>> d['Jack'] = 88
>>> d['Jack']
88
```

+ 避免key不存在

  + 一是通过`in`判断key是否存在

  ```python
  >>> 'Thomas' in d
  False
  ```

  + 二是通过dict提供的`get()`方法，如果key不存在，可以返回`None`，或者自己指定的value：

  ```python
  >>> d.get('Thomas')
  # 注意：返回None的时候Python的交互环境不显示结果。
  >>> d.get('Thomas', -1)
  -1
  ```

+ 删除key（value也会跟着删除）用`pop(key)`方法

```python
>>> d.pop('Bob')
75
>>> d
{'Michael': 95, 'Tracy': 85}
```



2.  set：一组没有重复key的集合，不存储value

<font color='red'>set的key必须是不可变对象</font>

```python
# 要创建一个set，需要提供一个list作为输入集合：
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
# 显示的{1, 2, 3}只是告诉你这个set内部有1，2，3这3个元素，显示的顺序也不表示set是有序的
```

+ 添加`add(key)`	删除`remove(key)`

```python
# 添加
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}

# 删除
>>> s.remove(4)
>>> s
{1, 2, 3}
```

+ set可以看成数学意义上的无序和无重复元素的集合，因此，<font color='red'>两个set可以做数学意义上的交集、并集等操作</font>

```python
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
# &是交集
>>> s1 & s2	
{2, 3}
# |是并集
>>> s1 | s2
{1, 2, 3, 4}
```



3. 可变对象 VS 不可变对象
   + list 是可变对象
   
   ```python
   >>> a = ['c', 'b', 'a']
   >>> a.sort()
   >>> a
   ['a', 'b', 'c']
   ```
   
   + str 是不可变对象
   
   ```python
   >>> a = 'abc'
   >>> b = a.replace('a', 'A')
   >>> b
   'Abc'
   >>> a
   'abc'
   ```
   
   

## 函数

### 调用函数

常见函数：`abs()`  `max()`   `math.sqrt()`  `math.cos()`  `math.sin()`

数据类型转化：`int()`  `float()`  `str()`  `bool()`

+ <font color='red'>函数名其实就是指向一个函数对象的引用，可以把函数名赋给一个变量</font>

```python
>>> a = abs # 变量a指向abs函数
>>> a(-1) # 所以也可以通过a调用abs函数
1
```



### 定义函数

+ 使用`def`语句

```python
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x

```

+ 空函数

```python
# pass可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个pass，让代码能运行起来。
def nop():
    pass

# pass实例2
if age >= 18:
    pass
```

+ <font color='red'>返回多个值</font>

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

# Python的函数返回多值其实就是返回一个tuple
>>> x, y = move(100, 100, 60, math.pi / 6)
>>> print(x, y)
151.96152422706632 70.0

>>> r = move(100, 100, 60, math.pi / 6)
>>> print(r)
(151.96152422706632, 70.0)
```

+ 函数执行完毕也没有`return`语句时，自动`return None`



### 函数的参数

#### 默认参数

**必选参数在前，默认参数在后**，否则Python的解释器会报错

```python
def enroll(name, gender, age=6, city='Beijing'):
    print('name:', name)
    print('gender:', gender)
    print('age:', age)
    print('city:', city)
```

+ 有多个默认参数时，调用的时候，既可以按顺序提供默认参数
+ 当不按顺序提供部分默认参数时，需要把参数名写上

定义默认参数要牢记一点：<font color='red'>默认参数必须指向不变对象！</font>

```python
def add_end(L=[]):
    L.append('END')
    return L
    
>>> add_end()
['END']
>>> add_end()
['END', 'END']
>>> add_end()
['END', 'END', 'END']

# Python函数在定义的时候，默认参数L的值就被计算出来了，即[]
# 因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。
```

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L

>>> add_end()
['END']
>>> add_end()
['END']
```



#### 可变参数

*：可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。

```python
# 范例1
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
# 但是调用的时候，需要先组装出一个list或tuple：
>>> calc([1, 2, 3])
14
>>> calc((1, 3, 5, 7))
84

# 范例2：可变参数
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
# 利用可变参数，调用函数的方式可以简化
>>> calc(1, 2, 3)
14
>>> calc(1, 3, 5, 7)
84

# 范例3：已经有一个list或者tuple，如何调用一个可变参数
>>> nums = [1, 2, 3]
>>> calc(*nums)
14
```



#### 关键字参数

**：关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
    
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```



#### 命名关键字参数

```python
def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
```

+ 如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收`city`和`job`作为关键字参数。

```python
# 命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。
def person(name, age, *, city, job):
    print(name, age, city, job)
    
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```

+ 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
    
# 命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错：
>>> person('Jack', 24, 'Beijing', 'Engineer')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: person() missing 2 required keyword-only arguments: 'city' and 'job'
```



### 递归函数

```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n - 1)

# 递归调用的次数过多，会导致栈溢出
>>> fact(1000)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 4, in fact
  ...
  File "<stdin>", line 4, in fact
RuntimeError: maximum recursion depth exceeded in comparison

# 解决递归调用栈溢出的方法是通过尾递归优化
# 在函数返回的时候，调用自身本身，并且，return语句不能包含表达式
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

可以看出：`return fact_iter(num - 1, num * product)`仅返回递归函数本身，`num - 1`和`num * product`在函数调用前就会被计算，不影响函数调用。



递归，即只用找到n是n-1通过什么逻辑到达的即可

**汉诺塔**：

一层，逻辑肯定是 A->C,

那么二层是什么？答案是：在一层的基础上，将第一层全部移到B上，这个时候A只剩一块了，那么将A->C,然后再将所有的B->C。结束

那么三层是什么？在二层的基础上重复上述逻辑。

**解决思路三步**：

1. 把n-1个的盘子看成一个整体，从a挪动到b;
2. 把第n个盘子(也就是最后一个盘子，也可以是第一个盘子），从a到c;
3.  把n-1的盘子从b到c.

```python
def move(n,a,b,c):
    if n == 1:
        print(a,"-->",c)
    else:
        move(n-1,a,c,b) #n-1从a到b,c辅助
        move(1,a,b,c) #也可以写成同上文第二步，因为只会挪动一次：print(a,"-->",c)
        move(n-1,b,a,c) #n-1从b到c,a辅助
        
move(3, 'A', 'B', 'C')
```



## 高级特性

### 切片

```python
>>> L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']

# L[:3]等同于L[0:3]
# 打印0 1 2
>>> L[0:3]	
['Michael', 'Sarah', 'Tracy']
# 打印 -2 -1
>>> L[-2:]	
['Bob', 'Jack']
>>> L[-2:-1]
['Bob']

>>> L = list(range(100))
# 前10个数，每两个取一个
>>> L[:10:2]
[0, 2, 4, 6, 8]
# 所有数，每5个取一个
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
# 只写[:]就可以原样复制一个list
>>> L[:]
[0, 1, 2, 3, ..., 99]
```

tuple 和 字符串也可以进行切片操作

```python
>>> (0, 1, 2, 3, 4, 5)[:3]
(0, 1, 2)

>>> 'ABCDEFG'[:3]
'ABC'
>>> 'ABCDEFG'[::2]
'ACEG'
```

#### 练习

利用切片操作，实现一个trim()函数，去除字符串首尾的空格，注意不要调用str的`strip()`方法：

```python
# -*- coding: utf-8 -*-
def trim(s):
	while s and s[0] == ' ':
        s = s[1:]
    while s and s[-1] == ' ':
        s = s[:-1]
    return s
```



### 迭代

默认情况下，`dict`迭代的是key。如果要迭代value，可以用`for value in d.values()`，如果要同时迭代key和value，可以用`for k, v in d.items()`。

list、tuple、字符串直接迭代 

+ 如何判断一个对象是可迭代对象呢？方法是通过`collections.abc`模块的`Iterable`类型判断：

```python
>>> from collections.abc import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```



#### 练习

请使用迭代查找一个list中最小和最大值，并返回一个tuple：

```python
# -*- coding: utf-8 -*-
def findMinAndMax(L):
    if not L:
        return (None,None)
    min = L[0]    
    max = L[0]
    for x in L:
        if x < min:
            min = x
        if x > max:
            max = x
    return (min,max)
        
```



### 列表生成式

写列表生成式时，把要生成的元素`x * x`放到前面，后面跟`for`循环，就可以把list创建出来

```python
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# for循环后面还可以加上if判断
# 但是，不能在最后的if加上else,因为for后面的if是一个筛选条件
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
# if写在for前面必须加else,因为for前面的部分是一个表达式
>>> [x if x % 2 == 0 else -x for x in range(1, 11)]
[-1, 2, -3, 4, -5, 6, -7, 8, -9, 10]

# 可以使用两层循环，可以生成全排列
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

# 也可以使用两个变量来生成list
>>> d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> [k + '=' + v for k, v in d.items()]
['y=B', 'x=A', 'z=C']
```



#### 练习

tip：使用内建的`isinstance`函数可以判断一个变量是不是字符串：

```python
解释>>> x = 'abc'
>>> y = 123
>>> isinstance(x, str)
True
>>> isinstance(y, str)
False
```

如果list中既包含字符串，又包含整数，由于非字符串类型没有`lower()`方法，所以列表生成式会报错：

```python
解释>>> L = ['Hello', 'World', 18, 'Apple', None]
>>> [s.lower() for s in L]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 1, in <listcomp>
AttributeError: 'int' object has no attribute 'lower'
```

请修改列表生成式，通过添加`if`语句保证列表生成式能正确地执行：

```python
# -*- coding: utf-8 -*-
L1 = ['Hello', 'World', 18, 'Apple', None]
L2 = [x.lower() for x in L1 if isinstance(x,str)]
```



### 生成器

一边循环一边计算的机制，称为生成器：generator

+ generator

  + 只要把一个列表生成式的`[]`改成`()`，就创建了一个**generator**

  ```python
  >>> L = [x * x for x in range(10)]
  >>> L
  [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
  >>> g = (x * x for x in range(10))
  >>> g
  <generator object <genexpr> at 0x1022ef630>
  打印
  ```

  + 可以通过`next()`函数获得generator的下一个返回值

    ```python
    >>> next(g)
    0
    >>> next(g)
    1
    >>> next(g)
    4
    >>> next(g)
    9
    >>> next(g)
    16
    >>> next(g)
    25
    >>> next(g)
    36
    >>> next(g)
    49
    >>> next(g)
    64
    >>> next(g)
    81
    >>> next(g)
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    StopIteration
    ```

    使用`for`循环

    ```python
    >>> g = (x * x for x in range(10))
    >>> for n in g:
    ...     print(n)
    ... 
    0
    1
    4
    9
    16
    25
    36
    49
    64
    81
    ```

+ generator函数

  + 如果一个函数定义中包含`yield`关键字，那么这个函数就不再是一个普通函数，而是一个generator函数，**调用一个generator函数将返回一个generator**

  ```python
  def fib(max):
      n, a, b = 0, 0, 1
      while n < max:
          yield b		# 原函数是print(b)
          a, b = b, a + b
          n = n + 1
      return 'done'
  
  >>> f = fib(6)		# 原函数会打印1 1 2 3 5 8 'done'
  >>> f
  <generator object fib at 0x104feaaa0>
  ```

+ generator函数执行

  + 普通函数是顺序执行，遇到`return`语句或者最后一行函数语句就返回。
  + 而变成generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。

  ```python
  # 定义一个generator函数，依次返回数字1，3，5
  def odd():
      print('step 1')
      yield 1
      print('step 2')
      yield(3)
      print('step 3')
      yield(5)
      
  # 调用该generator函数时，首先要生成一个generator对象，然后用next()函数不断获得下一个返回值    
  >>> o = odd()
  >>> next(o)
  step 1
  1
  >>> next(o)
  step 2
  3
  >>> next(o)
  step 3
  5
  >>> next(o)
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  StopIteration
  
  # 注意区分，以下相当于每次创建了一个新的generator对象
  >>> next(odd())
  step 1
  1
  >>> next(odd())
  step 1
  1
  >>> next(odd())
  step 1
  1
  ```

+ fib函数

  + 把函数改成generator函数后，直接使用`for`循环来迭代

  ```python
  >>> for n in fib(6):
  ...     print(n)
  ...
  1
  1
  2
  3
  5
  8
  ```

  + 但是用`for`循环调用generator时，发现拿不到generator的`return`语句的**返回值**。如果想要拿到返回值，必须捕获`StopIteration`错误，返回值包含在`StopIteration`的`value`中

  ```python
  >>> g = fib(6)
  >>> while True:
  ...     try:
  ...         x = next(g)
  ...         print('g:', x)
  ...     except StopIteration as e:
  ...         print('Generator return value:', e.value)
  ...         break
  ...
  g: 1
  g: 1
  g: 2
  g: 3
  g: 5
  g: 8
  Generator return value: done
  ```



#### 练习

杨辉三角

```python
# -*- coding: utf-8 -*-

def triangles():
        row = [1]
    while True:
        yield row
        row = [1] + [row[i] + row[i + 1] for i in range(len(row) - 1)] + [1]
        
  
# 打印
n = 0
results = []
for t in triangles():
    results.append(t)
    n = n + 1
    if n == 10:
        break

for t in results:
    print(t)

if results == [
    [1],
    [1, 1],
    [1, 2, 1],
    [1, 3, 3, 1],
    [1, 4, 6, 4, 1],
    [1, 5, 10, 10, 5, 1],
    [1, 6, 15, 20, 15, 6, 1],
    [1, 7, 21, 35, 35, 21, 7, 1],
    [1, 8, 28, 56, 70, 56, 28, 8, 1],
    [1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
]:
    print('测试通过!')
else:
    print('测试失败!')
```



### 迭代器

+ 这些可以直接作用于`for`循环的对象统称为**可迭代对象**：`Iterable`

  + 可以使用`isinstance()`判断一个对象是否是`Iterable`对象

  ```python
  >>> from collections.abc import Iterable
  >>> isinstance([], Iterable)
  True
  >>> isinstance({}, Iterable)
  True
  >>> isinstance('abc', Iterable)
  True
  >>> isinstance((x for x in range(10)), Iterable)
  True
  >>> isinstance(100, Iterable)
  False
  ```

+ 可以被`next()`函数调用并不断返回下一个值的对象称为迭代器：`Iterator`

  + 可以使用`isinstance()`判断一个对象是否是`Iterator`对象

  ```python
  >>> from collections.abc import Iterator
  >>> isinstance((x for x in range(10)), Iterator)
  True
  >>> isinstance([], Iterator)
  False
  >>> isinstance({}, Iterator)
  False
  >>> isinstance('abc', Iterator)
  False
  ```

+ 生成器都是`Iterator`对象，但`list`、`dict`、`str`虽然是`Iterable`，却不是`Iterator`

  + 把`list`、`dict`、`str`等`Iterable`变成`Iterator`可以使用`iter()`函数

  ```python
  >>> isinstance(iter([]), Iterator)
  True
  >>> isinstance(iter('abc'), Iterator)
  True
  ```

  + 举例

  ```python
  # 例1
  for x in [1, 2, 3, 4, 5]:
      pass
      
  # 例2等价于例1
  # 首先获得Iterator对象:
  it = iter([1, 2, 3, 4, 5])
  # 循环:
  while True:
      try:
          # 获得下一个值:
          x = next(it)
      except StopIteration:
          # 遇到StopIteration就退出循环
          break
  ```

  

+ 为什么`list`、`dict`、`str`等数据类型不是`Iterator`？

  + 这是因为Python的`Iterator`对象表示的是一个数据流，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出`StopIteration`错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过`next()`函数实现按需计算下一个数据，所以`Iterator`的计算是惰性的，只有在需要返回下一个数据时它才会计算。
  + `Iterator`甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。



## 函数式编程

**面向过程编程**

想象你在做一顿饭，面向过程编程就像是你按照食谱的指示，一步一步地去做。每一步都是一个过程，比如：

1. 切菜：你按照食谱的指示去切各种蔬菜。
2. 煮水：然后把水煮沸。
3. 炒菜：最后按照一定的顺序把蔬菜放进锅里炒。

在这种编程方式中，你关心的是如何做：先做什么，然后做什么，每一步都具体指明做的动作。

**函数式编程**

函数式编程，用同样做饭的比喻，就像你设计了一些特别的烹饪工具（函数），每个工具都是为了完成一个特定的任务。这些工具的特点是：

- 使用这些工具（函数）时，你只需要知道它需要什么原材料（输入）和它能做出什么菜（输出），而不需要知道里面具体是如何操作的。
- 这些工具（函数）是可以被重复使用的，不管用在哪个食谱上，只要需要相同的处理，就可以使用。

举个简单的例子，如果有一个“切菜器”（函数），你只要把菜放进去，它就会自动切好菜，你不需要关心它是怎么切的，每次放进去同样的菜，出来的切好的菜也是一样的。

函数式编程的核心就在于：

- 不关心过程：不像面向过程那样关心每一步怎么做，而是更关心“这个工具（函数）需要什么输入，能产生什么输出”。
- 没有副作用：就像那个“切菜器”（函数），它不会突然决定不工作，或者因为它切了菜就改变了菜的种类。它每次工作的结果都是可预测的，不会影响到其他的东西。

**总结**

- 面向过程编程：就像是按照食谱做菜，你需要一步一步地跟着做，关心每一步的具体操作。
- 函数式编程：更像是使用一些特制的工具来帮助你完成任务，你只需要知道每个工具的输入和输出，而不需要关心它内部是怎么实现的。



### 高阶函数

举例

```python
def add(x, y, f):
    return f(x) + f(y)
    
x = -5
y = 6
f = abs
f(x) + f(y) ==> abs(-5) + abs(6) ==> 11
return 11
```

#### map / reduce

1.  `map()`函数接收两个参数，一个是函数，一个是`Iterable`;`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回。

```python
# example 1
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]

# example 2
>>> list(map(str, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

2. `reduce`把一个函数作用在一个序列`[x1, x2, x3, ...]`上，**这个函数必须接收两个参数**；`reduce`把结果继续和序列的下一个元素做累积计算：`reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`

```python
# example 1
# 也可以直接使用sum()函数
>>> from functools import reduce
>>> def add(x, y):
...     return x + y
...
>>> reduce(add, [1, 3, 5, 7, 9])
25

# example 2
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

```python
# 把str转换为int
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> def char2num(s):
...     digits = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
...     return digits[s]
...
>>> reduce(fn, map(char2num, '13579'))
13579

# 整理可得
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))
```

3. 练习

```python
# -*- coding: utf-8 -*-
# 题目1
# 利用map()函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']

def normalize(name):
     name = name.lower()
     name = name[0].upper() + name[1:]
     return name

# 测试:
L1 = ['adam', 'LISA', 'barT']
L2 = list(map(normalize, L1))
print(L2)

```

注：python中的字符串不能修改，这是常量

```python
# -*- coding: utf-8 -*-
# 题目2
# 请编写一个prod()函数，可以接受一个list并利用reduce()求积

from functools import reduce

def prod(L):
    def f(x,y):
        return x * y
    return reduce(f,L)
    
print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))
if prod([3, 5, 7, 9]) == 945:
    print('测试成功!')
else:
    print('测试失败!')
```

```python
# -*- coding: utf-8 -*-
# 题目3 (不会lambda！)
# 利用map和reduce编写一个str2float函数，把字符串'123.456'转换成浮点数123.456

from functools import reduce

def str2float(s):
    def str2float:
		integer，decimal = s.split('.')
	def char2num(s):
        DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6}
		return DIGITS[s]
	return reduce(lambda:x, y: x * 10 + y,map(char2num, s)) / (10 ** len(decimal))
```



#### filter

`filter()`也接收一个函数和一个序列。

和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素。

```python
# 在一个list中，删掉偶数，只保留奇数
def is_odd(n):
    return n % 2 == 1

list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
# 结果: [1, 5, 9, 15]
```

```python
# 把一个序列中的空字符串删掉
def not_empty(s):
    return s and s.strip()

list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
# 结果: ['A', 'B', 'C']
```



**用filter求素数** （不会lambda!）

1. 首先，列出从`2`开始的所有自然数，构造一个序列：
2. 取序列的第一个数`2`，它一定是素数，然后用`2`把序列的`2`的倍数筛掉
3. 取新序列的第一个数`3`，它一定是素数，然后用`3`把序列的`3`的倍数筛掉
4. 取新序列的第一个数`5`，然后用`5`把序列的`5`的倍数筛掉

```python
# 先构造一个从3开始的奇数序列
# 注意这是一个生成器，并且是一个无限序列
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n

# 定义一个筛选函数
def _not_divisible(n):
    return lambda x: x % n > 0

# 定义一个生成器，不断返回下一个素数
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
        
# 由于primes()也是一个无限序列，所以调用时需要设置一个退出循环的条件
# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```



**练习**

```python
# 回数是指从左向右读和从右向左读都是一样的数，例如12321，909。请利用filter()筛选出回数
# -*- coding: utf-8 -*-
def is_palindrome(n):
    nStr = str(n)
    return nStr == nStr[::-1]
    
# 测试:
output = filter(is_palindrome, range(1, 1000))
print('1~1000:', list(output))
if list(filter(is_palindrome, range(1, 200))) == [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99, 101, 111, 121, 131, 141, 151, 161, 171, 181, 191]:
    print('测试成功!')
else:
    print('测试失败!')
```



#### sorted

1. `sorted()`函数就可以对list进行排序

```python
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```

2. `sorted()`函数也是一个高阶函数，它还可以接收一个`key`函数来实现自定义的排序，例如按绝对值大小排序

```python
# key指定的函数将作用于list的每一个元素上，并根据key函数返回的结果进行排序
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```

3.  字符串排序

```python
# 默认情况下，对字符串排序，是按照ASCII的大小比较的
>>> sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']

>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
['about', 'bob', 'Credit', 'Zoo']

# 反向排序，可以传入第三个参数reverse=True
>>> sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)
['Zoo', 'Credit', 'bob', 'about']
```

**练习**

```python
# 假设我们用一组tuple表示学生名字和成绩
# L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
# 请用sorted()对上述列表分别按名字排序
# -*- coding: utf-8 -*-

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]

def by_name(t):
    return t[0]
    
L2 = sorted(L, key=by_name)
print(L2)
```

```python
#再按成绩从高到低排序：
# -*- coding: utf-8 -*-

L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]

def by_score(t):
    return -t[1]

L2 = sorted(L, key=by_score)
print(L2)
```



### 返回函数

返回函数即：函数作为返回值

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum

# 当我们调用lazy_sum()时，返回的并不是求和结果，而是求和函数
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
# 调用函数f时，才真正计算求和的结果
>>> f()
25
```

当调用`lazy_sum()`时，每次调用都会返回一个新的函数，即使传入相同的参数

```python
>>> f1 = lazy_sum(1, 3, 5, 7, 9)
>>> f2 = lazy_sum(1, 3, 5, 7, 9)
>>> f1==f2
False
```



**闭包**：内层函数引用了外层函数的局部变量

返回的函数并没有立刻执行，而是直到调用了`f()`才执行。

```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()

# 原因就在于返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了3，因此最终结果为9
>>> f1()
9
>>> f2()
9
>>> f3()
9
```

```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
    
# 结果
>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9
```



**nonlocal**

如果只是读外层变量的值，会发现返回的闭包函数调用一切正常

```python
def inc():
    x = 0
    def fn():
        # 仅读取x的值:
        return x + 1
    return fn

f = inc()
print(f()) # 1
print(f()) # 1
```

如果对外层变量赋值，由于Python解释器会把`x`当作函数`fn()`的局部变量，会报错

原因是`x`作为局部变量并没有初始化，直接计算`x+1`是不行的。但我们其实是想引用`inc()`函数内部的`x`，所以需要在`fn()`函数内部加一个`nonlocal x`的声明。加上这个声明后，解释器把`fn()`的`x`看作外层函数的局部变量，它已经被初始化了，可以正确计算`x+1`。

```python
def inc():
    x = 0
    def fn():
        # nonlocal x
        x = x + 1
        return x
    return fn

f = inc()
print(f()) # 1
print(f()) # 2
```

<font color = 'red'>使用闭包时，对外层变量赋值前，需要先使用nonlocal声明该变量不是当前函数的局部变量。</font>



**练习**

利用闭包返回一个计数器函数，每次调用它返回递增整数

```python
# -*- coding: utf-8 -*-
def createCounter():
    x = 0
    def counter():
        nonlocal x
        x = x + 1
        return x
    return counter

# 测试:
counterA = createCounter()
print(counterA(), counterA(), counterA(), counterA(), counterA()) # 1 2 3 4 5
counterB = createCounter()
if [counterB(), counterB(), counterB(), counterB()] == [1, 2, 3, 4]:
    print('测试通过!')
else:
    print('测试失败!')
```



### 匿名函数

关键字`lambda`表示匿名函数，冒号前面的`x`表示函数参数。

匿名函数有个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。

```python
>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```

匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数

```python
>>> f = lambda x: x * x
>>> f
<function <lambda> at 0x101c6ef28>
>>> f(5)
25
```

同样，也可以把匿名函数作为返回值返回

```python
def build(x, y):
    return lambda: x * x + y * y
```



**练习**

```python
# 原函数
def is_odd(n):
    return n % 2 == 1

L = list(filter(is_odd, range(1, 20)))

# 改写
L = list(filter(lambda x:x % 2 == 1, range(1, 20)))
```



### 装饰器

由于函数也是一个对象，而且函数对象可以被赋值给变量，所以，通过变量也能调用该函数。

函数对象有一个`__name__`属性（注意：是前后各两个下划线），可以拿到函数的名字

```python
>>> def now():
...     print('2015-3-25')
...
>>> f = now
>>> f()
2015-3-25

>>> now.__name__
'now'
>>> f.__name__
'now'
```

现在，假设我们要增强`now()`函数的功能，比如，在函数调用前后自动打印日志，但又不希望修改`now()`函数的定义，这种在代码运行期间动态增加功能的方式，称之为“**装饰器**”

```py
# 观察log，因为它是一个decorator，所以接受一个函数作为参数，并返回一个函数。
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

# 借助Python的@语法，把decorator置于函数的定义处
# 把@log放到now()函数的定义处，相当于执行了语句now = log(now)
@log
def now():
    print('2015-3-25')

# 调用now()函数，不仅会运行now()函数本身，还会在运行now()函数前打印一行日志
>>> now()
call now():
2015-3-25
```

原来的`now()`函数仍然存在，只是现在同名的`now`变量指向了新的函数，于是调用`now()`将执行新函数，即在`log()`函数中返回的`wrapper()`函数。

经过decorator装饰之后的函数，它们的`__name__`已经从原来的`'now'`变成了`'wrapper'`。但不需要编写`wrapper.__name__ = func.__name__`这样的代码，Python内置的`functools.wraps`就是干这个事的。

```python
>>> now.__name__
'wrapper'
```

一个完整的decorator写法：

```py
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper


# 带参数的decorator：
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```



**练习**

```py
# 请设计一个decorator，它可作用于任何函数上，并打印该函数的执行时间
# -*- coding: utf-8 -*-
import time, functools

def metric(fn):
     @functools.wraps(fn)
     def wrapper(*args, **kwargs):
         start = time.time()
         result = fn(*args, **kwargs)
         end = time.time()
         print(f'{fn.__name__} executed in {(end - start)*1000:.2f} ms')
         return result
     return wrapper
     
# 测试
@metric
def fast(x, y):
    time.sleep(0.0012)
    return x + y;

@metric
def slow(x, y, z):
    time.sleep(0.1234)
    return x * y * z;

f = fast(11, 22)
s = slow(11, 22, 33)
if f != 33:
    print('测试失败!')
elif s != 7986:
    print('测试失败!')
```



### 偏函数

`int()`函数可以把字符串转换为整数，当仅传入字符串时，`int()`函数默认按十进制转换；但`int()`函数还提供额外的`base`参数，默认值为`10`。如果传入`base`参数，就可以做N进制的转换

```py
>>> int('12345')
12345
>>> int('12345', base=8)
5349
>>> int('12345', 16)
74565
```

假设要转换大量的二进制字符串，每次都传入`int(x, base=2)`非常麻烦。于是可以定义一个`int2()`的函数，默认把`base=2`传进去

```python
def int2(x, base=2):
    return int(x, base)

>>> int2('1000000')
64
>>> int2('1010101')
85
```

`functools.partial`就是帮助我们创建一个偏函数的，不需要我们自己定义`int2()`

```py
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85
# 也可以在函数调用时传入其他值
>>> int2('1000000', base=10)
1000000
```

创建偏函数时，实际上可以接收函数对象、`*args`和`**kw`这3个参数

```py
# 实际上会把10作为*args的一部分自动加到左边
max2 = functools.partial(max, 10)

max2(5, 6, 7)   
# 等同于
args = (10, 5, 6, 7)
max(*args)
# 结果为10

```



## 模块

### 使用模块

```py
#!/usr/bin/env python3		# 让文件直接在Unix/Linux/Mac上运行
# -*- coding: utf-8 -*-		# .py文件使用标准UTF-8编码

' a test module '			# 表示模块的文档注释

__author__ = 'Michael Liao' # 作者名字

import sys					# 导入模块

def test():
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':	# 如果在其他地方导入该hello模块时，if判断将失败
    test()					# 这种if测试可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试
```

`sys`模块有一个`argv`变量，用list存储了命令行的所有参数。`argv`至少有一个元素，因为第一个参数永远是该.py文件的名称

+ 运行`python3 hello.py`获得的`sys.argv`就是`['hello.py']`；
+ 运行`python3 hello.py Michael`获得的`sys.argv`就是`['hello.py', 'Michael']`。

```py
# 运行结果
$ python3 hello.py
Hello, world!
$ python hello.py Michael
Hello, Michael!
```



**作用域**

+ 正常的函数和变量名是公开的（public），可以被直接引用，比如：`abc`，`x123`，`PI`等；

+ 类似`__xxx__`这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的`__author__`，`__name__`就是特殊变量

+ 类似`_xxx`和`__xxx`这样的函数或变量就是非公开的（private），不应该被直接引用，比如`_abc`，`__abc`等；

```python
# private函数/变量的用处
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```

外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public。



### 安装第三方模块

+ 推荐使用清华源 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple + 模块名

+ 直接使用pycharm，将第三方库的镜像源设置为阿里或其他国内的镜像源，安装依赖库又方便又快。



## 面向对象编程

### 类和实例

1. `class`后面紧接着是类名，即`Student`，类名通常是大写开头的单词；紧接着是`(object)`，表示该类是从哪个类继承下来的；

   通常，如果没有合适的继承类，就使用`object`类，这是所有类最终都会继承的类。

```py
class Student(object):
    pass
```

2. 由于类可以起到模板的作用，因此，可以在创建实例的时候，把一些我们认为必须绑定的属性强制填写进去。通过定义一个特殊的`__init__`方法，在创建实例的时候，就把`name`，`score`等属性绑上去

​     注意到`__init__`方法的第一个参数永远是`self`，表示创建的实例本身

```py
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score
        

# 创建实例
>>> bart = Student('Bart Simpson', 59)
>>> bart.name
'Bart Simpson'
>>> bart.score
59
```

3. 数据封装

```py
class Student(object):
    ...

    def get_grade(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 60:
            return 'B'
        else:
            return 'C'
        
    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```



### 访问限制

**私有变量(private)**：如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线`__`

```py
class Student(object):
	# 已经无法从外部访问实例变量.__name和实例变量.__score了
    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))
```

外界访问或改变私有变量：

```py
class Student(object):
    ...

    # 外界获取私有变量
    def get_name(self):
        return self.__name

    def get_score(self):
        return self.__score
    
    # 外界改变私有变量
    def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
    
```

<font color = 'red'>注意</font>：在Python中，变量名类似`__xxx__`的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量，所以，不能用`__name__`、`__score__`这样的变量名

```py
>>> bart = Student('Bart Simpson', 59)
>>> bart.get_name()
'Bart Simpson'
>>> bart.__name = 'New Name' # 设置__name变量！
>>> bart.__name
'New Name'

>>> bart.get_name() # get_name()内部返回self.__name
'Bart Simpson'
```

可以看出，`bart.__name`只是新设置了一个`__name`成员变量，而非原先的`__name`私有变量



### 继承和多态

```py
# 已经编写了Animal类
class Animal(object):
    def run(self):
        print('Animal is running...')
        
# 编写子类，直接从Animal类继承
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```

当子类和父类都存在相同的`run()`方法时，类的`run()`覆盖了父类的`run()`。在代码运行的时候，总是会调用子类的`run()`。这样，就获得了继承的另一个好处：多态。

<font color = 'red'>注意（静态语言 vs 动态语言）</font>：

对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。

对于Python这样的动态语言来说，则不一定需要传入`Animal`类型。**我们只需要保证传入的对象有一个`run()`方法就可以了**

```py
class Timer(object):
    def run(self):
        print('Start...')
```



### 获取对象信息

1. 使用type()

```py
# 基本类型
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>

# 一个变量指向函数或者类
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```

```py
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

```py
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```



2. **使用isinstance()**（优先使用）

```
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```

<font color = 'red'>还可以判断一个变量是否是某些类型中的一种</font>

```py
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```



3. 使用dir()

如果要获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的list

```py
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']

>>> len('ABC')
3
>>> 'ABC'.__len__()
3
```

仅仅把属性和方法列出来是不够的，配合`getattr()`、`setattr()`以及`hasattr()`，我们可以直接操作一个对象的状态

```py
>>> class MyObject(object):
...     def __init__(self):
...         self.x = 9
...     def power(self):
...         return self.x * self.x
...
>>> obj = MyObject()

# 测试
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19

>>> getattr(obj, 'z', 404) # 获取属性'z'，如果不存在，返回默认值404
404

# 获得对象的方法
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
>>> fn # fn指向obj.power
<bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
>>> fn() # 调用fn()与调用obj.power()是一样的
81
```



### 实例属性和类属性

**实例属性**：给实例绑定属性的方法是通过实例变量，或者通过`self`变量

```py
class Student(object):
    def __init__(self, name):
        self.name = name

s = Student('Bob')
s.score = 90
```

**类属性**：可以直接在class中定义属性，这种属性是类属性，归`Student`类所有

```py
class Student(object):
    name = 'Student'
    
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```

<font color = 'red'>注意</font>：`Student.count++`  Python不能这样写  → ` Student.count += 1`



## 面向对象高级编程

### 使用\__slots__

```py
# 定义class
class Student(object):
    pass

# 给实例绑定一个属性
>>> s = Student()
>>> s.name = 'Michael' # 动态给实例绑定一个属性
>>> print(s.name)
Michael

# 给实例绑定一个方法
>>> def set_age(self, age): # 定义一个函数作为实例方法
...     self.age = age
...
>>> from types import MethodType
>>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
>>> s.set_age(25) # 调用实例方法
>>> s.age # 测试结果
25
```

```py
# 但是，给一个实例绑定的方法，对另一个实例是不起作用的
>>> s2 = Student() # 创建新的实例
>>> s2.set_age(25) # 尝试调用方法
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'

# 为了给所有实例都绑定方法，可以给class绑定方法
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```

定义一个特殊的`__slots__`变量，来限制该class实例能添加的属性

```py
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称

>>> s = Student() # 创建新的实例
>>> s.name = 'Michael' # 绑定属性'name'
>>> s.age = 25 # 绑定属性'age'
>>> s.score = 99 # 绑定属性'score'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'score'
```



### 使用@property

Python内置的`@property`装饰器就是负责把一个方法变成属性调用

```py
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

把一个getter方法变成属性，只需要加上`@property`就可以了

此时，`@property`本身又创建了另一个装饰器`@score.setter`，负责把一个setter方法变成属性赋值

```py
>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

还可以定义只读属性，只定义getter方法，不定义setter方法就是一个只读属性。

如下，`birth`是可读写属性，而`age`就是一个*只读*属性，因为`age`可以根据`birth`和当前时间计算出来

```py
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2015 - self._birth
```

<font color = 'red'>注意</font>：属性的方法名不要和实例变量重名。



**练习**

请利用`@property`给一个`Screen`对象加上`width`和`height`属性，以及一个只读属性`resolution`

```py
# -*- coding: utf-8 -*-

class Screen(object):
    @property
    def width(self):
        return self._width

    @width.setter
    def width(self,value):
        self._width = value

    @property
    def height(self):
        return self._height

    @height.setter
    def height(self,value):
        self._height = value

    @property
    def resolution(self):
        return self._height * self._width
    
    
# 测试:
s = Screen()
s.width = 1024
s.height = 768
print('resolution =', s.resolution)
if s.resolution == 786432:
    print('测试通过!')
else:
    print('测试失败!')
    


```



### 多重继承

![微信截图_20240711210750](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407112108873.png)

![微信截图_20240711210802](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407112109401.png)

![微信截图_20240711210817](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407112109998.png)

正确的做法是采用**多重继承**。

首先，主要的类层次仍按照哺乳类和鸟类设计

```py
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```

现在，我们要给动物再加上`Runnable`和`Flyable`的功能，只需要先定义好`Runnable`和`Flyable`的类

```py
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
        
# 对于需要Runnable功能的动物，就多继承一个Runnable      
class Dog(Mammal, Runnable):
    pass
# 对于需要Flyable功能的动物，就多继承一个Flyable
class Bat(Mammal, Flyable):
    pass
```



### 定制类

#### \__str__

```py
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...
>>> print(Student('Michael'))
<__main__.Student object at 0x109afb190>	# 太丑

# 修改
>>> class Student(object):
...     def __init__(self, name):
...         self.name = name
...     def __str__(self):
...         return 'Student object (name: %s)' % self.name
...
>>> print(Student('Michael'))
Student object (name: Michael)
```

直接敲变量不用`print`，打印出来的实例还是丑

这是因为直接显示变量调用的不是`__str__()`，而是`__repr__()`

两者的区别是`__str__()`返回用户看到的字符串，而`__repr__()`返回程序开发者看到的字符串，也就是说，`__repr__()`是为调试服务的。

```py
>>> s = Student('Michael')
>>> s
<__main__.Student object at 0x109afb310>

class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name=%s)' % self.name
    __repr__ = __str__
```



#### \__iter__

如果一个类想被用于`for ... in`循环，类似list或tuple那样，就必须实现一个`__iter__()`方法，该方法返回一个迭代对象

Python的for循环就会不断调用该迭代对象的`__next__()`方法拿到循环的下一个值，直到遇到`StopIteration`错误时退出循环。

```py
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
 

>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```



#### \__getitem__

要表现得像list那样按照下标取出元素，需要实现`__getitem__()`方法

```py
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a
   
   
>>> f = Fib()
>>> f[0]
1
>>> f[1]
1
>>> f[2]
2
>>> f[3]
3
>>> f[10]
89
>>> f[100]
573147844013817084101        
```



#### \__getattr__

调用一个不存在的属性

+ 除了可以加上一个该属性外

+ 还可以写一个`__getattr__()`方法，动态返回一个属性。

```py
class Student(object):

    def __init__(self):
        self.name = 'Michael'

    def __getattr__(self, attr):
        if attr=='score':
            return 99
```

```py
class Student(object):
	# 返回函数也是可以的
    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
            
>>> s.age()
25
```

注意：

1. 只有在没有找到属性的情况下，才调用`__getattr__`
2. 任意调用如`s.abc`都会返回`None`，这是因为我们定义的`__getattr__`默认返回就是`None`



#### \__call__

一个对象实例可以有自己的属性和方法，当我们调用实例方法时，我们用`instance.method()`来调用。

任何类，只需要定义一个`__call__()`方法，就可以直接对实例进行调用。

```py
class Student(object):
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print('My name is %s.' % self.name)
```

```py
>>> s = Student('Michael')
>>> s() # self参数不要传入
My name is Michael.
```

怎么判断一个变量是对象还是函数呢？

其实，更多的时候，我们需要判断一个对象是否能被调用，能被调用的对象就是一个`Callable`对象

```py
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```



### 使用枚举类

```py
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```



### 使用元类



## 错误、调试和测试



## IO编程



## 进程和线程



## 正则表达式



## 常用内建模块



## 常用第三方模块
