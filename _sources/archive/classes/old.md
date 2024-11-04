# Introduction to classes

## BB3110

Computational Python

---

layout: false

## Classes and Exceptions

### Object orientation

* The term object orientation refers to a way of grouping data and functions together.
* A class is associated with data members and methods (functions) operating on the data
* The class definition is a template for variable construction
* Invoking the class name as if it were a function is said to create an instance of the class
* The `__init__` method of the class initializes the new object
* One objective of of object oriented approach is to reuse old code
* sub-classing: reusing an existing class, modifying parts
* inheritance: a subclass inherits all properties of the parent class 

---

### First example


```
>>> class Fruit:
...     """A fruit object class"""
...
...     def __init__(self, name):
...         print("entering Fruit.__init__")
...         self.name = name
...
...     def __str__(self):
...         print("entering Fruit.__str__")
...         return f"{self.name} tastes sweet!"

```
```
>>> fruit = Fruit("Banana")
entering Fruit.__init__

```

```
>>> str(fruit)
entering Fruit.__str__
'Banana tastes sweet!'

```

```
>>> print(fruit)
entering Fruit.__str__
Banana tastes sweet!

```
---

### A modified class

* A subclass of ``Fruit`` (parent or super class)
* Redefine the ``__str__`` function

```
>>> class Citrus(Fruit):
...     def __str__(self):
...         print("entering Citrus.__str__")
...         return f"{self.name} tastes sour"

```
```
>>> lemon = Citrus("Lemon")
entering Fruit.__init__

```
```
>>> print(lemon)
entering Citrus.__str__
Lemon tastes sour

```

* Note that we enter the `__init__` method of the parent class `Fruit`

---

### A more general class

Consider a more specialized class

* Let taste be an attribute of the object (that can change) 
* We overwride both the ``__init__`` and ``__str__``
* Call the ``__init__`` function of the parent class to initialize the inherited attributes

```
>>> class FruitWithTaste(Fruit):
...     """ A fruit object class with taste"""
...     
...     def __init__(self, name, taste="sweet"):
...         print("entering FruitWithTaste.__init__")
...         super().__init__(name) 
...         self.taste = taste
...
...     def __str__(self):
...         print("entering FruitWithTaste.__str__")
...         return f"{self.name} tastes {self.taste}!"

```
```
>>> apple = FruitWithTaste("Apple")
entering FruitWithTaste.__init__
entering Fruit.__init__

```
```
>>> print(apple)
entering FruitWithTaste.__str__
Apple tastes sweet!

```

---

### Methods that alter data

* It possible to alter data (object attributes) directly by assignment  (no private variables)
::
```
    apple.taste = "sour"
```

* o-o approach - only alter data through defined class methods 

```
>>> class FruitWithTaste(Fruit):
...     """ A fruit object class with taste"""
...     
...     def __init__(self, name, taste="sweet"):
...         print("entering FruitWithTaste.__init__")
...         Fruit.__init__(self, name) 
...         self.taste = taste
...
...     def __str__(self):
...         print("entering FruitWithTaste.__str__")
...         return f"{self.name} tastes {self.taste}!"
...     def rot(self):
...         self.taste = "rotten"

```
---

```
>>> apple = FruitWithTaste("Apple")
entering FruitWithTaste.__init__
entering Fruit.__init__

```

```
>>> apple.rot()

```

```
>>> print(apple)
entering FruitWithTaste.__str__
Apple tastes rotten!

```

---

## Exceptions

### Error handling in python

* When there is an error, the program stops with a stack trace
* The error type is written
* When the error condition is detected the program is said to *raise an exception*
* The ``try..except`` statement provides the programmer with tools to handle potential errors

---

### Common errors

Missing argument 

```    
    #exceptions.py
    import sys
    arg1 = sys.argv[1]
    ...

```
```
    $ python exceptions.py
    Traceback (most recent call last):
      File "exceptions.py", line 3, in <module>
        arg1 = sys.argv[1]
    IndexError: list index out of range

```
* The ``IndexError`` exception is raised because the code attempts to address a list element outside the range
    
---

### File missing
```
    ...
    arg1 = sys.argv[1]
    inputfile  = open(arg1)

```
```bash
    $ python exceptions.py notafile
    Traceback (most recent call last):
      File "exceptions.py", line 4, in <module>
        inputfile = open(arg1)
    FileNotFoundError: [Errno 2] No such file or directory: 'notafile'

```

* The ``FileNotFoundError`` exception is raised because the code attempts to open a non-existing file
* By default, the ``open`` statement assumes open for reading.

---

### Handling errors

* Errors which can be predicted can be handled by a ``try-except`` block

```
import sys

try:
    f = open(sys.argv[1])
except(IndexError):
    print(f"Usage: {sys.argv[0]}  <filename>")
    sys.exit(1)
except(FileNotFoundError):
    print(f"{sys.argv[0]}: file {sys.argv[1]} not found")
    sys.exit(1)

class WrongFileType(Exception):
    pass

if not sys.argv[1].endswith('.txt'):
    raise WrongFileType
    

```
```
$ python exceptions.py
Usage: exceptions.py <filename>
$ python exceptions.py notafile
exceptions.py: file notafile not found
$ python exceptions somefile.wrongtype
Traceback (most recent call last):
  File "exceptions.py", line 16, in <module>
    raise WrongFileType
__main__.WrongFileType


```
