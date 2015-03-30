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

















