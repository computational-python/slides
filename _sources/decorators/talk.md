<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Decorators

Computational Python

---

layout: false


## Decorators


* alter the behaviour of a function
* function which takes a function as a input
* and returns another function as output

---

### Example

```
>>> def verbose(func):
...     def wrapper():
...         print "before", func.__name__
...         result = func()
...         print "after", func.__name__
...         return result
...     return wrapper
```

* original

```
    def hello():
        print "hello"
    hello()
    hello
```

* modified

```
    hello = verbose(hello)
    hello()
    before hello
    hello
    after hello
```
---

### Decorator syntax

There is a special syntax for this type of modifications

```
    @verbose
    def hello():
        print "hello"
    hello()
    before hello
    hello
    after hello
```

---

### General case

* handle arbitrary arguments
* in a function definition

```
    def f(*args, **kwargs):
        print args
        print kwargs
```

* accepts arbitrary arguments
* `args`  is a list
* `kwargs` is a dictionary

A function call 

```
    f(1, 2, c=3, d=4)
```
gives input variables
```
    args -> (1, 2)
    kwargs -> {'c': 3, 'd': 4}
```

---

### General wrapper

```
    def verbose2(func):
        def wrapper(*args, **kwargs):
            print "before", func.__name__
            print "with arguments", args, kwargs
            result = func(*args, **kwargs)
            print "after", func.__name__
            return result
        return wrapper
```

```
    @verbose2
    def hello2(name):
        print "Hello %s!" % name
```
```
    hello2('Dolly')
    before hello2
    with arguments ('Dolly',) {}
    Hello Dolly!
    after hello2
```

---

### A parameterized wrapper

```
    def debug(flag=False):
        def verbose3(func):
            def wrapper(*args, **kwargs):
                if flag:
                    print "before", func.__name__,
                    print "with arguments", args, kwargs
                result = func(*args, **kwargs)
                if flag:
                    print "after", func.__name__
                return result
            return wrapper
        return verbose3

```

---

### Usage

```
    @debug(False)
    def hello3(name):
        print "Hello again %s!" % name

    hello3('Dolly')
    Hello again Dolly!
```

```
    @debug(True)
    def hello3(name):
        print "Hello again %s!" % name

    hello3('Dolly')
    before hello3 with arguments ('Dolly',) {}
    Hello again Dolly!
    after hello3
```

---

### Common usage

* Inspecting arguments
* Logging
* Timing
* Retrying a function that fails
* Redirecting output

Literature: Guide to Learning Python Decorators , by Tim Mattsson
