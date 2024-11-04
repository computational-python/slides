
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Introduction to Python

## FCA3004

KTH

---
layout: false


## What is Python

<a title="Daniel Stroud, CC BY-SA 4.0
&lt;https://creativecommons.org/licenses/by-sa/4.0&gt;, via Wikimedia Commons"
href="https://commons.wikimedia.org/wiki/File:Guido-portrait-2014.jpg"><img
width="256" alt="Guido-portrait-2014"
src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Guido-portrait-2014.jpg/512px-Guido-portrait-2014.jpg"></a>

Python is a scripting language written by Guido van Rossum 

> *My original motivation for creating Python was the perceived need for a higher
> level language in the Amoeba [Operating Systems] project. I realized that the
> development of system administration utilities in C was taking too long.
> Moreover, doing these things in the Bourne shell wouldn’t work for a variety of
> reasons. … So, there was a need for a language that would bridge the gap
> between C and the shell.*

---

## Who uses Python

From small scripts to large applications

<div class="row">
    <div class="col">
        <code class="hljs">print("Hello world!")</code>
    </div>
    <div class="col">
     <img class="img-fluid" src="{{ base }}/img/th-808922334.jpeg">
    </div>
    <div class="col">
     <img class="img-fluid" src="{{ base }}/img/Instagram_logo_2022.svg.png">
    </div>
</div>

---

## How to run Python

### Command-line

Many Python programs are run from the computer's command-line interface (CLI). 
In the CLI you type commands that the computer interprets and acts upon.

* The CLI on a mac/linux is a terminal program that reads commands in the bash
scripting language (or equivalent)
* The CLI on windows is the console program known as CMD.
* The CLI prompt is a character string printed at the beginning of each line when 
  waiting for a command

In these slides we use the convention

* `$ `  is the bash prompt (mac/linux)
* `C> ` is the CMD prompt (windows)

Typing `python` in the CLI starts an interactive Python session accepting
Python commands 

* `>>> ` is the Python prompt

---


* The Python interactive shell is also known as the REPL (read-evaluate-print-loop)
    - R read a Python expression
    - E evaluate the expression
    - P print the value of the expressions to the screen
    - L loop (start over)
    
~~~
$ python
>>> print("Hello world")
Hello world
>>>
~~~

* Use case: a desktop calculator

~~~
$ python
>>> 10 + 10*0.25
12.5
>>> _  * 2
25.0

~~~

* in this context the underscore `_` means the value of the previous expression

---

### Running Python scripts

Assuming you have a file with Python instructions:

`hello.py `

{{ '<img src="{root}/img/hi.png" width="50%">'.format(root=base) }}

you run the script by giving it as a command-line argument to the python
interpreter

~~~
$ python hello.py
Hello world
Goodbye
~~~

---

## Creating Python scripts

### Text editors

To enter code into files you need to use a text editor (not a word processor
like Microsoft Word). A text editor is good for programming if it automatically
colors special keywords for the programming language of that file. A simple 
editor that fulfills this is `nano`.

{{ '<img src="{chapter}/img/nano.png" height="250">'.format(chapter=base) }}

Developers survey on 
<a href="https://insights.stackoverflow.com/survey/2018/#development-environments-and-tools">
most popular programming editor
</a>: 
normally lists editors like vim, emacs, atom, sublime.
Spending time to learn one well is worth the investment.

---

### IDE:s

IDE = Integrated development environment

~~~
$ pip install mu-editor
$ mu-editor hello.py
~~~

{{ '<img src="{root}/img/mu.png" height="350">'.format(root=base) }}

---

### Visual Studio Code

* Microsoft open source project

{{ '<img src="{root}/img/vsc.png" height="400">'.format(root=base) }}

---

### Pycharm

* Community and Professional editions from JetBrains

{{ '<img src="{root}/img/pycharm.png" height="400">'.format(root=base) }}

* see https://www.jetbrains.com/student/

---

### Notebooks

* work/develop in browser
* mix documentation and code
~~~
$ jupyter notebook
~~~
{{ '<img src="{root}/img/jupyter.png" height="350" >'.format(root=base) }}
* good for exploration/experimentation/demonstration
* not good for writing large structured programs


---

# Language fundamentals

## Python names

* Names in Python are what most languages refer to as variables, or variable names
* To save the value of an object a name can be assigned to it
* The assignment operator is `=`
* Assignment is to bind a name to an object

~~~
>>> x = 8 * 9

~~~

1. The right-hand side is evaluated
1. An `int` object with value 72 is created in memory
1. An association is created with this object and the name `x`

<pre style='background-color:#fbf1c7;'>
                ┏━━┓
                ┃ x┃
    ┌─┐   ┌─┐   ┡━━┩ 
    │8│ * │9│ → │72│
    └─┘   └─┘   └──┘
</pre> 
---

* The name can be used in other expressions as an alias for that value

~~~
>>> print(x + x)
144

~~~

---

## Python values and types

Objects and expressions in Python have a value of some type.
A type determines the range of possible values and operations that are defined
for objects of that type.


###  Numerical types

* whole numbers (`int`): e.g. `-1, 7, 2000`
* decimal numbers (`float`): `3.14, 1.0 -7.25`
* complex numbers (`complex`): `1j, 7+5j`
* logical (`bool`): `True`, `False`


### Numerical operations
* `+` Addition
* `-` Subtraction
* `*` Multiplication
* `/` Division (`7 / 2 → 3.5`)
* `//` Integer floor division (`7 // 2 → 3`)
* `%` Integer modulus (remainder) (`7 % 2 → 1`)

---

### Logical operations
* `and` 
    - `True and True` → `True`
    - `True and False` → `False`
    - `False and False` → `False`
* `or` 
    - `True or True` → `True`
    - `True or False` → `True`
    - `False or False` → `False`
* `not` 
    - `not True` → `False`
    - `not False` → `True`

* equality
    - `1 == 1` → `True`
    - `1 == 1.0` → `True`
    - `1 != 1` → `False`
---
* identity
    - `1 is 1` → `True`
    - `1 is 1.0` → `False`
* relational
    - `1 < 1` → `False`
    - `1 >= 1.0` → `True`
---

###  Strings: `str`

- a string is a sequence of characters [(unicode)](https://home.unicode.org/)
- literal strings are written within quotation marks
- single `'` and double `"` quotation marks have the same status
- three quotation marks limit strings that can span several lines
- there is no single character type

~~~
>>> print("It's time")
It's time

~~~
~~~
>>> print('Our boss is "nice". 😀')
Our boss is "nice". 😀

~~~
~~~
>>> print("""Hello
... world""")
Hello
world

~~~

---

### String operations

* Concatenation

~~~
>>> 'hello' + 'world'
'helloworld'

~~~

* Duplication

~~~
>>> 'bye' * 2
'byebye'

~~~

### Common methods

~~~
>>> 'FORTRAN'.lower()
'fortran'

~~~

~~~
>>> 'c++'.upper()
'C++'

~~~

~~~
>>> 'jo'.capitalize()
'Jo'

~~~

---


## Container types

The most important container types are lists, tuples and dictionaries

### Lists

* A list is a ordered sequence of elements 
* A literal list in code is defined by square brackets and comma-separated
  elements `[0, 1, 2]`
* List can have members of different types
* List members are referenced with `[n]` where `n=0, 1, 2...`
* A list can be empty, `[]`

~~~
>>> colours = ['hearts', 'spades', 'diamonds', 'clubs']
>>> values = [2, 3, 4, 5, 6, 7, 8, 9, 10, 'knight', 'queen', 'king', 'ace']
>>> colours[0]
'hearts'

~~~

*Note that brackets are both used in the notation for a literal list and for
getting a member from a collection (not only for lists)*

~~~
>>> a_list = [1, 'a']
>>> a_member = a_list[0]
~~~

---

### Tuples

* An immutable (unchangeable) sequence of objects
* Similar to lists
* () is the empty tuple
* (1,)  contains 1 element -note the comma

Handy packing and unpacking

```
>>> t = 1, 2 # packing
>>> x, y = t # unpacking
>>> x
1
>>> y
2
>>> x, y = y, x # swapping (packing and unpacking)
>>> x
2
>>> y
1

```
---

### Dictionaries

* The `dict` type define sets of key-value pairs
* Curly braces with comma-separates pairs define a literal dict
* Each pair is separated by a colon `:`
* The key can be any immutable (unchangeable) object
* Very useful for complex structures
* Efficient and highly optimized

```
empty =  {} # empty dict
newdict = {'a': 1, 'b': 2}
```

---

## Program logic

### Repetition (iteration, looping)

* The `for ... in` statement is used repeat the same operation for all elements of a
sequence

* A loop variable will reference the elements of the sequence, one at a time

### Looping over lists

```python
>>> for e in [1, 2, 3]: 
...    print(e)
1
2
3

```
---

### Looping over strings

```python
>>> for c in 'hello':
...     print(c)
h
e
l
l
o

```

### Looping over dictionaries


```python
>>> d = {'a': 1, 'b': 2}
>>> for k, v in d.items():
...    print(k, v)
a 1
b 2

```

```python
>>> for k in d:
...    print(k, d[k])
a 1
b 2

```


---

## Branching

Conditional execution of code blocks depending on whether an expression
evaluates to True or not:

```
>>> if True:
...     print("Yes")
... else:
...     print("No")
Yes

```

```
>>> i = j = 0
>>> if i > j:
...     print(i, " is larger than ", j)
... else:
...     print(i, "is smaller than or equal to", j)
0 is smaller than or equal to 0

```

Most object types have some truthiness. Empty lists in a logical context
evaluate to False, non-empty to True

```
>>> if []:
...     print("Non-empty list")
... else:
...     print("Empty list")
Empty list

```

---

## Program units

### Functions

* Functions are objects that can take some input and return some output.
Functions are the primary way of grouping code into independent units, that can be tested and reused

* Function definitions start `def`, a name,  parentheses with or without
arguments (comma-separated) and a colon.
The body of the function is indented with respect to the `def` keyword.
The last line of a function definition is normally a `return` statement and determines the value of a function call


```
>>> def square(x):
...    x2 = x * x
...    return x2

```

---

* Functions are called with function name and an actual parameter. 

```
>>> square(2)
4

```

* Inside the function the formal parameter `x` becomes a reference to the actual
parameter `2`.


---

### Modules


* a file with Python source
   - name is the filename without the ``.py`` extension
* `import` modules to reuse code
* members of module referenced with dot notation `module.member`

Commonly used Python modules

* ``sys``
* ``os``
* ``math``

---

#### `sys`

* system modules
* needed e.g. for arguments to a script
* `sys.argv` is a list containing command-line arguments
* The first element `sys.argv [0]` is the file name

~~~
import sys
infile = sys.argv[1]
~~~

#### `os`

* Interaction with operating system
* Example: execute a unix command 

```
    import os
    os.system('/bin/date')
```

---

#### `math`

* all basic elementary functions
* fundamental constants

```
    import math
    print(math.sin(math.pi/2))
```


#### Tip

Many use the math modules as a desktop calculator

    $ python
    >>> from math import *
    >>> print(pi/2)
    1.5707963267948966
    >>>

---

### Writing/using your own modules

* Suppose you have written file ``hello.py`` with function ``say_hello``

~~~
#hello.py
def say_hello():
    return "Hello world!"
~~~

* To access the same function in other code, import the module

~~~
>>> import hello
>>> message = hello.say_hello()
>>> print(message)
Hello world!

~~~

---

### Sample code: multiplication table

Version 1: if we only know print
~~~
print(1 * 7)
print(2 * 7)
print(3 * 7)
~~~

Version 2: extract the varying data to a variable
~~~
i = 1
print(i * 7)
i = 2
print(i * 7)
i = 3
print(i * 7)
~~~

Version 3: introduction the for loop, indented blocks
~~~
for i in (1, 2, 3, 4, 5, 6, 7, 8 , 9, 10):
    print(i * 7)
~~~

Version 4: the `range` function
~~~
n = 7
for i in range(1, 11):
    print(i * n)
~~~

---

Version 5: define as a function
~~~
def print_mult_table(n):
    for i in range(1, 11):
        print(i * n)
~~~

Version 6: call the function from the command line
~~~
import sys

def print_mult_table(n):
    for i in range(1, 11):
        print(i * n)

if __name__ == "__main__":
    print_mult_table(int(sys.argv[1]))
~~~

* the system variable `__name__` has the value `"__main__"` (double underscores)
when the file is executed as a program
* it has the value equal to the file name when it is imported as a module
* command-line argments appear in the system variable `sys.argv`, a Python list
which has the file name as the first member (of index zero)

A common pattern when a file is used both as an independent script and as a
module imported by other programs

---

## Summary

* Basic syntax - indentation
* Basic built-in types
* Scalar and container types
* Variables
* Functions
* Modules

---

## Reference

### Standard documentation
* https://docs.python.org/3

### On-line books
* Jake van der Plas: 
<a href="http://nbviewer.jupyter.org/github/jakevdp/WhirlwindTourOfPython/blob/master/Index.ipynb"> A Worldwind Tour of Python</a>
* Jake van der Plas: <a href="https://jakevdp.github.io/PythonDataScienceHandbook/">Data Science Handbook</a>
* Al Sweigart: <a href="https://automatetheboringstuff.com">Automate the Boring Stuff with Python</a>

### Online tutorials
* https://docs.python.org/3/tutorial/
* https://realpython.com
* https://dabeaz-course.github.io/practical-python

### Youtube
* Variables in Python https://www.youtube.com/watch?v=_AEJHKGk9ns


