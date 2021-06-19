---
title: 【python】python学习笔记（未整理）
tags:
  - python
abbrlink: 52eba3ce
date: 2021-06-15 16:04:42
---



该文章是根据慕课网《高级python进阶》写的，算是以前的学习笔记，现在该课程已下线。

这里我推荐廖雪峰的教程，https://www.liaoxuefeng.com/wiki/1016959663602400/

或者看官方文档，但是想快速入门不是很推荐官方文档：https://docs.python.org/zh-cn/3/tutorial/index.html

```
函数式编程：贴近计算（底层语言贴近计算机）

函数 != 函数式

函数式编程的特点：
把计算视为函数而非指令
纯函数式编程：不需要变量，没有副作用，测试简单
支持高阶函数，代码简洁

python支持的函数式编程特点：
不是纯函数式编程：允许有变量
支持高阶函数：函数也可以作为变量传入
支持闭包：有了闭包就能返回函数
有限度地支持匿名函数


高阶函数：能接收函数做参数的函数
	变量可以指向函数，函数的参数可以接收变量，所以一个函数可以接收另外一个函数作为参数

>>> def add(x,y,f):
...     return f(x)+f(y)
...
>>> add(-5,9,abs)
14


python中map()函数
map()是 Python 内置的高阶函数，它接收一个函数 f 和一个 list，并通过把函数 f 依次作用在 list 的每个元素上，得到一个新的 list 并返回。

def f(x):
    return x*x
print map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])

def format_name(s):
    return s.lower().title()

print map(format_name, ['adam', 'LISA', 'barT'])

注意：
map()函数不改变原有的 list，而是返回一个新的 list。
由于list包含的元素可以是任何类型，因此，map() 不仅仅可以处理只包含数值的 list，事实上它可以处理包含任意类型的 list，只要传入的函数f可以处理这种数据类型。

python中reduce()函数
reduce()函数也是Python内置的一个高阶函数。reduce()函数接收的参数和 map()类似，一个函数 f，一个list，但行为和 map()不同，reduce()传入的函数 f 必须接收两个参数，reduce()对list的每个元素反复调用函数f，并返回最终结果值。
reduce()还可以接收第3个可选参数，作为计算的初始值。

# 求和函数reduce实现
def f(x, y):
    return x + y

print reduce(f, [1, 3, 5], 100)
# 109

# Python内置了求和函数sum()，但没有求积的函数，请利用recude()来求积：

def prod(x, y):
    return x*y

print reduce(prod, [2, 4, 5, 7, 12])



python中filter()函数
filter()函数是 Python 内置的另一个有用的高阶函数，filter()函数接收一个函数 f 和一个list，这个函数 f 的作用是对每个元素进行判断，返回 True或 False，filter()根据判断结果自动过滤掉不符合条件的元素，返回由符合条件元素组成的新list。

请利用filter()过滤出1~100中平方根是整数的数：
import math

def is_sqr(x):
    r = math.sqrt(x)
    return r % 1 == 0

print filter(is_sqr, range(1, 101))


利用filter()，可以完成很多有用的功能，例如，删除 None 或者空字符串：
def is_not_empty(s):
    return s and len(s.strip()) > 0
filter(is_not_empty, ['test', None, '', 'str', '  ', 'END'])

注意: s.strip(rm) 删除 s 字符串中开头、结尾处的 rm 序列的字符。

当rm为空时，默认删除空白符（包括'\n', '\r', '\t', ' ')


python中自定义排序函数

Python内置的 sorted()函数可对list进行排序：
>>>sorted([36, 5, 12, 9, 21])

[5, 9, 12, 21, 36]

但 sorted()也是一个高阶函数，它可以接收一个比较函数来实现自定义排序，比较函数的定义是，传入两个待比较的元素 x, y，如果 x 应该排在 y 的前面，返回 -1，如果 x 应该排在 y 的后面，返回 1。如果 x 和 y 相等，返回 0。
因此，如果我们要实现倒序排序，只需要编写一个reversed_cmp函数：
def reversed_cmp(x, y):
    if x > y:
        return -1
    if x < y:
        return 1
    return 0
>>> sorted([36, 5, 12, 9, 21], reversed_cmp)
[36, 21, 12, 9, 5]

sorted()也可以对字符串进行排序，字符串默认按照ASCII大小来比较：


python中返回函数

Python的函数不但可以返回int、str、list、dict等数据类型，还可以返回函数！

例如，定义一个函数 f()，我们让它返回一个函数 g，可以这样写：
def f():
    print 'call f()...'
    # 定义函数g:
    def g():
        print 'call g()...'
    # 返回函数g:
    return g

>>> x = f()   # 调用f()
call f()...
>>> x   # 变量x是f()返回的函数：
<function g at 0x1037bf320>
>>> x()   # x指向函数，因此可以调用
call g()...   # 调用x()就是执行g()函数定义的代码

请注意区分返回函数和返回值：
def myabs():
    return abs   # 返回函数
def myabs2(x):
    return abs(x)   # 返回函数调用的结果，返回值是一个数值

返回函数可以把一些计算延迟执行。例如
def calc_sum(lst):
    def lazy_sum():
        return sum(lst)
    return lazy_sum

>>> f = calc_sum([1, 2, 3, 4])
>>> f
<function lazy_sum at 0x1037bfaa0>
>>> f()
10

python中闭包

在函数内部定义的函数和外部定义的函数是一样的，只是他们无法被外部访问：
def calc_sum(lst):
    def lazy_sum():
        return sum(lst)
    return lazy_sum

像这种内层函数引用了外层函数的变量（参数也算变量），然后返回内层函数的情况，称为闭包（Closure）。

闭包的特点是返回的函数还引用了外层函数的局部变量，所以，要正确使用闭包，就要确保引用的局部变量在函数返回后不能变。举例如下：

# 希望一次返回3个函数，分别计算1x1,2x2,3x3:
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()

你可能认为调用f1()，f2()和f3()结果应该是1，4，9，但实际结果全部都是 9（请自己动手验证）。

原因就是当count()函数返回了3个函数时，这3个函数所引用的变量 i 的值已经变成了3。由于f1、f2、f3并没有被调用，所以，此时他们并未计算 i*i，当 f1 被调用时：

>>> f1()
9     # 因为f1现在才计算i*i，但现在i的值已经变为3
因此，返回函数不要引用任何循环变量，或者后续会发生变化的变量。


python中匿名函数

高阶函数可以接收函数做参数，有些时候，我们不需要显式地定义函数，直接传入匿名函数更方便

>>> map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9])
[1, 4, 9, 16, 25, 36, 49, 64, 81]

通过对比可以看出，匿名函数 lambda x: x * x 实际上就是：

def f(x):
    return x * x

关键字lambda 表示匿名函数，冒号前面的 x 表示函数参数。
匿名函数有个限制，就是只能有一个表达式，不写return，返回值就是该表达式的结果。
使用匿名函数，可以不必定义函数名，直接创建一个函数对象，很多时候可以简化代码：


python中decorator装饰器

问题：
定义了一个函数
想在运行时动态增加功能
又不想改动函数本身的代码

方法一：直接修改原函数（废话）
方法二：通过高阶函数返回新函数

def f1(x):
	return x*2
def new_f(f):   # 装饰器函数
	def fn(x):
		print 'call'+ f._name_+'()'
		return f(x)
	return fn

调用装饰器函数：
方法一：
g1 = new_f(f1)
print g1(5)
# 10

f1 = new_fn(f1)
print f1(5)
# call f1()
# 10

python内置的@语法就是为了简化装饰器的调用

@new_fun 
def f1(x):
	return x*2

相当于：
def f1(x):
	return x*2
f1 = new_fn(f1)


装饰器的作用
可以极大地简化代码，避免每个函数编写重复性代码
打印日志： @log
检测性能： @performance
数据库事物：@transaction
URl路由：@post('/register')



python中编写无参数decorator

Python的 decorator 本质上就是一个高阶函数，它接收一个函数作为参数，然后，返回一个新函数。
def log(f):
    def fn(*args, **kw):
        print 'call ' + f.__name__ + '()...'
        return f(*args, **kw)
    return fn

@log
def add(x, y):
    return x + y
print add(1, 2)


python中编写带参数decorator

def log(prefix):
    def log_decorator(f):
        def wrapper(*args, **kw):
            print '[%s] %s()...' % (prefix, f.__name__)
            return f(*args, **kw)
        return wrapper
    return log_decorator

@log('DEBUG')
def test():
    pass
print test()



python中完善decorator

@decorator可以动态实现函数功能的增加，但是，经过@decorator“改造”后的函数，和原函数相比，除了功能多一点外，有没有其它不同的地方？
def log(f):
    def wrapper(*args, **kw):
        print 'call...'
        return f(*args, **kw)
    return wrapper
@log
def f2(x):
    pass
print f2.__name__
# wrapper

由于decorator返回的新函数函数名已经不是'f2'，而是@log内部定义的'wrapper'。这对于那些依赖函数名的代码就会失效。decorator还改变了函数的__doc__等其它属性。如果要让调用者看不出一个函数经过了@decorator的“改造”，就需要把原函数的一些属性复制到新函数中：

Python内置的functools可以用来自动化完成复制decorator改变的属性的任务：

import functools
def log(f):
    @functools.wraps(f)
    def wrapper(*args, **kw):
        print 'call...'
        return f(*args, **kw)
    return wrapper

最后需要指出，由于我们把原函数签名改成了(*args, **kw)，因此，无法获得原函数的原始参数信息。即便我们采用固定参数来装饰只有一个参数的函数：
def log(f):
    @functools.wraps(f)
    def wrapper(x):
        print 'call...'
        return f(x)
    return wrapper

也可能改变原函数的参数名，因为新函数的参数名始终是 'x'，原函数定义的参数名不一定叫 'x'。


python中偏函数

当一个函数有很多参数时，调用者就需要提供多个参数。如果减少参数个数，就可以简化调用者的负担。
比如，int()函数可以把字符串转换为整数，当仅传入字符串时，int()函数默认按十进制转换。但int()函数还提供额外的base参数，默认值为10。如果传入base参数，就可以做 N 进制的转换：
假设要转换大量的二进制字符串，每次都传入int(x, base=2)非常麻烦，于是，我们想到，可以定义一个int2()的函数，默认把base=2传进去：
def int2(x, base=2):
    return int(x, base)
这样，我们转换二进制就非常方便了：
>>> int2('1000000')
64
>>> int2('1010101')
85

functools.partial就是帮助我们创建一个偏函数的，不需要我们自己定义int2()，可以直接使用下面的代码创建一个新的函数int2：
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85

所以，functools.partial可以把一个参数多的函数变成一个参数少的新函数，少的参数需要在创建时指定默认值，这样，新函数调用的难度就降低了。


python中模块和包的概念

将代码分拆放入多个py文件的好处是：同一个名字的变量互不影响（函数名也是变量）

在文件系统中：
包就是文件夹
文件名就是模块名  test.py的模块名是test
包可以有多级


模块名冲突解决：放入不同的包中

引用完整模块
#test.py     自身模块名test
import p1.util     引用p1.util模块
print p1.util.f(2,10)    调用p1.util模块的f函数



如何区分包和普通目录
包下面有个__init__.py
包的每一层都必须有这个文件__init__.py，即使是空文件

python之导入模块

要使用一个模块，我们必须首先导入该模块。Python使用import语句导入一个模块。
你可以认为math就是一个指向已导入模块的变量，通过该变量，我们可以访问math模块中所定义的所有公开的函数、变量和类：
import math
>>> math.pow(2, 0.5) # pow是函数
1.4142135623730951

>>> math.pi # pi是变量
3.141592653589793

如果我们只希望导入用到的math模块的某几个函数，而不是所有函数，可以用下面的语句：
from math import pow, sin, log
这样，可以直接引用 pow, sin, log 这3个函数，但math的其他函数没有导入进来：
>>> pow(2, 10)
1024.0
>>> sin(3.14)
0.0015926529164868282


如果遇到名字冲突怎么办？比如math模块有一个log函数，logging模块也有一个log函数，如果同时使用，如何解决名字冲突？
如果使用import导入模块名，由于必须通过模块名引用函数名，因此不存在冲突：
import math, logging
print math.log(10)   # 调用的是math的log函数
logging.log(10, 'something')   # 调用的是logging的log函数

如果使用 from...import 导入 log 函数，势必引起冲突。这时，可以给函数起个“别名”来避免冲突：
from math import log
from logging import log as logger   # logging的log现在变成了logger
print log(10)   # 调用的是math的log
logger(10, 'import from logging')   # 调用的是logging的log


python中动态导入模块
如果导入的模块不存在，Python解释器会报 ImportError 错误：
>>> import something
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named something

利用ImportError错误，我们经常在Python中动态导入模块：
Python 2.6/2.7提供了json 模块，但Python 2.5以及更早版本没有json模块，不过可以安装一个simplejson模块，这两个模块提供的函数签名和功能都一模一样。

try:
    import json
except ImportError:
    import simplejson as json

print json.dumps({'python':2.7})
上述代码先尝试从cStringIO导入，如果失败了（比如cStringIO没有被安装），再尝试从StringIO导入。

try 的作用是捕获错误，并在捕获到指定错误时执行 except 语句。


python之使用__future__

当新版本的一个特性与旧版本不兼容时，该特性将会在旧版本中添加到__future__中，以便旧的代码能在旧版本中测试新特性。
要“试用”某一新的特性，就可以通过导入__future__模块的某些功能来实现。

Python 2.7的整数除法运算结果仍是整数：
但是，Python 3.x已经改进了整数的除法运算，“/”除将得到浮点数，“//”除才仍是整数：
要在Python 2.7中引入3.x的除法规则，导入__future__的division：
>>> from __future__ import division
>>> print 10 / 3
3.3333333333333335


任务
在Python 3.x中，字符串统一为unicode，不需要加前缀 u，而以字节存储的str则必须加前缀 b。请利用__future__的unicode_literals在Python 2.7中编写unicode字符串。
from __future__  import unicode_literals

s = 'am I an unicode?'
print isinstance(s, unicode)

# isinstance（object，type） 来判断一个对象是否是一个已知的类型。 
其第一个参数（object）为对象，第二个参数（type）为类型名(int...)或类型名的一个列表((int,list,float)是一个列表)。其返回值为布尔型（True or flase）。
若对象的类型与参数二的类型相同则返回True。若参数二为一个元组，则若对象类型与元组中类型名之一相同即返回True。


python 提供的模块管理工具
easy_install
pip(推荐，已经内置到python2.7.9，其他需要在安装python时选择安装)

查找python包的网站： https://pypi.org/


python面向对象编程
面向对象编程是一种程序设计范式
把程序看做不同对象的相互调用
对现实世界建立对象模型


面向对象编程的基本思想
类和实例
	类用于定义抽象类型
	实例根据类的定义被创建出来


类： class Person:
	pass
实例：xiaoming = Person()
         xiaojun = Person()

面向对象编程的重要思想：数据封装
class Person:
	def __init__(self, name):
		self.name = name


p1 = Person('xiaoming')
p2 = Person('xiaojun')


python之定义类并创建实例
在Python中，类通过 class 关键字定义。
按照 Python 的编程习惯，类名以大写字母开头，紧接着是(object)，表示该类是从哪个类继承下来的。
创建实例使用 类名+()，类似函数调用的形式创建


python中创建实例属性
如何让每个实例拥有各自不同的属性？由于Python是动态语言，对每一个实例，都可以直接给他们的属性赋值，例如，给xiaoming这个实例加上name、gender和birth属性：
xiaoming = Person()
xiaoming.name = 'Xiao Ming'
xiaoming.gender = 'Male'
xiaoming.birth = '1990-1-1'

给xiaohong加上的属性不一定要和xiaoming相同：
xiaohong = Person()
xiaohong.name = 'Xiao Hong'
xiaohong.school = 'No. 1 High School'
xiaohong.grade = 2

实例的属性可以像普通变量一样进行操作：
xiaohong.grade = xiaohong.grade + 1

任务
请创建包含两个 Person 类的实例的 list，并给两个实例的 name 赋值，然后按照 name 进行排序。
class Person(object):
    pass

p1 = Person()
p1.name = 'Bart'

p2 = Person()
p2.name = 'Adam'

p3 = Person()
p3.name = 'Lisa'

L1 = [p1, p2, p3]
L2 = sorted(L1, lambda x,y:cmp(x.name,y.name))

print L2[0].name
print L2[1].name
print L2[2].name


python中初始化实例属性
在定义 Person 类时，可以为Person类添加一个特殊的__init__()方法，当创建实例时，__init__()方法被自动调用，我们就能在此为每个实例都统一加上以下属性：
class Person(object):
    def __init__(self, name, gender, birth):
        self.name = name
        self.gender = gender
        self.birth = birth

__init__() 方法的第一个参数必须是 self（也可以用别的名字，但建议使用习惯用法），后续参数则可以自由指定，和定义函数没有任何区别。

相应地，创建实例时，就必须要提供除 self 以外的参数：
xiaoming = Person('Xiao Ming', 'Male', '1991-1-1')
xiaohong = Person('Xiao Hong', 'Female', '1992-2-2')

有了__init__()方法，每个Person实例在创建时，都会有 name、gender 和 birth 这3个属性，并且，被赋予不同的属性值，访问属性使用.操作符：
print xiaoming.name
# 输出 'Xiao Ming'
print xiaohong.birth
# 输出 '1992-2-2'

这里的self相当于this指向当前实例


python中访问限制
我们可以给一个实例绑定很多属性，如果有些属性不希望被外部访问到怎么办？
Python对属性权限的控制是通过属性名来实现的，如果一个属性由双下划线开头(__)，该属性就无法被外部访问。
class Person(object):
    def __init__(self, name):
        self.name = name
        self._title = 'Mr'
        self.__job = 'Student'
p = Person('Bob')
print p.name
# => Bob
print p._title
# => Mr
print p.__job
# => Error
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Person' object has no attribute '__job'
可见，只有以双下划线开头的"__job"不能直接被外部访问。
但是，如果一个属性以"__xxx__"的形式定义，那它又可以被外部访问了，以"__xxx__"定义的属性在Python的类中被称为特殊属性，有很多预定义的特殊属性可以使用，通常我们不要把普通属性用"__xxx__"定义。
以单下划线开头的属性"_xxx"虽然也可以被外部访问，但是，按照习惯，他们不应该被外部访问。

class Person(object):
    def __init__(self, name, score):
        self.name = name
        self.__score = score

p = Person('Bob', 59)


print p.name

try:
    print p.__score
except AttributeError:
    print 'AttributeError'


 
python中类属性和实例属性名字冲突怎么办
修改类属性会导致所有实例访问到的类属性全部都受影响，但是，如果在实例变量上修改类属性会发生什么问题呢？
class Person(object):
    address = 'Earth'
    def __init__(self, name):
        self.name = name

p1 = Person('Bob')
p2 = Person('Alice')

print 'Person.address = ' + Person.address

p1.address = 'China'
print 'p1.address = ' + p1.address

print 'Person.address = ' + Person.address
print 'p2.address = ' + p2.address

结果如下：
Person.address = Earth
p1.address = China
Person.address = Earth
p2.address = Earth

我们发现，在设置了 p1.address = 'China' 后，p1访问 address 确实变成了 'China'，但是，Person.address和p2.address仍然是'Earch'，怎么回事？

原因是 p1.address = 'China'并没有改变 Person 的 address，而是给 p1这个实例绑定了实例属性address ，对p1来说，它有一个实例属性address（值是'China'），而它所属的类Person也有一个类属性address，所以:

访问 p1.address 时，优先查找实例属性，返回'China'。

访问 p2.address 时，p2没有实例属性address，但是有类属性address，因此返回'Earth'。

可见，当实例属性和类属性重名时，实例属性优先级高，它将屏蔽掉对类属性的访问。

当我们把 p1 的 address 实例属性删除后，访问 p1.address 就又返回类属性的值 'Earth'了：

del p1.address
print p1.address
# => Earth
可见，千万不







    F:\game\尼尔机械纪元中文版\nierjixiejiyuan\3DMGAME-NieR_Automata.(UP1).v6.5.CHS.Green.part16.rar: 3DMGAME-NieR_Automata.(UP1).v6.5.CHS.Green\NieR Automata CPY\data\movie\ev0483.usm 
校验和错误。文件被破坏
    F:\game\尼尔机械纪元中文版\nierjixiejiyuan\3DMGAME-NieR_Automata.(UP1).v6.5.CHS.Green.part30.rar: 必需的压缩卷不存在
    F:\game\尼尔机械纪元中文版\nierjixiejiyuan\3DMGAME-NieR_Automata.(UP1).v6.5.CHS.Green.part29.rar: 3DMGAME-NieR_Automata.(UP1).v6.5.CHS.Green\NieR Automata CPY\data\movie\ev1150.usm 校验和错误。文件被破坏


要在实例上修改类属性，它实际上并没有修改类属性，而是给实例绑定了一个实例属性。

python中方法也是属性
我们在 class 中定义的实例方法其实也是属性，它实际上是一个函数对象：
也就是说，p1.get_grade 返回的是一个函数对象，但这个函数是一个绑定到实例的函数，p1.get_grade() 才是方法调用。
因为方法也是一个属性，所以，它也可以动态地添加到实例上，只是需要用 types.MethodType() 把一个函数变为一个方法：
import types
def fn_get_grade(self):
    if self.score >= 80:
        return 'A'
    if self.score >= 60:
        return 'B'
    return 'C'

class Person(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score

p1 = Person('Bob', 90)
p1.get_grade = types.MethodType(fn_get_grade, p1, Person)
print p1.get_grade()
# => A
p2 = Person('Alice', 65)
print p2.get_grade()
# ERROR: AttributeError: 'Person' object has no attribute 'get_grade'
# 因为p2实例并没有绑定get_grade

给一个实例动态添加方法并不常见，直接在class中定义要更直观

函数调用不需要传入 self，但是方法调用需要传入self?


python中定义类方法
和属性类似，方法也分实例方法和类方法。
在class中定义的全部是实例方法，实例方法第一个参数 self 是实例本身。
要在class中定义类方法，需要这么写：

class Person(object):
    count = 0
    @classmethod
    def how_many(cls):
        return cls.count
    def __init__(self, name):
        self.name = name
        Person.count = Person.count + 1

print Person.how_many()
p1 = Person('Bob')
print Person.how_many()

通过标记一个 @classmethod，该方法将绑定到 Person 类上，而非类的实例。类方法的第一个参数将传入类本身，通常将参数名命名为 cls，上面的 cls.count 实际上相当于 Person.count。

因为是在类上调用，而非实例上调用，因此类方法无法获得任何实例变量，只能获得类的引用。

什么是继承？

 新类不必从头编写
新类从现有的类继承，就自动拥有了现有类的所有功能
新类只需要编写现有类缺少的新功能


继承的好处
复用代码

父类（又称基类，超类）
子类（又称派生类，继承类）

继承的特点
总是从某个类继承，如果没有合适的类则从object继承

class MyClass(object):
	pass

不要忘记调用super().__init__ 正确初始化父类
def __init__(self, args):
	super(SubClass, self).__init__(args)
	pass

python中继承一个类

class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender

class Student(Person):
    def __init__(self, name, gender, score):
        super(Student, self).__init__(name, gender)
        self.score = score

class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender

class Teacher(Person):

    def __init__(self, name, gender, course):
        super(Teacher,self).__init__(name,gender)
        self.course = course

t = Teacher('Alice', 'Female', 'English')
print t.name
print t.course


python中判断类型
函数isinstance()可以判断一个变量的类型，既可以用在Python内置的数据类型如str、list、dict，也可以用在我们自定义的类，它们本质上都是数据类型。


python中多态

class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
    def whoAmI(self):
        return 'I am a Person, my name is %s' % self.name

class Student(Person):
    def __init__(self, name, gender, score):
        super(Student, self).__init__(name, gender)
        self.score = score
    def whoAmI(self):
        return 'I am a Student, my name is %s' % self.name

class Teacher(Person):
    def __init__(self, name, gender, course):
        super(Teacher, self).__init__(name, gender)
        self.course = course
    def whoAmI(self):
        return 'I am a Teacher, my name is %s' % self.name
在一个函数中，如果我们接收一个变量 x，则无论该 x 是 Person、Student还是 Teacher，都可以正确打印出结果：

def who_am_i(x):
    print x.whoAmI()

p = Person('Tim', 'Male')
s = Student('Bob', 'Male', 88)
t = Teacher('Alice', 'Female', 'English')

who_am_i(p)
who_am_i(s)
who_am_i(t)
I am a Person, my name is Tim
I am a Student, my name is Bob
I am a Teacher, my name is Alice

这种行为称为多态。也就是说，方法调用将作用在 x 的实际类型上。s 是Student类型，它实际上拥有自己的 whoAmI()方法以及从 Person继承的 whoAmI方法，但调用 s.whoAmI()总是先查找它自身的定义，如果没有定义，则顺着继承链向上查找，直到在某个父类中找到为止。

由于Python是动态语言，所以，传递给函数 who_am_i(x)的参数 x 不一定是 Person 或 Person 的子类型。任何数据类型的实例都可以，只要它有一个whoAmI()的方法即可：

class Book(object):
    def whoAmI(self):
        return 'I am a book'
这是动态语言和静态语言（例如Java）最大的差别之一。动态语言调用实例方法，不检查类型，只要方法存在，参数正确，就可以调用。

python中多重继承
除了从一个父类继承外，Python允许从多个父类继承，称为多重继承。

多重继承的继承链就不是一棵树了，它像这样：

class A(object):
    def __init__(self, a):
        print 'init A...'
        self.a = a

class B(A):
    def __init__(self, a):
        super(B, self).__init__(a)
        print 'init B...'

class C(A):
    def __init__(self, a):
        super(C, self).__init__(a)
        print 'init C...'

class D(B, C):
    def __init__(self, a):
        super(D, self).__init__(a)
        print 'init D...'
像这样，D 同时继承自 B 和 C，也就是 D 拥有了 A、B、C 的全部功能。多重继承通过 super()调用__init__()方法时，A 虽然被继承了两次，但__init__()只调用一次：
>>> d = D('d')
init A...
init C...
init B...
init D...

多重继承的目的是从两种继承树中分别选择并继承出子类，以便组合功能使用。

举个例子，Python的网络服务器有TCPServer、UDPServer、UnixStreamServer、UnixDatagramServer，而服务器运行模式有 多进程ForkingMixin 和 多线程ThreadingMixin两种。

要创建多进程模式的 TCPServer：

class MyTCPServer(TCPServer, ForkingMixin)
    pass
要创建多线程模式的 UDPServer：

class MyUDPServer(UDPServer, ThreadingMixin):
    pass


python中获取对象信息
拿到一个变量，除了用 isinstance() 判断它是否是某种类型的实例外，还有没有别的方法获取到更多的信息呢？

对于Student类有：
class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender

class Student(Person):
    def __init__(self, name, gender, score):
        super(Student, self).__init__(name, gender)
        self.score = score
    def whoAmI(self):
        return 'I am a Student, my name is %s' % self.name

首先可以用 type() 函数获取变量的类型，它返回一个 Type 对象：

>>> type(123)
<type 'int'>
>>> s = Student('Bob', 'Male', 88)
>>> type(s)
<class '__main__.Student'>

其次，可以用 dir() 函数获取变量的所有属性：

>>> dir(123)   # 整数也有很多属性...
['__abs__', '__add__', '__and__', '__class__', '__cmp__', ...]

>>> dir(s)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'gender', 'name', 'score', 'whoAmI']


对于实例变量，dir()返回所有实例属性，包括`__class__`这类有特殊意义的属性。注意到方法`whoAmI`也是 s 的一个属性。
dir()返回的属性是字符串列表，如果已知一个属性名称，要获取或者设置对象的属性，就需要用 getattr() 和 setattr( )函数了：
>>> getattr(s, 'name')  # 获取name属性
'Bob'

>>> setattr(s, 'name', 'Adam')  # 设置新的name属性

>>> s.name
'Adam'

>>> getattr(s, 'age')  # 获取age属性，但是属性不存在，报错：
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'age'

>>> getattr(s, 'age', 20)  # 获取age属性，如果属性不存在，就返回默认值20：
20

python中什么是特殊方法（魔术方法）

python如何把任意变量变成str

因为任何数据类型的实例都有一个特殊方法 __str__()

如果给Person类加上__str__()这个特殊方法，我们就可以按照自己的方法打实例。

>>> p = Person('Bob','male')
>>> print p
<Person: Bob, male>

python中的特殊方法：
用于打印的 __str__
用于长度的 __len__
用于比较的 __cmp__
……

特点

定义在class中
不需要直接调用
python的某些函数或操作符会调用对应的特殊方法，如print调用 __str__()


正确实现特殊方法
只需要编写 用到的特殊方法
有关联性的特殊方法都必须一起实现

如用到 __getattr__ ，那么就要一起实现 __setattr__ 和 __delattr__



python中 __str__和__repr__
如果要把一个类的实例变成 str，就需要实现特殊方法__str__()：
>>> class Person(object):
...    def __init__(self, name, gender):
......        self.name = name
......        self.gender = gender
...    def __str__(self):
......        return '(Person: %s, %s)' % (self.name, self.gender)

>>> p = Person('Bob', 'male')
>>> print p
 (Person: Bob, male)

# 但是，如果直接敲变量 p：
>>> p
<main.Person object at 0x10c941890>

似乎__str__() 不会被调用。

因为 Python 定义了__str__()和__repr__()两种方法，__str__()用于显示给用户，而__repr__()用于显示给开发人员。

有一个偷懒的定义__repr__的方法：
class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender
    def __str__(self):
        return '(Person: %s, %s)' % (self.name, self.gender)
    __repr__ = __str__


python中 __cmp__
对 int、str 等内置数据类型排序时，Python的 sorted() 按照默认的比较函数 cmp 排序，但是，如果对一组 Student 类的实例排序时，就必须提供我们自己的特殊方法 __cmp__()：

请修改 Student 的 __cmp__ 方法，让它按照分数从高到底排序，分数相同的按名字排序。

class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def __str__(self):
        return '(%s: %s)' % (self.name, self.score)

    __repr__ = __str__

    def __cmp__(self, s):
        if self.score == s.score:
            return cmp(self.name, s.name)
        return -cmp(self.score, s.score)

L = [Student('Tim', 99), Student('Bob', 88), Student('Alice', 99)]
print sorted(L)


python中 __len__
如果一个类表现得像一个list，要获取有多少个元素，就得用 len() 函数。

要让 len() 函数工作正常，类必须提供一个特殊方法__len__()，它返回元素的个数。

例如，我们写一个 Students 类，把名字传进去：
class Students(object):
    def __init__(self, *args):
        self.names = args
    def __len__(self):
        return len(self.names)
只要正确实现了__len__()方法，就可以用len()函数返回Students实例的“长度”：

>>> ss = Students('Bob', 'Alice', 'Tim')
>>> print len(ss)
3

python中数学运算
Python 提供的基本数据类型 int、float 可以做整数和浮点的四则运算以及乘方等运算。

但是，四则运算不局限于int和float，还可以是有理数、矩阵等。

要表示有理数，可以用一个Rational类来表示：

class Rational(object):
    def __init__(self, p, q):
        self.p = p
        self.q = q
p、q 都是整数，表示有理数 p/q。

如果要让Rational进行+运算，需要正确实现__add__：

class Rational(object):
    def __init__(self, p, q):
        self.p = p
        self.q = q
    def __add__(self, r):
        return Rational(self.p * r.q + self.q * r.p, self.q * r.q)
    def __str__(self):
        return '%s/%s' % (self.p, self.q)
    __repr__ = __str__
现在可以试试有理数加法：

>>> r1 = Rational(1, 3)
>>> r2 = Rational(1, 2)
>>> print r1 + r2
5/6

python中类型转换
Rational类实现了有理数运算，但是，如果要把结果转为 int 或 float 怎么办？
如果要把 Rational 转为 int，应该使用：

r = Rational(12, 5)
n = int(r)


要让int()函数正常工作，只需要实现特殊方法__int__():

class Rational(object):
    def __init__(self, p, q):
        self.p = p
        self.q = q
    def __int__(self):
        return self.p // self.q
结果如下：

>>> print int(Rational(7, 2))
3
>>> print int(Rational(1, 3))
0

同理，要让float()函数正常工作，只需要实现特殊方法__float__()。
class Rational(object):
    def __init__(self, p, q):
        self.p = p
        self.q = q

    def __int__(self):
        return self.p // self.q

    def __float__(self):
        return float(self.p) / self.q

print float(Rational(7, 2))
print float(Rational(1, 3))


python中 @property
考察 Student 类：

class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
当我们想要修改一个 Student 的 scroe 属性时，可以这么写：

s = Student('Bob', 59)
s.score = 60
但是也可以这么写：

s.score = 1000
显然，直接给属性赋值无法检查分数的有效性。

如果利用两个方法：

class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.__score = score
    def get_score(self):
        return self.__score
    def set_score(self, score):
        if score < 0 or score > 100:
            raise ValueError('invalid score')
        self.__score = score
这样一来，s.set_score(1000) 就会报错。

这种使用 get/set 方法来封装对一个属性的访问在许多面向对象编程的语言中都很常见。

但是写 s.get_score() 和 s.set_score() 没有直接写 s.score 来得直接。

有没有两全其美的方法？----有。

因为Python支持高阶函数，在函数式编程中我们介绍了装饰器函数，可以用装饰器函数把 get/set 方法“装饰”成属性调用：

class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.__score = score
    @property
    def score(self):
        return self.__score
    @score.setter
    def score(self, score):
        if score < 0 or score > 100:
            raise ValueError('invalid score')
        self.__score = score
注意: 第一个score(self)是get方法，用@property装饰，第二个score(self, score)是set方法，用@score.setter装饰，@score.setter是前一个@property装饰后的副产品。

现在，就可以像使用属性一样设置score了：

>>> s = Student('Bob', 59)
>>> s.score = 60
>>> print s.score
60
>>> s.score = 1000
Traceback (most recent call last):
  ...
ValueError: invalid score
说明对 score 赋值实际调用的是 set方法。

python中 __slots__
由于Python是动态语言，任何实例在运行期都可以动态地添加属性。

如果要限制添加的属性，例如，Student类只允许添加 name、gender和score 这3个属性，就可以利用Python的一个特殊的__slots__来实现。

顾名思义，__slots__是指一个类允许的属性列表：

class Student(object):
    __slots__ = ('name', 'gender', 'score')
    def __init__(self, name, gender, score):
        self.name = name
        self.gender = gender
        self.score = score
现在，对实例进行操作：

>>> s = Student('Bob', 'male', 59)
>>> s.name = 'Tim' # OK
>>> s.score = 99 # OK
>>> s.grade = 'A'
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'grade'
__slots__的目的是限制当前类所能拥有的属性，如果不需要添加任意动态的属性，使用__slots__也能节省内存。


python中 __call__
在Python中，函数其实是一个对象：

>>> f = abs
>>> f.__name__
'abs'
>>> f(-123)
123
由于 f 可以被调用，所以，f 被称为可调用对象。

所有的函数都是可调用对象。

一个类实例也可以变成一个可调用对象，只需要实现一个特殊方法__call__()。

我们把 Person 类变成一个可调用对象：

class Person(object):
    def __init__(self, name, gender):
        self.name = name
        self.gender = gender

    def __call__(self, friend):
        print 'My name is %s...' % self.name
        print 'My friend is %s...' % friend
现在可以对 Person 实例直接调用：

>>> p = Person('Bob', 'male')
>>> p('Tim')
My name is Bob...
My friend is Tim...
单看 p('Tim') 你无法确定 p 是一个函数还是一个类实例，所以，在Python中，函数也是对象，对象和函数的区别并不显著。
```

