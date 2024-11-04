# exceptions

## h2

### h3

titlepage

---

layout: false

## Exceptions

- used to handle errors during execution
- can be caught and handled


## What are exceptions?

* An exception is a signal that a condition has occurred that can't be easily handled using the normal flow-of-control of a Python program.
* Exceptions are often defined as being "errors" but this is not always the case.
* Exceptions are objects that are raised when an error occurs in a program.
* When an exception is raised, the program stops executing the current code and jumps to the nearest exception handler.
* If an exception is not caught, the program will terminate with an error message.

---

## What are some common exceptions?

* `SyntaxError` - Raised when the code is not valid Python.
* `IndentationError` - Raised when there is an incorrect indentation.
* `NameError` - Raised when a variable is not found in the local or global scope.
* `TypeError` - Raised when an operation or function is applied to an object of inappropriate type.
* `ValueError` - Raised when a function receives an argument of the correct type but an inappropriate value.
* `ZeroDivisionError` - Raised when the second argument of a division or modulo operation is zero.

---

## Example

A script handles a file provided by as a command-line argument

~~~
# read_content.py
import sys
f = open(sys.argv[1])
content = f.read()
...
~~~

~~~
$ python -i read_file.py
Traceback (most recent call last):
  File "...read_content.py", line 3, in <module>
    open(sys.argv[1])
         ~~~~~~~~^^^
IndexError: list index out of range
>>> sys.argv
['read_content.py']
~~~

* The script expects a command-line argument but none was provided.
* The `IndexError` exception is raised because the script tries to access an element of a list that does not exist.


---

### Look before you leap (LBYL)

* check the conditions for performing an operation before executing it

~~~
# read_content-2.py
import sys
if len(sys.argv) > 1:
    open(sys.argv[1])
else:
    print(f"Usage: {sys.argv[0]} <filename>")
~~~

~~~
$ python -i open_file-v2.py
Usage: open_file-v2.py <filename>
~~~


### Easier to ask for forgiveness than permission (EAFP)

* try to perform an operation and handle errors afterwards

~~~
# open_file.py
import sys
try:
    open(sys.argv[1])
except IndexError:
    print(f"Usage: {sys.argv[0]} <filename>")
~~~

## How to

* Consider old and new exceptions
   - old bubble up from a function call

* Consider recoverable and non-recoverable exceptions
