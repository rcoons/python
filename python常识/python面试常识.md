## python面试基本常识

### 1.Python解释器种类以及特点

CPython

**c语言开发的 使用最广的解释器**

IPython

**基于cpython之上的一个交互式计时器 交互方式增强 功能和cpython一样**

PyPy

**目标是执行效率 采用JIT技术 对python代码进行动态编译，提高执行效率**

JPython

**运行在Java上的解释器 直接把python代码编译成Java字节码执行**

IronPython

**运行在微软 .NET 平台上的解释器，把python编译成. NET 的字节码**

### 2.python的递归深度

#### 递归深度

**Python语言默认的递归深度是很有限的，当递归深度超过值的时候，就会引发RuntimeError异常****

**Python专门设置的一种机制用来防止无限递归造成Python溢出，这个值理论上1000，实际运行时在900多次时就会报错**

#### 解决方案

**最大递归次数是可以重新调整的。解决的方式是手工设置递归调用深度：**

```python
import sys 
sys.setrecursionlimit(1000000) #执行这个代码后，递归深度调整到1000000层，基本上够用了
```

**其他办法：改成非递归**

### 3.python的深浅拷贝

#### 定义

**深拷贝的时候python将所有数据在内存中新建了一份，所以如果你修改新的模版的时候老模版不会变。相反，在浅copy 的时候，python仅仅将最外层的内容在内存中新建了一份出来，第二层并没有在内存中新建，所以你修改了新模版，默认模版也被修改了**

**下面我们先看一个具体的例子**

#### 浅拷贝

```python
import copy
wife = {'name':{'diaoq':30},'slaras':10000}
hasband = copy.copy(wife) 
print(hasband,wife)
# {'name': {'diaoq': 30}, 'slaras': 10000}   {'name': {'diaoq': 30}, 'slaras': 10000}
hasband['name']['diaoq']= 32
hasband['slaras'] = 12000
print(hasband)
# {'slaras': 12000, 'name': {'diaoq': 32}}
print(wife)
# {'slaras': 12000, 'name': {'dioq': 32}}
```

#### 深拷贝

```python
import copy
wife = {'name':{'diaoq':30},'slaras':10000}
hasband = copy.deepcopy(wife) 
print(hasband,wife)
# {'name': {'diaoq': 30}, 'slaras': 10000}   {'name': {'diaoq': 30}, 'slaras': 10000}
hasband['name']['diaoq']= 32
hasband['slaras'] = 12000
print(hasband)
# {'slaras': 12000, 'name': {'diaoq': 32}}
print(wife)
# {'slaras': 10000, 'name': {'diaoq': 30}}
```

#### 总结

1. 浅拷贝时，修改的元素类型是可变类型时，他变我也变，修改的类型是不可变类型时，他变我不变
2. 深拷贝时，他变我不变

### 4.  python的匿名函数

1. 为了解决那些功能很简单的需求而设计的一句话函数

2. 函数名 = lambda   参数 ：返回值

3. 匿名函数不管逻辑多复杂，只能写一行，且逻辑执行结束后的内容就是返回值

   ```python
   temp = lambda x,y:x+y
   print(temp(4,10))   # 14
   
   可以替代：
   def foo(x,y):
       return x+y
   print(foo(4,10))    # 14
   ```

###  5.  装饰器

**装饰器(Decorators)是 Python 的一个重要部分。简单地说：他们是修改其他函数的功能的函数。他们有助于让我们的代码更简短，装饰器本质上是一个高阶函数，所以装饰器也可以有自己的参数**

#### 日志装饰器

**日志是装饰器运用的另一个亮点**

```python
from functools import wraps
 
def logit(func):
    @wraps(func)
    def with_logging(*args, **kwargs):
        print(func.__name__ + " was called")
        return func(*args, **kwargs)
    return with_logging
 
@logit
def addition_func(x):
   """Do some math."""
   return x + x

result = addition_func(4)
# Output: addition_func was called
```

#### wraps作用

**被装饰后的函数其实已经是另外一个函数了（函数名等函数属性会发生改变），所以，Python的functools包中提供了一个叫wraps的decorator来消除这样的副作用。写一个decorator的时候，最好在实现之前加上functools的wrap，它能保留原有函数的名称和docstring**

### 6.  \*args，\**kwargs？参数是什么？

**如果我们不确定要往函数中传入多少个参数，或者我们想往函数中以列表和元组的形式传参数时，那就使要用\*args；如果我们不知道要往函数中传入多少个关键词参数，或者想传入字典的值作为关键词参数时，那就要使用\*\*kwargs**

### 7.python中的负号“-”

**列表**

```python
list1 = [1,2,3,4]

#output: 4
print(list1[-1])
```

**字符串**

```python
ss = "abcd"

#output: abc
#打印字符串长度len（ss）-1的内容
print(ss[:-1])

#output: dcba
#字符串反转，-1，表示依次反转
print(ss[::-1])

#output: db
#字符串反转，-2，表示相隔一个字符反转
print(ss[::-2])
```
### 8. python单下划线和双下划线

#### 单下划线

**常用于模块中，在一个模块中以单下划线开头的变量和方法会被默认划入模块内部范围。当使用 from my_module import * 导入时，单下划线开头的变量和方法是不会被导入的。但使用 import my_module 导入的话，仍然可以用 my_module._var 这样的形式访问属性或方法**

#### 双下划线

**双下划线开头和结尾的是一些 python 的“魔术”对象，如类成员的 \_\_init\_\_、\_\_del\__、\_\_add\_\_、\_\_getitem\_\_ 等，以及全局的\_\_file\_\_、\_\_name\_\_等,多用来表示私有变量或类**

### 9.python的内存管理

**Python的内存管理是由私有heap空间管理的。所有的Python对象和数据结构都在一个私有heap中。程序员没有访问该heap的权限，只有解释器才能对它进行操作。为Python的heap空间分配内存是由Python的内存管理模块进行的，其核心API会提供一些访问该模块的方法供程序员使用。Python有自带的垃圾回收系统，它回收并释放没有被使用的内存，让它们能够被其他程序使用**

### 10. 多进程与多线程的区别

**1. 一个程序至少有一个进程，一个进程至少有一个线程**

**2.线程的划分尺度小于进程，使得多线程程序的并发性高**

**3.进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率**

**4.线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制**

**5.从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看做多个独立的应用，来实现进程的调度和管理以及资源分配。这就是进程和线程的重要区别**

### 11.文件操作：read,readline和readlines

#### read

**读取整个文件将文件内容放到一个字符串变量中，劣势是：如果文件非常大，尤其是大于内存时，无法使用read()方法**

#### readline

**每次读取一行；返回的是一个字符串对象，保持当前行的内存，缺点：比readlines慢得多**

#### readlines

**一次性读取整个文件；自动将文件内容分析成一个行的列表**

### 12.如何判断单向链表中是否有坏

**首先遍历链表，寻找是否有相同地址，借此判断链表中是否有环。如果程序进入死循环，则需要一块空间来存储指针，遍历新指针时将其和储存的旧指针比对，若有相同指针，则该链表有环，否则将这个新指针存下来后继续往下读取，直到遇见NULL，这说明这个链表无环**

### 13.sort和sorted的区别

**1.sorted执行之后返回一个新的列表，sort是在原有的列表做修改**

**2. sorted 可以对更多组合排序，sort只能对列表排序**

```python

list1 = [1,3,2,6,4]
list1.sort()
print(list1)# [1,2,3,4,6]

d = {1: 1000, 4: 250, 2: 750, 3: 500}
d2 = sorted(d.items())
print(d2) # [(1, 1000), (2, 750), (3, 500), (4, 250)]
```

可以看到，sorted可以对元祖进行排序

### 14 列表解析式

**列表解析式又叫列表推导式，和标准的python循环相比，不仅可读性强，而且需要的代码量也是最少的，执行速度也是比标准循环快乐不少，对于列表推导式，我们可以从列表中选择具体的元素，并做一些操作和判断，从而创建新的列表。值得注意的是，我们甚至能使用 Pandas Series 或 NumPy Array 进行列表推导操作，废话不多说，直接上代码**

```python

#标准的循环模式
list1 = [1,3,2,6,4]
list2 = []
for one in list1:
    list2.append(one)
    
#列表推导式
list1 = [1,3,2,6,4]
list2 = [one for one in list1]
    
```
**代码量明显降低了很多，当然也可以在里面加入一些条件处理语句**

```python

#标准的循环模式
list1 = [1,3,2,6,4]
list2 = []
for one in list1:
    if one > 2:
        list2.append(one)
    
#列表推导式
list1 = [1,3,2,6,4]
list2 = [one for one in list1 if one > 2]
    
```
**从代码量上，很明显列表推导式简洁了很多，只要用习惯了列表推导式就没必要再去使用python标准循环**

