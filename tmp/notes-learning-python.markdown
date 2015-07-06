##Learning Python
+ 在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件。
+ Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言。对于单个字符的编码，Python提供了``ord()``函数获取字符的整数表示，``chr()``函数把编码转换为对应的字符。
+ 由于Python的字符串类型是``str``，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把``str``变为以字节为单位的``bytes``。以Unicode表示的``str``通过``encode()``方法可以编码为指定的``bytes``。反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是``bytes``。要把``bytes``变为``str``，就需要用``decode()``方法。
+ ``len()``函数计算的是``str``的字符数，如果换成``bytes``，``len()``函数就计算字节数
+ 文件编码

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

+ ``List``,``append``,``insert``,``pop``
+ ``tuple``,tuple和list非常类似，但是tuple一旦初始化就不能修改,不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。tuple的陷阱：当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来。如果要定义一个空的tuple，可以写成``()``，但是，要定义一个只有1个元素的tuple，如果你这么定义：``t = (1)``定义的不是tuple，是1这个数！这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是1。所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义``t = (1,)``

+ if判断条件还可以简写，比如写：

```python
if x:
    print('True')
```
只要x是非零数值、非空字符串、非空list等，就判断为True，否则为False。

+ Python的循环有两种，一种是for...in循环，依次把list或tuple中的每个元素迭代出来，第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。
+ Python内置了字典：dict的支持，dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。给定一个名字，比如'Michael'，dict在内部就可以直接计算出Michael对应的存放成绩的“页码”，也就是95这个数字存放的内存地址，直接取出来，所以速度非常快。这种key-value存储方式，在放进去的时候，必须根据key算出value的存放位置，这样，取的时候才能根据key直接拿到value。由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉。要避免key不存在的错误，有两种办法，一是通过in判断key是否存在``'Thomas' in d``,二是通过dict提供的get方法，如果key不存在，可以返回None，或者自己指定的value

```python
>>> d.get('Thomas')
>>> d.get('Thomas', -1)
-1
```

要删除一个key，用``pop(key)``方法，对应的value也会从dict中删除。dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key。

+ set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。要创建一个set，需要提供一个list作为输入集合：

```python
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}
```

通过``add(key)``方法可以添加元素到set中，可以重复添加，但不会有效果。通过``remove(key)``方法可以删除元素

+ 在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。
+ Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。所以，定义默认参数要牢记一点：默认参数必须指向不变对象！

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```

+ 要定义出这个函数，我们必须确定输入的参数。由于参数个数不确定，我们首先想到可以把a，b，c……作为一个list或tuple传进来,但是调用的时候，需要先组装出一个list或tuple.定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个*号。在函数内部，参数numbers接收到的是一个tuple，因此，函数代码完全不变。Python允许你在list或tuple前面加一个*号，把list或tuple的元素变成可变参数传进去

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

+ 可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。和可变参数类似，也可以先组装出一个dict，然后，把该dict转换为关键字参数传进去.

```python
>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

``**extra``表示把``extra``这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的一份拷贝，对kw的改动不会影响到函数外的extra。

+ 对于关键字参数，函数的调用者可以传入任意不受限制的关键字参数。至于到底传入了哪些，就需要在函数内部通过kw检查。

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

如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：

```python
def person(name, age, *, city, job):
    print(name, age, city, job)
```

和关键字参数**kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。调用方式如下：

```python
>>> person('Jack', 24, city='Beijing', job='Engineer')
Jack 24 Beijing Engineer
```

使用命名关键字参数时，要特别注意，*不是参数，而是特殊分隔符。

+ 对于任意函数，都可以通过类似``func(*args, **kw)``的形式调用它，无论它的参数是如何定义的。要注意定义可变参数和关键字参数的语法：``*args``是可变参数，args接收的是一个tuple；``**kw``是关键字参数，kw接收的是一个dict。可变参数既可以直接传入：``func(1, 2, 3)``，又可以先组装list或tuple，再通过``*args``传入：``func(*(1, 2, 3))``；关键字参数既可以直接传入：``func(a=1, b=2)``，又可以先组装dict，再通过``**kw``传入：``func(**{'a': 1, 'b': 2})``。定义命名的关键字参数不要忘了写分隔符``*``，否则定义的将是位置参数。

+ 使用递归函数需要注意防止栈溢出。在计算机中，函数调用是通过栈（stack）这种数据结构实现的，每当进入一个函数调用，栈就会加一层栈帧，每当函数返回，栈就会减一层栈帧。由于栈的大小不是无限的，所以，递归调用的次数过多，会导致栈溢出。

+ 解决递归调用栈溢出的方法是通过尾递归优化，事实上尾递归和循环的效果是一样的，所以，把循环看成是一种特殊的尾递归函数也是可以的。尾递归是指，在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。

上面的``fact(n)``函数由于``return n * fact(n - 1)``引入了乘法表达式，所以就不是尾递归了。要改成尾递归方式，需要多一点代码，主要是要把每一步的乘积传入到递归函数中：

```python
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)
```

可以看到，``return fact_iter(num - 1, num * product)``仅返回递归函数本身，``num - 1``和``num * product``在函数调用前就会被计算，不影响函数调用。

尾递归调用时，如果做了优化，栈不会增长，因此，无论多少次调用也不会导致栈溢出。遗憾的是，大多数编程语言没有针对尾递归做优化，Python解释器也没有做优化，所以，即使把上面的fact(n)函数改成尾递归方式，也会导致栈溢出。