# Introduction to iterators

## Olav Vahtras

KTH

---

layout: false

## Iterators and generators

### Iterators

* objects that support iteration protocol
* to be used in for loops

#### Some well-known iterators

* A list

```
>>> for member in ['lists', 'have', 'square', 'brackets:', []]:
...     print(member, end=" ")
lists have square brackets: [] 

```

---

#### Dictionaries support iteration

* The loop variable is the key of the key-value pair

```
>>> dct = {'a':1, 'b':2}
>>> for key in dct:
...     print(key, dct[key])
a 1
b 2

```
--

#### Strings support iteration

```
>>> for char in 'a str':
...     print(char)
a
<BLANKLINE>
s
t
r

```
---

#### File objects  support iteration


<!--
>>> open('123', 'w').write('one\ntwo\nthree\n')
14

-->

    123
    ┌─────┐
    │one   │
    │two   │
    │three │
    └─────┘

~~~
>>> for row in open('123'):
...     print(row, end="")
one
two
three

~~~

convert any iterable to list 

~~~
>>> list(open('123'))
['one\n', 'two\n', 'three\n']

~~~

---

### Consider the list

~~~
>>> li = [0, 1, 2]

~~~

* Lists have an `__iter__` method which is invoked with

```
>>> li_iter = iter(li)

```
* returns an object with a `__next__` method invoked with

```
>>> next(li_iter)
0
>>> next(li_iter)
1
>>> next(li_iter)
2
>>> next(li_iter)
Traceback (most recent call last):
  File "iter.py", line 20, in <module>
    print(li_iter.next())
StopIteration

```

* Illustrates what happens behind the scenes in a for loop

---

### Objects supporting iteration protocol

* *iterables* have a method `__iter__()` which returns an iterator
* the *iterator* has a method `__next__()` producing the next sequence value
* going beyond the last value raises a `StopIteration` - the for loop quits

---

### Defining your own iterator

```
>>> class Counter(object):
...     def __init__(self, size):
...         print(f"__init__: initializing iterable with {size=}")
...         self.size = size
...         self.start = 0
...
...     def __iter__(self):
...         print(f"__iter__: starting up iteration with {self.size=}")
...         return self
...
...     def __next__(self):
...         if self.start < self.size:
...             self.start = self.start + 1
...             return self.start
...         raise StopIteration

```
```
>>> c = Counter(3)
__init__: initializing iterable with size=3
>>> for num in c:
...     print(num, end=" ")
__iter__: starting up iteration with self.size=3
1 2 3 

```

---

### Generators


* Functions that contain the  yield statment
* Support iteration protocol - they return an iterator
* Resumable
* Generators are not executed when invoked, they are iterated over

### function vs. generator

```
>>> def f(n):
...    return n
>>> print(type(f), type(f(1)))
<class 'function'> <class 'int'>

```
```
>>> def g(n):
...    yield n
>>> print(type(g), type(g(1)))
<class 'function'> <class 'generator'>

```

---

### Example

```
>>> def g(n):
...     print('enter g with',n)
...     yield n
...     print('after yield')

```
```
>>> g2=g(2)
>>> g2.__next__()
enter g with 2
2
>>> g2.__next__() #doctest: +SKIP
Traceback (most recent call last):
...
StopIteration

```

* So: this function appears to pause at the yield statement after returning the value and continue from there the next time the next() method is called...

* When the function exits a StopIteration exception is raised

---

### Example

```
>>> def g(n):
...     print('enter g with ',n)
...     i=0
...     while i < n:
...         yield i
...         print('after yield')
...         i += 1
...     print('after while')

```
--

```
>>> g2 = g(2); next(g2); next(g2)
enter g with  2
0
after yield
1

```

--

```
>>> next(g2) #doctest: +SKIP
after yield
after while
...
StopIteration

```

### Iterator in for loop

```
>>> for  i in g(5):
...     print(i, end=" ")
enter g with  5
0 after yield
1 after yield
2 after yield
3 after yield
4 after yield
after while

```

### Example: fibonacci


```
>>> def fib(n):
...     a = 1
...     b = 2
...     while a < n:
...         yield a
...         a, b = b, a + b
>>> for i in fib(5):
...     print(i, end=" ")
1 2 3 

```

### Convert to list

```
It is always possible to convert a generator to a list


```
>>> print(list(fib(100)))
[1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

```

---

### Summary

* Several common types support iteration (list, dict, file, str)
* Objects support iteration have an `__iter__` function returning an iterable
* The iterables have a `next` method that steps through some sequence
* Generators are functions with a `yield` statement and work like iterators
