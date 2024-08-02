# 利用python进行数据分析

## 第1章 准备工作

```py
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import statsmodels as sm
```



## 第2章 语法基础，IPython和Jupyter Notebooks

**IPython**：一个强化的Python解释器

**Jupyter notebooks**：一个网页代码笔记本，它原先是IPython的一个子项目。



### 2.1 IPython基础

**配置环境**

```py
pip install ipython
pip install jupyter
```



**Tab补全**

补全<u>命名、对象和模块属性</u>，还可以在输入文件路径时，补全电脑上对应的<u>文件信息</u>



**自省**

1. 在 变量/函数/实例方法 前后使用问号`?`，可以显示对象的信息

2. `?`字符与通配符结合可以匹配所有的名字

```py
# 可以获得所有包含load的顶级NumPy命名空间
In [13]: np.*load*?
np.__loader__
np.load
np.loads
np.loadtxt
np.pkgload
```

在正则表达式中，用`*`表示任意个字符（包括0个），用`+`表示至少一个字符，用`?`表示0个或1个字符，用`{n}`表示n个字符，用`{n,m}`表示n-m个字符

3. 使用`??`会显示函数的源码



**%run命令**

可以用`%run`命令运行所有的Python程序

```py
In [14]: %run ipython_script_test.py
```

在Jupyter notebook中，你也可以使用`%load`，它将脚本导入到一个代码格中

```py
>>> %load ipython_script_test.py
```



**中断运行的代码**

代码运行时按`Ctrl-C`，无论是%run或长时间运行命令，都会导致`KeyboardInterrupt`。



**从剪贴板执行程序**

1. `%paste`可以直接运行剪贴板中的代码

```py
In [17]: %paste
x = 5
y = 7
if x > 5:
    x += 1

    y = 8
## -- End pasted text --
```

2. `%cpaste`功能类似，但会给出一条提示

```py
In [18]: %cpaste
Pasting code; enter '--' alone on the line to stop or use Ctrl-D.
:x = 5
:y = 7
:if x > 5:
:    x += 1
:
:    y = 8
:--
```

可以粘贴任意多的代码再运行，如果粘贴了错误的代码，可以用Ctrl-C中断。



**快捷键**

![表2-1 IPython的标准快捷键](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407181535528.webp)



**魔术命令**

IPython中特殊的命令（Python中没有）被称作“魔术”命令，魔术命令是在指令前添加百分号%前缀

自动魔术：只要没有变量和函数名相同，魔术函数默认可以不用百分号，用`%automagic`打开或关闭。

1. 可以用`%quickref`或`%magic`学习下所有特殊命令

![表2-2 一些常用的IPython魔术命令](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407181541925.webp)

2. `%matplotlib`魔术函数配置了IPython shell和Jupyter notebook中的matplotlib

```py
# IPython shell中
In [26]: %matplotlib
Using matplotlib backend: Qt4Agg

# 在JUpyter中，命令有所不同
In [26]: %matplotlib inline
```



### 2.2 Python语法基础

1. 引用

```py
# 赋值也被称作绑定，我们是把一个名字绑定给一个对象。变量名有时可能被称为绑定变量
# a和b实际上是同一个对象
In [8]: a = [1, 2, 3]
In [9]: b = a

In [10]: a.append(4)

In [11]: b
Out[11]: [1, 2, 3, 4]
```

```py
# 当你将对象作为参数传递给函数时，新的局域变量创建了对原始对象的引用，而不是复制
def append_element(some_list, element):
    some_list.append(element)
    
In [27]: data = [1, 2, 3]

In [28]: append_element(data, 4)

In [29]: data
Out[29]: [1, 2, 3, 4]
```



2. 用`isinstance`函数检查对象是某个类型的实例

```py
In [21]: a = 5

In [22]: isinstance(a, int)
Out[22]: True

# 还可以用类型元组
In [23]: a = 5; b = 4.5

In [24]: isinstance(a, (int, float))
Out[24]: True

In [25]: isinstance(b, (int, float))
Out[25]: True
```



3. 属性和方法

```py
In [1]: a = 'foo'

In [2]: a.<Press Tab>
a.capitalize  a.format      a.isupper     a.rindex      a.strip
a.center      a.index       a.join        a.rjust       a.swapcase
a.count       a.isalnum     a.ljust       a.rpartition  a.title
a.decode      a.isalpha     a.lower       a.rsplit      a.translate
a.encode      a.isdigit     a.lstrip      a.rstrip      a.upper
a.endswith    a.islower     a.partition   a.split       a.zfill
a.expandtabs  a.isspace     a.replace     a.splitlines
a.find        a.istitle     a.rfind       a.startswith
```



4. 鸭子类型：不关心对象的类型，只关心对象是否有某些方法或用途

```py
if not isinstance(x, list) and isiterable(x):
    x = list(x)
```



5. 引入

```py
# some_module.py
PI = 3.14159

def f(x):
    return x + 2

def g(a, b):
    return a + b
```

```py
# 第一种引入方式
import some_module
result = some_module.f(5)
pi = some_module.PI

# 第二种引入方式
from some_module import f, g, PI
result = g(5, PI)

# 第三种引入方式
import some_module as sm
from some_module import PI as pi, g as gf

r1 = sm.f(pi)
r2 = gf(6, pi)
```



6. 二元运算符和比较运算符
   + 要判断两个引用是否**指向同一个对象**，可以使用`is`方法

```py
In [35]: a = [1, 2, 3]

In [36]: b = a

In [37]: c = list(a)

In [38]: a is b
Out[38]: True

In [39]: a is not c
Out[39]: True

# 使用is not 和使用 == 是不同的
In [40]: a == c
Out[40]: True
```



7. 可变与不可变对象

`tuple()` 和` set()` 是不可变的



8. 数据类型

   ![表2-4 Python的标量](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407181606638.webp)

   + 数值类型：Python的主要数值类型是`int`和`float`

   + 字符串

     + `""`或`''`都可以
     + 对于有换行符的字符串，可以使用三引号，`'''`或`"""`都行

     ```py
     c = """
     This is a longer string that
     spans multiple lines
     """
     ```

     + 可以用`count()`统计相关字符个数：

     ```py
     In [55]: c.count('\n')
     Out[55]: 3
     ```

     + 字符串是不可变的，不能修改字符串

     ```py
     In [56]: a = 'this is a string'
     In [57]: a[10] = 'f'	# 报错
     
     In [58]: b = a.replace('string', 'longer string')
     
     In [59]: b
     Out[59]: 'this is a longer string'
     
     In [60]: a
     Out[60]: 'this is a string'
     ```

     + 用`r'...'`来消除转义的影响

     ```py
     In [69]: s = r'this\has\no\special\characters'
     
     In [70]: s
     Out[70]: 'this\\has\\no\\special\\characters'
     ```

     + 字符串的模板化

     ```py
     In [74]: template = '{0:.2f} {1:s} are worth US${2:d}'
     
     In [75]: template.format(4.5560, 'Argentine Pesos', 1)
     Out[75]: '4.56 Argentine Pesos are worth US$1'
     ```

   + 字节和Unicode

     + `encode()`将Unicode表示的str转化为指定的bytes
     + `decode()`将bytes转换为指定字符的str



9. 日期和时间

```py
In [102]: from datetime import datetime, date, time

In [103]: dt = datetime(2011, 10, 29, 20, 30, 21)

In [104]: dt.day
Out[104]: 29

In [105]: dt.minute
Out[105]: 30

In [106]: dt.date()
Out[106]: datetime.date(2011, 10, 29)

In [107]: dt.time()
Out[107]: datetime.time(20, 30, 21)
```

+ `strftime`方法可以将datetime格式化为字符串：

```py
In [108]: dt.strftime('%m/%d/%Y %H:%M')
Out[108]: '10/29/2011 20:30'
```

+ `strptime`可以将字符串转换成`datetime`对象：

```py
In [109]: datetime.strptime('20091031', '%Y%m%d')
Out[109]: datetime.datetime(2009, 10, 31, 0, 0)
```

+ 所有格式化命令

![表2-5 Datetime格式化指令（与ISO C89兼容）](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407181619944.webp)



10. 三元表达式

```py
value = true-expr if condition else false-expr
```



## 第3章 Python的数据结构、函数和文件

### 3.1 数据结构和序列

1. 元组tuple

   + 拆分元组 `*`

   ```py
   In [29]: values = 1, 2, 3, 4, 5
   # 许多Python程序员会将不需要的变量使用下划线
   In [30]: a, b, *_ = values
   
   In [31]: a, b
   Out[31]: (1, 2)
   
   In [32]: _
   Out[32]: [3, 4, 5]
   ```

   + tuple方法: `count()`统计个数

   ```py
   In [34]: a = (1, 2, 2, 2, 3, 4, 2)
   
   In [35]: a.count(2)
   Out[35]: 4
   ```

2. 列表list

   + 添加 / 删除元素

     + `append()`：在末尾添加元素
     + `insert(index, value)`：在特定的位置插入元素
     + `pop()`：没有参数即pop最后一个值；有参数则代表pop元素的下标
     + `remove(value)`：会寻找第一个value值并除去

   + 判断存在

     + 用 `in` 或者 `not in`

   + 串联和组合列表

     + 串联用`+`
     + 用`extend()`方法

     ```py
     In [58]: x = [4, None, 'foo']
     
     In [59]: x.extend([7, 8, (2, 3)])
     
     In [60]: x
     Out[60]: [4, None, 'foo', 7, 8, (2, 3)]
     ```

   + 排序

     + `sort()`：将一个列表原地排序（不创建新的对象）

       ```py
       In [64]: b = ['saw', 'small', 'He', 'foxes', 'six']
       # 可以用二级排序key来进行排序
       In [65]: b.sort(key=len)
       
       In [66]: b
       Out[66]: ['He', 'saw', 'six', 'small', 'foxes']
       ```

   + 二分搜索

     + `bisect.bisect`可以找到插入值后仍保证排序的位置
     + `bisect.insort`是向这个位置插入值

     ```py
     In [67]: import bisect
     
     In [68]: c = [1, 2, 2, 2, 3, 4, 7]
     
     In [69]: bisect.bisect(c, 2)
     Out[69]: 4
     
     In [70]: bisect.bisect(c, 5)
     Out[70]: 6
     
     In [71]: bisect.insort(c, 6)
     
     In [72]: c
     Out[72]: [1, 2, 2, 2, 3, 4, 6, 7]
     ```

     > 注意：`bisect`模块不会检查列表是否已排好序，进行检查的话会耗费大量计算。因此，对未排序的列表使用`bisect`不会产生错误，但结果不一定正确。

   + 切片

     + 切片也可以被序列赋值

     ```py
     In [74]: seq = [7, 2, 3, 7, 5, 6, 0, 1]
     In [75]: seq[3:4] = [6, 3]
     
     In [76]: seq
     Out[76]: [7, 2, 3, 6, 3, 5, 6, 0, 1]
     ```

     + 负数表明从后向前切片

     ```py
     # Out[76]: [7, 2, 3, 6, 3, 5, 6, 0, 1]
     In [79]: seq[-4:]
     Out[79]: [5, 6, 0, 1]
     
     In [80]: seq[-6:-2]
     Out[80]: [6, 3, 5, 6]
     ```

     + 在第二个冒号后面使用`step`，可以隔几个取元素

     ```py
     In [81]: seq[::2]
     Out[81]: [7, 3, 3, 6, 1]
     
     In [82]: seq[::-1]
     Out[82]: [1, 0, 6, 5, 3, 6, 3, 2, 7]
     ```

   + 序列函数

     + enumerate函数

     ```py
     for i, value in enumerate(collection):
        # do something with value
     ```

     + sorted函数：可以从任意序列的元素返回一个新的排好序的列表（新列表！）

     ```py
     In [87]: sorted([7, 1, 2, 6, 0, 3, 2])
     Out[87]: [0, 1, 2, 2, 3, 6, 7]
     
     In [88]: sorted('horse race')
     Out[88]: [' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']
     ```

     + zip函数：将多个列表、元组或其它序列成对组合成一个**元组列表**

     ```py
     In [89]: seq1 = ['foo', 'bar', 'baz']
     
     In [90]: seq2 = ['one', 'two', 'three']
     
     In [91]: zipped = zip(seq1, seq2)
     
     In [92]: list(zipped)
     Out[92]: [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
     ```

     ```py
     # 元素的个数取决于最短的序列
     In [93]: seq3 = [False, True]
     
     In [94]: list(zip(seq1, seq2, seq3))
     Out[94]: [('foo', 'one', False), ('bar', 'two', True)]
     ```

     ```py
     # zip可以被用来解压序列，也可以当作把行的列表转换为列的列表
     In [96]: pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),
        ....:             ('Schilling', 'Curt')]
     
     In [97]: first_names, last_names = zip(*pitchers)
     
     In [98]: first_names
     Out[98]: ('Nolan', 'Roger', 'Schilling')
     
     In [99]: last_names
     Out[99]: ('Ryan', 'Clemens', 'Curt')
     ```

     + reversed函数

     ```py
     # reversed可以从后向前迭代一个序列
     In [100]: list(reversed(range(10)))
     Out[100]: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
     ```

     > 要记住`reversed`是一个生成器，只有实体化（即列表或for循环）之后才能创建翻转的序列。

3. 字典

   + 删除值

     + `del`关键字

     ```py
     In [111]: d1
     Out[111]: 
     {'a': 'some value',
      'b': [1, 2, 3, 4],
      7: 'an integer',
      5: 'some value',
      'dummy': 'another value'}
     
     In [112]: del d1[5]
     
     In [113]: d1
     Out[113]: 
     {'a': 'some value',
      'b': [1, 2, 3, 4],
      7: 'an integer',
      'dummy': 'another value'}
     ```

     + `pop()`方法：返回值的同时删除键

     ```py
     # 书接上回
     In [114]: ret = d1.pop('dummy')
     
     In [115]: ret
     Out[115]: 'another value'
     
     In [116]: d1
     Out[116]: {'a': 'some value', 'b': [1, 2, 3, 4], 7: 'an integer'}
     ```

   + 迭代

     + `keys（）`
     + `values()`
     + `items()`

     ```py
     In [117]: list(d1.keys())
     Out[117]: ['a', 'b', 7]
     
     In [118]: list(d1.values())
     Out[118]: ['some value', [1, 2, 3, 4], 'an integer']
     ```

   + 融合

     + 用`update()`方法，原地改变字典

     ```py
     In [119]: d1.update({'b' : 'foo', 'c' : 12})
     
     In [120]: d1
     Out[120]: {'a': 'some value', 'b': 'foo', 7: 'an integer', 'c': 12}
     ```

   + 用序列创建字典

     + 反正记住`mapping[key] = value`

   + 默认值

     + `get()`
       + `value = some_dict.get(key, default_value)`
     + `defaultdict（type）`
       + `by_letter = defaultdict(list)`

   + 有效的键类型

     + 字典的值可以是任意Python对象，而**键**通常是不可变的标量类型（整数、浮点型、字符串）或元组
     + 可以用`hash`函数检测一个对象是否是可哈希的（可被用作字典的键）

     ```py
     In [127]: hash('string')
     Out[127]: 5023931463650008331
     
     In [128]: hash((1, 2, (2, 3)))
     Out[128]: 1097636502276347782
     
     In [129]: hash((1, 2, [2, 3])) # fails because lists are mutable
     ---------------------------------------------------------------------------
     TypeError                                 Traceback (most recent call last)
     <ipython-input-129-800cd14ba8be> in <module>()
     ----> 1 hash((1, 2, [2, 3])) # fails because lists are mutable
     TypeError: unhashable type: 'list'
     ```

     ```py
     # 解决方法是将列表转化为元组tuple()
     In [130]: d = {}
     
     In [131]: d[tuple([1, 2, 3])] = 5
     
     In [132]: d
     Out[132]: {(1, 2, 3): 5}
     ```

4. 集合：集合是无序的不可重复的元素的集合。你可以把它当做字典，但是只有键没有值。

   + 创建

   ```py
   In [133]: set([2, 2, 2, 1, 3, 3])
   Out[133]: {1, 2, 3}
   
   In [134]: {2, 2, 2, 1, 3, 3}
   Out[134]: {1, 2, 3}
   ```

   + 运算

     ```py
     In [135]: a = {1, 2, 3, 4, 5}
     
     In [136]: b = {3, 4, 5, 6, 7, 8}
     ```

     + 合并：用`union`方法，或者`|`运算符

       ```py
       In [137]: a.union(b)
       Out[137]: {1, 2, 3, 4, 5, 6, 7, 8}
       
       In [138]: a | b
       Out[138]: {1, 2, 3, 4, 5, 6, 7, 8}
       ```

     + 交集：用`intersection`或`&`运算符

       ```py
       In [139]: a.intersection(b)
       Out[139]: {3, 4, 5}
       
       In [140]: a & b
       Out[140]: {3, 4, 5}
       ```

   + 常用的集合方法

     + 图

     ![表3-1 Python的集合操作](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202407182202166.webp)

   + 列表 转 集合

     ```py
     In [147]: my_data = [1, 2, 3, 4]
     
     In [148]: my_set = {tuple(my_data)}
     
     In [149]: my_set
     Out[149]: {(1, 2, 3, 4)}
     ```

     + 检测一个集合是否是另一个集合的子集或父集：`issubset()` 或 `issuperset()`

     ```py
     In [150]: a_set = {1, 2, 3, 4, 5}
     
     In [151]: {1, 2, 3}.issubset(a_set)
     Out[151]: True
     
     In [152]: a_set.issuperset({1, 2, 3})
     Out[152]: True
     ```

5. 推导式

   + 列表推导式

   ```py
   [expr for val in collection if condition]
   
   # 举例
   In [154]: strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
   
   In [155]: [x.upper() for x in strings if len(x) > 2]
   Out[155]: ['BAT', 'CAR', 'DOVE', 'PYTHON']
   ```

   + 字典推导式

   ```py
   dict_comp = {key-expr : value-expr for value in collection if condition}
   
   # 举例
   In [159]: loc_mapping = {val : index for index, val in enumerate(strings)}
   
   In [160]: loc_mapping
   Out[160]: {'a': 0, 'as': 1, 'bat': 2, 'car': 3, 'dove': 4, 'python': 5}
   ```

   + 集合推导式

   ```py
   set_comp = {expr for value in collection if condition}
   
   # 举例
   In [156]: unique_lengths = {len(x) for x in strings}
   
   In [157]: unique_lengths
   Out[157]: {1, 2, 3, 4, 6}
   
   # map函数可以进一步简化
   In [158]: set(map(len, strings))
   Out[158]: {1, 2, 3, 4, 6}
   ```

   ```py
   # 关于map()函数
   # map()函数会返回一个迭代器，我们通常可以将其转换为列表或元组来查看结果
   map(function, iterable)
   
   def square(x):  
       return x * x  
   
   numbers = [1, 2, 3, 4, 5]  
   squared_numbers = map(square, numbers)  
   
   squared_numbers_list = list(squared_numbers)  
   print(squared_numbers_list)  # 输出 [1, 4, 9, 16, 25]
   ```

   + 生成器表达式
   
     ```py
     In [189]: gen = (x ** 2 for x in range(100))
     
     In [190]: gen
     Out[190]: <generator object <genexpr> at 0x7fbbd5ab29e8>
     
     # 等价于
     def _make_gen():
         for x in range(100):
             yield x ** 2
     gen = _make_gen()
     
     # 生成器表达式也可以取代列表推导式，作为函数参数
     In [191]: sum(x ** 2 for x in range(100))
     Out[191]: 328350
     
     In [192]: dict((i, i **2) for i in range(5))
     Out[192]: {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
     ```
   
     

### 3.2 函数

1. 在函数中对全局变量进行赋值操作，但是那些变量必须用global关键字声明成全局

```py
In [168]: a = None

In [169]: def bind_a_variable():
   .....:     global a
   .....:     a = []
   .....: bind_a_variable()
   .....:

In [170]: print(a)
[]
```

> 注意：我常常建议人们不要频繁使用global关键字。因为全局变量一般是用于存放系统的某些状态的。如果你发现自己用了很多，那可能就说明得要来点儿面向对象编程了（即使用类）。

2. 可以返回多值，也可以返回字典
3. 使用内建字符串方法 和 正则表达式re 来处理数据

```py
import re

def clean_strings(strings):
    result = []
    for value in strings:
        # 使用strip()方法删除字符串的前导和尾随空格
        value = value.strip()
        # 使用re.sub('[!#?]', '', value)删除字符串中的任何'!'、'#'或'?'
        # 第一个参数是正则表达式，第二个参数是要替换的字符串，第三个参数是要被sub的字符串
        value = re.sub('[!#?]', '', value)
        # 使用title()方法将每个单词的首字母大写
        value = value.title()
        result.append(value)
    return result


In [171]: states = ['   Alabama ', 'Georgia!', 'Georgia', 'georgia', 'FlOrIda',
   .....:           'south   carolina##', 'West virginia?']

In [173]: clean_strings(states)
Out[173]: 
['Alabama',
 'Georgia',
 'Georgia',
 'Georgia',
 'Florida',
 'South   Carolina',
 'West Virginia']
```

法二：

```py
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)

clean_ops = [str.strip, remove_punctuation, str.title]

def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result
```

4. 匿名（lambda）函数

```py
def short_function(x):
    return x * 2

equiv_anon = lambda x: x * 2
```

```py
def add_numbers(x, y):
    return x + y
    
# 柯里化    
add_five = lambda y: add_numbers(5, y)

# partial函数
from functools import partial
add_five = partial(add_numbers, 5)	# == partial(add_numbers, x=5)
```

5. 生成器

```py
In [182]: dict_iterator = iter(some_dict)

In [183]: dict_iterator
Out[183]: <dict_keyiterator at 0x7fbbd5a9f908>

In [184]: list(dict_iterator)
Out[184]: ['a', 'b', 'c']
```

一般的函数执行之后只会返回单个值，而生成器则是以延迟的方式返回一个值序列，即每返回一个值之后暂停，直到下一个值被请求时再继续。

要创建一个生成器，只需将函数中的**return替换为yeild**即可：

```py
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i ** 2

# 调用该生成器时，没有任何代码会被立即执行
In [186]: gen = squares()

In [187]: gen
Out[187]: <generator object squares at 0x7fbbd5ab4570>

# 直到你从该生成器中请求元素时，它才会开始执行其代码
In [188]: for x in gen:
   .....:     print(x, end=' ')
Generating squares from 1 to 100
1 4 9 16 25 36 49 64 81 100
```

6. itertools模块

标准库itertools模块中有一组用于许多常见数据算法的生成器。

例如，`groupby`可以接受任何序列和一个函数。它根据函数的返回值对序列中的连续元素进行分组。

```py
In [193]: import itertools

In [194]: first_letter = lambda x: x[0]

In [195]: names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']

In [196]: for letter, names in itertools.groupby(names, first_letter):
   .....:     print(letter, list(names)) # names is a generator
A ['Alan', 'Adam']
W ['Wes', 'Will']
A ['Albert']
S ['Steven']
```

![表3-2 一些有用的itertools函数](https://camo.githubusercontent.com/2a09a3e3549ba0224107daf01b9b4e3232e6da2730e7447f9b4a35167bb3634f/687474703a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f373137383639312d313131383233643837363761313034642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

6. 错误和异常处理

   + `ValueError`错误

     `TypeError`错误（输入不是字符串或数值）

   ```py
   In [197]: float('1.2345')
   Out[197]: 1.2345
   
   In [198]: float('something')
   ---------------------------------------------------------------------------
   ValueError                                Traceback (most recent call last)
   <ipython-input-198-439904410854> in <module>()
   ----> 1 float('something')
   ValueError: could not convert string to float: 'something'
   
   In [204]: attempt_float((1, 2))
   ---------------------------------------------------------------------------
   TypeError                                 Traceback (most recent call last)
   <ipython-input-204-9bdfd730cead> in <module>()
   ----> 1 attempt_float((1, 2))
   <ipython-input-203-3e06b8379b6b> in attempt_float(x)
         1 def attempt_float(x):
         2     try:
   ----> 3         return float(x)
         4     except ValueError:
         5         return x
   TypeError: float() argument must be a string or a number, not 'tuple'
   ```

   处理错误：`try / except`

   无论try部分的代码是否成功，都执行一段代码，可以使用`finally`

   ```py
   def attempt_float(x):
       try:
           return float(x)
       # 当float(x)抛出异常时，才会执行except的部分
       except (TypeError, ValueError):
           return x
    
   # test
   In [200]: attempt_float('1.2345')
   Out[200]: 1.2345
   
   In [201]: attempt_float('something')
   Out[201]: 'something'
   ```

7. IPython的异常

如果是在%run一个脚本或一条语句时抛出异常，IPython默认会打印完整的调用栈（traceback），在栈的每个点都会有几行上下文：

```py
In [10]: %run examples/ipython_bug.py
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
/home/wesm/code/pydata-book/examples/ipython_bug.py in <module>()
     13     throws_an_exception()
     14
---> 15 calling_things()

/home/wesm/code/pydata-book/examples/ipython_bug.py in calling_things()
     11 def calling_things():
     12     works_fine()
---> 13     throws_an_exception()
     14
     15 calling_things()

/home/wesm/code/pydata-book/examples/ipython_bug.py in throws_an_exception()
      7     a = 5
      8     b = 6
----> 9     assert(a + b == 10)
     10
     11 def calling_things():

AssertionError:
```

自身就带有文本是相对于Python标准解释器的极大优点。

你可以用魔术命令`%xmode`，从Plain（与Python标准解释器相同）到Verbose（带有函数的参数值）控制文本显示的数量。

后面可以看到，发生错误之后，（用%debug或%pdb magics）可以进入stack进行事后调试。



### 3.3 文件和操作系统

1. 

打开文件：`open()`

关闭文件：如果使用open创建文件对象，一定要用`close()`关闭它

```py
In [207]: path = 'examples/segismundo.txt'

# 默认情况下，文件是以只读模式（'r'）打开的
In [208]: f = open(path)

In [211]: f.close()
```

读取文件：从文件中取出的行都带有完整的行结束符（EOL）

```py
# 得到一组没有EOL的行
# restrip()用于去除字符串末尾的空白字符（包括空格、制表符和换行符）或指定的字符
In [209]: lines = [x.rstrip() for x in open(path)]

In [210]: lines
Out[210]: 
['Sueña el rico en su riqueza,',
 'que más cuidados le ofrece;',
 '',
 'sueña el pobre que padece',
 'su miseria y su pobreza;',
 '',
 'sueña el que a medrar empieza,',
 'sueña el que afana y pretende,',
 'sueña el que agravia y ofende,',
 '',
 'y en el mundo, en conclusión,',
 'todos sueñan lo que son,',
 'aunque ninguno lo entiende.',
 '']
```

用`with语句`可以可以更容易地清理打开的文件，这样可以在退出代码块时，自动关闭文件。

```py
In [212]: with open(path) as f:
   .....:     lines = [x.rstrip() for x in f]
```



2. 

![表3-3 Python的文件模式](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202408021737411.webp)

+ w文件模式：输入f =open(path,'w')，就会有一个新文件被创建在examples/segismundo.txt，并覆盖掉该位置原来的任何数据。
+ x文件模式：它可以创建可写的文件，但是如果文件路径存在，就无法创建。



3. 

对于可读文件，一些常用的方法是`read、seek和tell`

+ `read`会从文件返回字符
  + 字符的内容是由文件的编码决定的（如UTF-8），如果是二进制模式打开的就是原始字节

```py
In [213]: f = open(path)

In [214]: f.read(10)
Out[214]: 'Sueña el r'

In [215]: f2 = open(path, 'rb')  # Binary mode

In [216]: f2.read(10)
Out[216]: b'Sue\xc3\xb1a el '
```

+ `tell`可以给出当前的位置

```py
In [217]: f.tell()
Out[217]: 11

In [218]: f2.tell()
Out[218]: 10
```

+ `seek`将文件位置更改为文件中的指定字节

```py
In [221]: f.seek(3)
Out[221]: 3

In [222]: f.read(1)
Out[222]: 'ñ'

# 最后关闭文件
In [223]: f.close()

In [224]: f2.close()
```



可以使用文件的`write或writelines`方法向文件写入

```py
# 创建一个无空行版的prof_mod.py
In [225]: with open('tmp.txt', 'w') as handle:
   .....:     handle.writelines(x for x in open(path) if len(x) > 1)

In [226]: with open('tmp.txt') as f:
   .....:     lines = f.readlines()

In [227]: lines
Out[227]: 
['Sueña el rico en su riqueza,\n',
 'que más cuidados le ofrece;\n',
 'sueña el pobre que padece\n',
 'su miseria y su pobreza;\n',
 'sueña el que a medrar empieza,\n',
 'sueña el que afana y pretende,\n',
 'sueña el que agravia y ofende,\n',
 'y en el mundo, en conclusión,\n',
 'todos sueñan lo que son,\n',
 'aunque ninguno lo entiende.\n']
```



常用的文件方法：

![表3-4 Python重要的文件方法或属性](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202408021746613.webp)



4. 文件的字节和Unicode

   + UTF-8是长度可变的Unicode编码，所以当我从文件请求一定数量的字符时，Python会从文件读取**足够多**（可能少至10或多至40字节）的字节进行解码

     + 取决于文本的编码，你可以将字节解码为str对象，但只有当**每个编码的Unicode字符都完全成形**时才能这么做

     ```py
     In [234]: data.decode('utf8')
     Out[234]: 'Sueña el '
     
     In [235]: data[:4].decode('utf8')
     ---------------------------------------------------------------------------
     UnicodeDecodeError                        Traceback (most recent call last)
     <ipython-input-235-300e0af10bb7> in <module>()
     ----> 1 data[:4].decode('utf8')
     UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc3 in position 3: unexpecte
     d end of data
     ```

     + 一种更方便的方法将Unicode转换为另一种编码

     ```py
     In [236]: sink_path = 'sink.txt'
     
     In [237]: with open(path) as source:
        .....:     with open(sink_path, 'xt', encoding='iso-8859-1') as sink:
        .....:         sink.write(source.read())
     
     In [238]: with open(sink_path, encoding='iso-8859-1') as f:
        .....:     print(f.read(10))
     Sueña el r
     ```

   + 如果以“rb”模式打开文件，则读取**确切**的请求字节数

     + 注意，**不要在二进制模式中使用seek**
     + 如果文件位置位于定义Unicode字符的字节的中间位置，读取后面会产生错误

     ```py
     In [240]: f = open(path)
     
     In [241]: f.read(5)
     Out[241]: 'Sueña'
     
     In [242]: f.seek(4)
     Out[242]: 4
     
     In [243]: f.read(1)
     ---------------------------------------------------------------------------
     UnicodeDecodeError                        Traceback (most recent call last)
     <ipython-input-243-7841103e33f5> in <module>()
     ----> 1 f.read(1)
     /miniconda/envs/book-env/lib/python3.6/codecs.py in decode(self, input, final)
         319         # decode input (taking the buffer into account)
         320         data = self.buffer + input
     --> 321         (result, consumed) = self._buffer_decode(data, self.errors, final
     )
         322         # keep undecoded input until the next call
         323         self.buffer = data[consumed:]
     UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb1 in position 0: invalid s
     tart byte
     
     In [244]: f.close()
     ```

     

## 第4章 NumPy基础：数组和矢量计算

### 4.1 NumPy的ndarray：一种多维数组对象

1. 引入NumPy生成一个包含随机数据的小数组

```py
In [12]: import numpy as np

# Generate some random data
In [13]: data = np.random.randn(2, 3)

In [14]: data
Out[14]: 
array([[-0.2047,  0.4789, -0.5194],
       [-0.5557,  1.9658,  1.3934]])

# 进行数学运算
In [15]: data * 10
Out[15]: 
array([[ -2.0471,   4.7894,  -5.1944],
       [ -5.5573,  19.6578,  13.9341]])

In [16]: data + data
Out[16]: 
array([[-0.4094,  0.9579, -1.0389],
       [-1.1115,  3.9316,  2.7868]])
```

2. ndarray的创建

   所有元素必须是相同类型

   每个数组都有一个**shape**（一个表示各维度大小的元组）和一个**dtype**（一个用于说明数组数据类型的对象）

   ```py
   In [17]: data.shape
   Out[17]: (2, 3)
   
   In [18]: data.dtype
   Out[18]: dtype('float64')
   ```

   + 创建ndarray

     + `np.array()`：接受一切序列型的对象（包括其他数组），然后产生一个新的含有传入数据的NumPy数组

     ```py
     In [19]: data1 = [6, 7.5, 8, 0, 1]
     
     In [20]: arr1 = np.array(data1)
     
     In [21]: arr1
     Out[21]: array([ 6. ,  7.5,  8. ,  0. ,  1. ])
     
     In [27]: arr1.dtype
     Out[27]: dtype('float64')
     ```

     嵌套序列（比如由一组等长列表组成的列表）将会被转换为一个多维数组

     ```py
     In [22]: data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
     
     In [23]: arr2 = np.array(data2)
     
     In [24]: arr2
     Out[24]: 
     array([[1, 2, 3, 4],
            [5, 6, 7, 8]])
     
     # ndim用于表示数组的维度（即轴的数量）
     In [25]: arr2.ndim
     Out[25]: 2
     
     In [26]: arr2.shape
     Out[26]: (2, 4)
     
     In [28]: arr2.dtype
     Out[28]: dtype('int64')
     ```

     + `zeros()`和`ones()`分别可以创建指定长度或形状的全0或全1数组
     + `empty()`只分配内存空间，不存储任何值

     ```py
     In [29]: np.zeros(10)
     Out[29]: array([ 0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.,  0.])
     
     In [30]: np.zeros((3, 6))
     Out[30]: 
     array([[ 0.,  0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.,  0.],
            [ 0.,  0.,  0.,  0.,  0.,  0.]])
     
     In [31]: np.empty((2, 3, 2))
     Out[31]: 
     array([[[ 0.,  0.],
             [ 0.,  0.],
             [ 0.,  0.]],
            [[ 0.,  0.],
             [ 0.,  0.],
             [ 0.,  0.]]])
     ```

   `arange()`是Python内置函数`range`的数组版

   ```py
   In [32]: np.arange(15)
   Out[32]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
   ```

   ![表4-1 数组创建函数](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202408022047788.webp)

3. ndarray的数据类型

```py
In [33]: arr1 = np.array([1, 2, 3], dtype=np.float64)

In [34]: arr2 = np.array([1, 2, 3], dtype=np.int32)

In [35]: arr1.dtype
Out[35]: dtype('float64')

In [36]: arr2.dtype
Out[36]: dtype('int32')
```

![img](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202408022050925.webp)

![img](https://raw.githubusercontent.com/bobo0214/typora_pic/main/img/202408022051929.webp)

ndarray的`astype`方法明确地将一个数组从一个dtype转换成另一个dtype

**调用astype总会创建一个新的数组**（一个数据的备份），即使新的dtype与旧的dtype相同

```py
In [37]: arr = np.array([1, 2, 3, 4, 5])

In [38]: arr.dtype
Out[38]: dtype('int64')

In [39]: float_arr = arr.astype(np.float64)

In [40]: float_arr.dtype
Out[40]: dtype('float64')
```

```py
# 举例1
In [41]: arr = np.array([3.7, -1.2, -2.6, 0.5, 12.9, 10.1])

In [42]: arr
Out[42]: array([  3.7,  -1.2,  -2.6,   0.5,  12.9,  10.1])

In [43]: arr.astype(np.int32)
Out[43]: array([ 3, -1, -2,  0, 12, 10], dtype=int32)

# 举例2：字符串 → 数值
# 注意：使用numpy.string_类型时，一定要小心，因为NumPy的字符串数据是大小固定的，发生截取时，不会发出警告。
In [44]: numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)

# 此处写的是float而不是np.float64，numpy会将Python类型映射到等价的dtype上
In [45]: numeric_strings.astype(float)
Out[45]: array([  1.25,  -9.6 ,  42.  ])
```

```py
# 举例3
In [46]: int_array = np.arange(10)

In [47]: calibers = np.array([.22, .270, .357, .380, .44, .50], dtype=np.float64)

In [48]: int_array.astype(calibers.dtype)
Out[48]: array([ 0.,  1.,  2.,  3.,  4.,  5.,  6.,  7.,  8.,  9.])

# 举例4
In [49]: empty_uint32 = np.empty(8, dtype='u4')

In [50]: empty_uint32
Out[50]: 
array([         0, 1075314688,          0, 1075707904,          0,
       1075838976,          0, 1072693248], dtype=uint32)
```

4. NumPy数组的运算：数组与标量的算术运算会将标量值传播到各个元素

```py
In [55]: 1 / arr
Out[55]: 
array([[ 1.    ,  0.5   ,  0.3333],
       [ 0.25  ,  0.2   ,  0.1667]])

In [56]: arr ** 0.5
Out[56]: 
array([[ 1.    ,  1.4142,  1.7321],
       [ 2.    ,  2.2361,  2.4495]])
```

​	大小相同的数组之间的比较会生成**布尔值数组**

```py
In [57]: arr2 = np.array([[0., 4., 1.], [7., 2., 12.]])

In [58]: arr2
Out[58]: 
array([[  0.,   4.,   1.],
       [  7.,   2.,  12.]])

In [59]: arr2 > arr
Out[59]:
array([[False,  True, False],
       [ True, False,  True]], dtype=bool)
```



5. 基本的索引和切片



### 4.2 通用函数：快速的元素级数组函数

### 4.3 利用数组进行数据处理

### 4.4 用于数组的文件输入输出

### 4.5 线性代数

### 4.6 伪随机数生成

### 4.7 示例：随机漫步
