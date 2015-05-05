---
layout: post
title: "Python Tutorial"
date: 2015-03-29 11:17
comments: true
categories: 
- dev
- python
---
<center><p><img src="/images/python_logo.png"></p></center>
*   [Whetting Your Appetite](#1)
*   [Using the Python Interpreter](#2)
    *   [Invoking the Interpreter](#2.1)
        *   [Argument Passing](#2.1.1)
        *   [Interactive Mode](#2.1.2)
    *   [The Interpreter and Its Environment](#2.2)
        *   [Source Code Encoding](#2.2.1)
*   [An Informal Introduction to Python](#3)
    *   [Using Python as Calculator](#3.1)
        *   [Numbers](#3.1.1)
        *   [Strings](#3.1.2)
        *   [Lists](#3.1.3)
    *   [First Steps Towards Programming](#3.2)
*   [More Control Flow Tools](#4)
    *   [if Statements](#4.1)
    *   [for Statements](#4.2)
    *   [The range() Function](#4.3)
    *   [break and continue Statements,and else Clauses and Loops](#4.4)
    *   [pass Statements](#4.5)
    *   [Defining Functions](#4.6)
    *   [More on Defining Functions](#4.7)
        *   [Default Argument Values](#4.7.1)
        *   [Keyword Arguments](#4.7.2)
        *   [Arbitrary Argument Lists](#4.7.3)
        *   [Unpacking Argument Lists](#4.7.4)
        *   [Lambda Expressions](#4.7.5)
        *   [Documentation Strings](#4.7.6)
        *   [Function Annotations](#4.7.7)
    *   [Intermezzo: Coding Style](#4.8)
*   [Data Structures](#5)
    *   [More on Lists](#5.1)
        *   [Using Lists as Stacks](#5.1.1)
        *   [Using Lists as Queues](#5.1.2)
        *   [List Comprehensions](#5.1.3)
        *   [Nested List Comprehensions](#5.1.4)
    *   [The del Statement](#.5.2)
    *   [Tuples and Sequences](#5.3)
    *   [Sets](#5.4)
    *   [Dictionaries](#5.5)
    *   [Looping Techniques](#5.6)
    *   [More on Conditions](#5.7)
    *   [Comparing Sequences and Other Types](#5.8)
*   [Modules](#6)
    *   [More on Modules](#6.1)
        *   [Executing modules as scripts](#6.1.1)
        *   [The Module Search Path](#6.1.2)
        *   ["Compiled" Python files](#6.1.3)
    *   [Standard Modules](#6.2)
    *   [The dir() Function](#6.3)
    *   [Packages](#6.4)
        *   [Importing * From a Package](#6.4.1)
        *   [Intra-package References](#6.4.2)
        *   [Packages in Multiple Directories](#6.4.3)
*   [Input and Output](#7)
    *   [Fancier Output Formatting](#7.1)
        *   [Old String Formatting](#7.1.1)
    *   [Reading and Writing Files](#7.2)
        *   [Methods of File Objects](#7.2.1)
        *   [Saving structured data with json](#7.2.2)
*   [Errors and Exceptions](#8)
    *   [Syntax Errors](#8.1)
    *   [Exceptions](#8.2)
    *   [Handling Exceptions](#8.3)
    *   [Raising Exceptions](#8.4)
    *   [User-defined Exceptions](#8.5)
    *   [Defining Clean-up Actions](#8.6)
    *   [Predefined Clean-up Actions](#8.7)
*   [Classes](#9)
    *   [A Word About Names and Objects](#9.1)
    *   [Python Scopes and Namespaces](#9.2)
        *   [Scopes and Namespacs Example](#9.2.1)
    *   [A First Look at Classes](#9.3)
        *   [Class Definition Syntax](#9.3.1)
        *   [Class Objects](#9.3.2)
        *   [Instance Objects](#9.3.3)
        *   [Method Objects](#9.3.4)
        *   [Class and Instance Variables](#9.3.5)
    *   [Random Remarks](#9.4)
    *   [Inheritance](#9.5)
        *   [Multiple Inheritance](#9.5.1)
    *   [Private Variables](#9.6)
    *   [Odds and Ends](#9.7)
    *   [Exceptions Are Classes Too](#9.8)
    *   [Iterators](#9.9)
    *   [Generators](#9.10)
    *   [Generator Expressions](#9.11)
*   [Brief Tour of the Standard Library](#10)
    *   [Operating System Interface](#10.1)
    *   [File Wildcards](#10.2)
    *   [Command Line Arguments](#10.3)
    *   [Error Output Redirection and Program Termination](#10.4)
    *   [String Pattern Matching](#10.5)
    *   [Mathematics](#10.6)
    *   [Internet Access](#10.7)
    *   [Dates and Times](#10.8)
    *   [Data Compression](#10.9)
    *   [Performance Measurement](#10.10)
    *   [Quality Control](#10.11)
    *   [Batteries Included](#10.12)
*   [Brief Tour of the Standard Library - Part II](#11)
    *   [Output Formatting](#11.1)
    *   [Templating](#11.2)
    *   [Working with Binary Data Record Layouts](#11.3)
    *   [Multi-threading](#11.4)
    *   [Logging](#11.5)
    *   [Weak References](#11.6)
    *   [Tools for Working with Lists](#11.7)
    *   [Decimal Floating Point Arithmetic](#11.8)

<!-- more -->
<h2 id="1">Whetting Your Appetite</h2>
<h2 id="2">Using the Python Interpreter</h2>
<h3 id="2.1">Invoking the Interpreter</h3>

``python -c command [arg]``,``python -m module [arg]``

<h4 id="2.1.1">Argument Passing</h4>

When known to the interpreter, the script name and additional arguments thereafter are turned into a list of strings and assigned to the argv variable in the sys module. You can access this list by executing ``import sys``.

<h4 id="2.1.2">Interactive Mode</h4>

<h3 id="2.2">The Interpreter and Its Environment</h3>
<h4 id="2.2.1">Source Code Encoding</h4>

It is also possible to specify a different encoding for source files. In order to do this, put one more special comment line right after the ``#!`` line to define the source file encoding:``# -*- coding: encoding -*-``.

<h2 id="3">An Informal Introduction to Python</h2>
<h3 id="3.1">Using Python as a Calculator</h3>
<h4 id="3.1.1">Numbers</h4>

Division (``/``) always returns a float. To do floor division and get an integer result (discarding any fractional result) you can use the ``//`` operator; to calculate the remainder you can use ``%``.With Python, it is possible to use the ``**`` operator to calculate powers.

In interactive mode, the last printed expression is assigned to the variable ``_``.This variable should be treated as read-only by the user. Don’t explicitly assign a value to it — you would create an independent local variable with the same name masking the built-in variable with its magic behavior.

<h4 id="3.1.2">Strings</h4>

Besides numbers, Python can also manipulate strings, which can be expressed in several ways. They can be enclosed in single quotes (``'...'``) or double quotes (``"..."``) with the same result. ``\`` can be used to escape quotes.

If you don’t want characters prefaced by ``\`` to be interpreted as special characters, you can use raw strings by adding an ``r`` before the first quote.``print(r'C:\some\name')``.

String literals can span multiple lines. One way is using triple-quotes: ``"""..."""`` or ``'''...'''``. End of lines are automatically included in the string, but it’s possible to prevent this by adding a ``\`` at the end of the line. 

```python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

Strings can be concatenated (glued together) with the ``+`` operator, and repeated with ``*``.Two or more string literals (i.e. the ones enclosed between quotes) next to each other are automatically concatenated.This only works with two literals though, not with variables or expressions.If you want to concatenate variables or a variable and a literal, use ``+``.

Strings can be indexed (subscripted), with the first character having index 0. There is no separate character type; a character is simply a string of size one.Indices may also be negative numbers, to start counting from the right.Note that since -0 is the same as 0, negative indices start from -1.

In addition to indexing, slicing is also supported. While indexing is used to obtain individual characters, slicing allows you to obtain substring.Note how the start is always included, and the end always excluded. This makes sure that ``s[:i] + s[i:]`` is always equal to ``s``.

Slice indices have useful defaults; an omitted first index defaults to zero, an omitted second index defaults to the size of the string being sliced.

One way to remember how slices work is to think of the indices as pointing between characters, with the left edge of the first character numbered 0. Then the right edge of the last character of a string of n characters has index n, for example:

```bash
 +---+---+---+---+---+---+
 | P | y | t | h | o | n |
 +---+---+---+---+---+---+
 0   1   2   3   4   5   6
-6  -5  -4  -3  -2  -1
```

Python strings cannot be changed — they are **immutable**. Therefore, assigning to an indexed position in the string results in an error.

<h4 id="3.1.3">Lists</h4>

Like strings (and all other built-in sequence type), lists can be indexed and sliced.

All slice operations return a new list containing the requested elements. This means that the following slice returns a new (shallow) copy of the list:

```python
>>> squares[:]
[1, 4, 9, 16, 25]
```

Lists also support operations like concatenation:

```python
>>> squares + [36, 49, 64, 81, 100]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Unlike strings, which are immutable, lists are a mutable type, i.e. it is possible to change their content.You can also add new items at the end of the list, by using the ``append()`` method.

Assignment to slices is also possible, and this can even change the size of the list or clear it entirely:

```python
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> letters
['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> # replace some values
>>> letters[2:5] = ['C', 'D', 'E']
>>> letters
['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> # now remove them
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
>>> # clear the list by replacing all the elements with an empty list
>>> letters[:] = []
>>> letters
[]
```

It is possible to nest lists (create lists containing other lists).

<h3 id="3.2">First Steps Towards Programming</h3>

In Python, like in C, any non-zero integer value is true; zero is false. The condition may also be a string or list value, in fact any sequence; anything with a non-zero length is true, empty sequences are false. 

```python
>>> print('The value of i is', i)
The value of i is 65536
```

The keyword argument end can be used to avoid the newline after the output, or end the output with a different string.

```python
>>> a, b = 0, 1
>>> while b < 1000:
...     print(b, end=',')
...     a, b = b, a+b
...
1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
```

<h2 id="4">More Control Flow Tools</h2>
<h3 id="4.1">if Statements</h3>
<h3 id="4.2">for Statements</h3>

The for statement in Python differs a bit from what you may be used to in C or Pascal. Rather than always iterating over an arithmetic progression of numbers (like in Pascal), or giving the user the ability to define both the iteration step and halting condition (as C), Python’s for statement iterates over the items of any sequence (a list or a string), in the order that they appear in the sequence. 

If you need to modify the sequence you are iterating over while inside the loop (for example to duplicate selected items), it is recommended that you first make a copy. Iterating over a sequence does not implicitly make a copy. The slice notation makes this especially convenient:

```python
>>> for w in words[:]:  # Loop over a slice copy of the entire list.
...     if len(w) > 6:
...         words.insert(0, w)
...
>>> words
['defenestrate', 'cat', 'window', 'defenestrate']
```

<h3 id="4.3">The range() Function</h3>

If you do need to iterate over a sequence of numbers, the built-in function ``range()`` comes in handy. It generates arithmetic progressions.

```python
range(-10, -100, -30)
  -10, -40, -70
```

```python
>>> print(range(10))
range(0, 10)
```

In many ways the object returned by ``range()`` behaves as if it is a list, but in fact it isn’t. It is an object which returns the successive items of the desired sequence when you iterate over it, but it doesn’t really make the list, thus saving space.We say such an object is iterable, that is, suitable as a target for functions and constructs that expect something from which they can obtain successive items until the supply is exhausted.

<h3 id="4.4">break and continue Statements, and else Clauses on Loops</h3>

When used with a loop, the else clause has more in common with the else clause of a try statement than it does that of if statements: **a try statement’s else clause runs when no exception occurs, and a loop’s else clause runs when no break occurs**. 

<h3 id="4.5">pass Statements</h3>

The ``pass`` statement does nothing. It can be used when a statement is required syntactically but the program requires no action. 

```python
>>> while True:
...     pass  # Busy-wait for keyboard interrupt (Ctrl+C)
...
```

<h3 id="4.6">Defining Functions</h3>

The keyword ``def`` introduces a function definition. It must be followed by the function name and the parenthesized list of formal parameters. The statements that form the body of the function start at the next line, and must be indented.

The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table; whereas variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names. Thus, global variables cannot be directly assigned a value within a function (unless named in a global statement), although they may be referenced.

A function definition introduces the function name in the current symbol table. The value of the function name has a type that is recognized by the interpreter as a user-defined function. This value can be assigned to another name which can then also be used as a function.

In fact, even functions without a return statement do return a value, albeit a rather boring one. This value is called None (it’s a built-in name). Writing the value None is normally suppressed by the interpreter if it would be the only value written. 

<h3 id="4.7">More on Defining Functions</h3>

<h4 id="4.7.1">Default Argument Values</h4>

```python
def ask_ok(prompt, retries=4, complaint='Yes or no, please!'):
    while True:
        ok = input(prompt)
        if ok in ('y', 'ye', 'yes'):
            return True
        if ok in ('n', 'no', 'nop', 'nope'):
            return False
        retries = retries - 1
        if retries < 0:
            raise OSError('uncooperative user')
        print(complaint)
```

The default values are evaluated at the point of function definition in the defining scope, so that

```python
i = 5

def f(arg=i):
    print(arg)

i = 6
f()
```

**Important warning**: The default value is evaluated only once. This makes a difference when the default is a mutable object such as a list, dictionary, or instances of most classes. For example, the following function accumulates the arguments passed to it on subsequent calls:

```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
```

This will print

```python
[1]
[1, 2]
[1, 2, 3]
```

If you don’t want the default to be shared between subsequent calls, you can write the function like this instead:

```python
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```

<h4 id="4.7.2">Keyword Arguments</h4>

Functions can also be called using keyword arguments of the form kwarg=value. For instance, the following function:

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
```

accepts one required argument (voltage) and three optional arguments (state, action, and type). This function can be called in any of the following ways:

```python
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

When a final formal parameter of the form ``**name`` is present, it receives a dictionary containing all keyword arguments except for those corresponding to a formal parameter. This may be combined with a formal parameter of the form ``*name`` (described in the next subsection) which receives a tuple containing the positional arguments beyond the formal parameter list. (``*name`` must occur before ``**name``.) 

<h4 id="4.7.3">Arbitrary Argument Lists</h4>

Finally, the least frequently used option is to specify that a function can be called with an arbitrary number of arguments. These arguments will be wrapped up in a tuple.

<h4 id="4.7.4">Unpacking Argument Lists</h4>

The reverse situation occurs when the arguments are already in a list or tuple but need to be unpacked for a function call requiring separate positional arguments. For instance, the built-in ``range()`` function expects separate start and stop arguments. If they are not available separately, write the function call with the *-operator to unpack the arguments out of a list or tuple:

```python
>>> list(range(3, 6))            # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]
```

In the same fashion, dictionaries can deliver keyword arguments with the **-operator.

<h4 id="4.7.5">Lambda Expressions</h4>

Small anonymous functions can be created with the lambda keyword. This function returns the sum of its two arguments: lambda a, b: a+b. Lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. Semantically, they are just syntactic sugar for a normal function definition. Like nested function definitions, lambda functions can reference variables from the containing scope:

```python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```

The above example uses a lambda expression to return a function. Another use is to pass a small function as an argument(++++++question mark++++++):

```python
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

<h4 id="4.7.6">Documentation Strings</h4>
<h4 id="4.7.7">Function Annotations</h4>

Annotations are stored in the __annotations__ attribute of the function as a dictionary and have no effect on any other part of the function. Parameter annotations are defined by a colon after the parameter name, followed by an expression evaluating to the value of the annotation. Return annotations are defined by a literal ->, followed by an expression, between the parameter list and the colon denoting the end of the def statement. The following example has a positional argument, a keyword argument, and the return value annotated with nonsense:

```python
>>> def f(ham: 42, eggs: int = 'spam') -> "Nothing to see here":
...     print("Annotations:", f.__annotations__)
...     print("Arguments:", ham, eggs)
...
>>> f('wonderful')
Annotations: {'eggs': <class 'int'>, 'return': 'Nothing to see here', 'ham': 42}
Arguments: wonderful spam
```

<h3 id="4.8">Intermezzo: Coding Style</h3>

[PEP8](http://www.python.org/dev/peps/pep-0008)

+ Use 4-space indentation, and no tabs.
+ Wrap lines so that they don’t exceed 79 characters.
+ Use blank lines to separate functions and classes, and larger blocks of code inside functions.
+ When possible, put comments on a line of their own.
+ Use docstrings.
+ Use spaces around operators and after commas, but not directly inside bracketing constructs
+ Name your classes and functions consistently; the convention is to use CamelCase for classes and lower_case_with_underscores for functions and methods. Always use self as the name for the first method argument.

<h2 id="5">Data Structures</h2>

<h3 id="5.1">More on Lists</h3>
+ ``list.append(x)``,Equivalent to ``a[len(a):] = [x]``
+ ``list.extend(L)``,Extend the list by appending all the items in the given list. Equivalent to ``a[len(a):] = L``
+ ``list.insert(i, x)``,Insert an item at a given position. The first argument is the index of the element before which to insert, so ``a.insert(0, x)`` inserts at the front of the list, and ``a.insert(len(a), x)`` is equivalent to ``a.append(x)``.
+ ``list.remove(x)``,Remove the first item from the list whose value is x. It is an error if there is no such item.
+ ``list.pop([i])``,the square brackets around i indicates that i is optional,Remove the item at the given position in the list, and return it. If no index is specified, ``a.pop()`` removes and returns the last item in the list.
+ ``list.clear()``,Remove all items from the list. Equivalent to del a[:].
+ ``list.index(x)``,Return the index in the list of the first item whose value is x. It is an error if there is no such item.
+ ``list.count(x)``,Return the number of times x appears in the list.
+ ``list.sort()``,Sort the items of the list in place.
+ ``list.reverse()``,Reverse the elements of the list in place.
+ ``list.copy()``,Return a shallow copy of the list. Equivalent to a[:].

Methods like insert, remove or sort that only modify the list have no return value printed – they return the default None. This is a design principle for all mutable data structures in Python.

<h4 id="5.1.1">Using Lists as Stacks</h4>

``append/pop``

<h4 id="5.1.2">Using Lists as Queues</h4>

```python
from collections import dequeue
queue = dequeue(["eric", "john"])
append/popLeft
```










