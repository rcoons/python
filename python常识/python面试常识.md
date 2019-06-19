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

