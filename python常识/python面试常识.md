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

