# python 语法复习
## *args 和 **kwargs :
 *args 是一个元组，**kwargs是一个字典，他们共同用于实现可变参数。*args必须位于**kwargs之前。
例子：

    def foo(*args, **kwargs):
        print 'args = ', args
        print 'kwargs = ', kwargs
        print '---------------------------------------'

    if __name__ == '__main__':
        foo(1,2,3,4)
        foo(a=1,b=2,c=3)
        foo(1,2,3,4, a=1,b=2,c=3)
        foo('a', 1, None, a=1, b='2', c=3)
    
    results:
    Required argument:  1
    Optional argument (*args):  2
    Optional argument (*args):  3
    Optional argument (*args):  4
    Optional argument k2 (*kwargs): 6
    Optional argument k1 (*kwargs): 5

`*args和**kwargs`不仅可以在函数定义中使用，同样可以在函数调用的时候使用。不同的是，如果说在函数定义的位置使用`*args和**kwargs`是一个将参数pack的过程，那么在函数调用的时候就是一个将参数unpack的过程了。

    def test_args(first, second, third, fourth, fifth):
        print 'First argument: ', first
        print 'Second argument: ', second
        print 'Third argument: ', third
        print 'Fourth argument: ', fourth
        print 'Fifth argument: ', fifth

    # Use *args
    args = [1, 2, 3, 4, 5]
    test_args(*args)
    # results:
    # First argument:  1
    # Second argument:  2
    # Third argument:  3
    # Fourth argument:  4
    # Fifth argument:  5

    # Use **kwargs
    kwargs = {
        'first': 1,
        'second': 2,
        'third': 3,
        'fourth': 4,
        'fifth': 5
    }

    test_args(**kwargs)
    # results:
    # First argument:  1
    # Second argument:  2
    # Third argument:  3
    # Fourth argument:  4
    # Fifth argument:  5

参数解析的优先级从高到低依次是：
1. F(arg1,arg2,...)
2. F(arg1=v1,arg2=v2,...)
3. F(*arg1)
4. F(**arg1)

## filter、map、reduce、lambda
[ Python特殊语法：filter、map、reduce、lambda [转]](https://www.cnblogs.com/longdouhzt/archive/2012/05/19/2508844.html)<br>
* 前三项都是python中自带的高阶函数。操作函数的函数叫做高阶函数。高阶函数可用作强大的抽象机制，极大提升语言的表现力。
* filter 对sequence（可迭代对象）全元素进行筛选， 返回值由sequence类型决定
* map 对sequence（可迭代对象）中各个item执行function,执行结果组成list返回。map也支持多个sequence，这就要求function也支持相应数量的参数输入
* reduced 对sequence（可迭代对象）中各个item顺序迭代调用，还可以传入starting_value作为初始值。
* lambda: 可以快速定义匿名函数。
可以用于方便的实现闭包。

    f = **lambda** x: [[y for j, y in enumerate(set(x)) if (i >> j) & 1] for i in range(2**len(set(x)))]

    上面这一句可以用于生成所有子集

## python中的语法糖
* if...else 三元表达式： 可以简化分支判断语句，如 x = y.lower() if isinstance(y, str) else y
* with语句： 用于文件操作时，可以帮我们自动关闭文件对象，使代码变得简洁；
* 装饰器： 可以在不改变函数代码及函数调用方式的前提下，为函数增加增强性功能；
* 列表生成式： 用于生成一个新的列表
* 生成器： 用于“惰性”地生成一个无限序列

### with 工作原理
1. 紧跟with后面的语句被求值后，返回对象的“–enter–()”方法被调用，这个方法的返回值将被赋值给as后面的变量； 
2. 当with后面的代码块全部被执行完之后，将调用前面返回对象的“–exit–()”方法。 

exit()方法中有３个参数， exc_type, exc_val, exc_tb，这些参数在异常处理中相当有用。 
* exc_type：　错误的类型 
* exc_val：　错误类型对应的值 
* exc_tb：　代码中错误发生的位置 

### 列表生成式
1. 语法格式
* 基础语法格式： 
[exp for iter_var in iterable]
* 带过滤功能语法格式：
[exp for iter_var in iterable if_exp]
* 循环嵌套语法格式：
[exp for iter_var_A in iterable_A for iter_var_B in iterable_B]

### 生成器
1. 作用：
按照某种算法不断生成新的数据，直到满足某一个指定的条件结束。
2. 构造方式:
    * 使用类似列表生成式的方式生成 (2*n + 1 for n in range(3, 11))
    * 使用包含yield的函数来生成
3. 生成器的执行过程：<br>
在执行过程中，遇到yield关键字就会中断执行，下次调用则继续从上次中断的位置继续执行。
4. 生成器的特性：
    * 只有在调用时才会生成相应的数据
    * 只记录当前的位置
    * 只能next，不能prev
5. 生成器的调用方式:
    * 调用内置的next()方法
    * 使用循环对生成器对象进行遍历

### 装饰器
1. 粗鄙之语解释装饰器：<br>
每个人都有的内裤主要功能是用来遮羞，但是到了冬天它没法为我们防风御寒，咋办？我们想到的一个办法就是把内裤改造一下，让它变得更厚更长，这样一来，它不仅有遮羞功能，还能提供保暖，不过有个问题，这个内裤被我们改造成了长裤后，虽然还有遮羞功能，但本质上它不再是一条真正的内裤了。于是聪明的人们发明长裤，在不影响内裤的前提下，直接把长裤套在了内裤外面，这样内裤还是内裤，有了长裤后宝宝再也不冷了。装饰器就像我们这里说的长裤，在不影响内裤作用的前提下，给我们的身子提供了保暖的功效。
2. 举例：<br>
[理解 Python 装饰器看这一篇就够了](https://foofish.net/python-decorator.html)

        @a
        def f():
        等价于：
        def f():

        f = a(f)

3. 要点：        
    1. 装饰器可以含参数
    2. 装饰器可以是类，主要依靠类的``__call__``方法
    3. 缺点就是原函数的元信息会丢失，可以通过wraps装饰器来拷贝。
    4. 装饰器顺序由下到上，由里到外。

### 常用装饰器介绍

#### property 
负责把一个方法变成属性调用。
和另一个装饰器 @arg.setter成对使用。如果没有setter就是一个只读属性。
例子： [使用@property](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386820062641f3bcc60a4b164f8d91df476445697b9e000#0)

#### wraps
Python装饰器（decorator）在实现的时候，被装饰后的函数其实已经是另外一个函数了（函数名等函数属性会发生改变），为了不影响，Python的functools包中提供了一个叫wraps的decorator来消除这样的副作用。写一个decorator的时候，最好在实现之前加上functools的wraps，它能保留原有函数的名称和docstring。


## python 面向对象编程
### 类和实例
实例可以直接定义不在类模板中出现的属性，当做自身特有的属性。
### 访问限制
* 如果想让内部属性不被外部访问，可以把属性的名称前加上两个下划线__，在Python中，实例的变量名如果以双下划线开头，就变成了一个私有变量(private)，只有内部可以访问，外部不能访问。
* Python中如果变量名以双下划线开头和结尾的，是特殊变量__XXX__。特殊变量是可以直接从类内部访问的。
* 有些时候，你会看到以一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，但不能导入。
* 不能直接访问__name是因为Python解释器对外把__name变量改成了_Student__name，所以，仍然可以通过_Student__name来访问__name变量。
* Python的访问限制其实并不严格，主要靠自觉。
### Mixin
Mixin 表示混入，理解上应该理解为作为功能将Mixin添加到子类中，而不是作为子类的父类。
1. mixin类必须表示一组功能。
2. 必须责任单一
3. 不依赖于子类的实现
4. 子类没有mixin也可以工作，只是少了某些功能。
### python 类中特殊方法
#### __init__
初始类对象，相当于C++里面的构造函数。

#### __call__
把类对象作为一个函数调用时，执行的函数。

#### __get__,__getattr__,__getattribute__
参考：[python3中__get__,__getattr__,__getattribute__的区别](https://www.cnblogs.com/Vito2008/p/5280216.html)



### 引用
[Python之列表生成式、生成器、可迭代对象与迭代器](https://www.cnblogs.com/yyds/p/6281453.html)
<br>
[Python 函数的艺术：高阶函数](https://juejin.im/entry/5879a5281b69e6006be00563)

