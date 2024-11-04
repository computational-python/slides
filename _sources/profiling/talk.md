<style>
.centered {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
<link rel="stylesheet" href="/js/highlight/styles/gruvbox-light.css">
<script src="/js/highlight/highlight.pack.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# Profiling

Computational Python
KTH

---

layout: false


# TOC
* [Profiling](#1)
  + [Learning Goals](#2)
  + [Profiling](#3)
  + [Line profiling](#6)

---

## Learning Goals

* to profile programs
* to do line-by-line profiling
* to profile short code pieces

---


## Profiling

* As Python programs can be potentially slow it is important to be able to
identify the bottlenecks of the code

* The most important rule is: never optimize code without profiling

* Even very experience programmers guess wrong

---

### Example


```
#hello_goodbye.py
import time
def hello():
    time.sleep(1)
    print("Hello")

def goodbye():
    time.sleep(2)
    print("Goodbye")

def main():
    hello()
    goodbye()

if __name__ == "__main__":
    main()
```

```
>>> import hello_goodbye
>>> hello_goodbye.main()
Hello
Goodbye

```

---
### Analyis: the cProfile module

```
>>> import cProfile
>>> cProfile.run('hello_goodbye.main()')
Hello
Goodbye
         10 function calls in 3.003 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    3.003    3.003 <string>:1(<module>)
        1    0.000    0.000    3.003    3.003 hello_goodbye.py:11(main)
        1    0.000    0.000    1.001    1.001 hello_goodbye.py:3(hello)
        1    0.000    0.000    2.002    2.002 hello_goodbye.py:7(goodbye)
        1    0.000    0.000    3.003    3.003 {built-in method builtins.exec}
        2    0.000    0.000    0.000    0.000 {built-in method builtins.print}
        2    3.003    1.502    3.003    1.502 {built-in method time.sleep}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}


```

Statistics

* number of times a function was called
* time spent per function
* time spent per function including subcalls (cumulative)


---

## Line profiling


Rather than knowing how much time is spent in the function we may want to know line-by-line what happens


### The `line_profiler` module

* Use the third-party package `line_profiler` to get timing statistics line-by-line
* Install with

```
$ pip install line_profiler
```


* The `line-profiler` package contains a script `kernprof` which is used to execute your
file (instead of `python`)
* `kernprof` defines a decorator which you can use to analyze the function in question

---
The steps are:

* Decorate the function you want to time with the `@profile` decorator
* Execute the script with the `kernprof` script

```
$ kernprof -l hello_goodbye.py 
Hello
Goodbye
Wrote profile results to hello_goodbye.py.lprof
$ python -m line_profiler hello_goodbye.py.lprof
Timer unit: 1e-06 s

Total time: 3.00327 s
File: hello_goodbye.py
Function: main at line 11

Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
    11                                           @profile
    12                                           def main():
    13         1      1001139 1001139.0     33.3      hello()
    14         1      2002132 2002132.0     66.7      goodbye()

```

---
### Profiling code snippets


* The timeit module executes a single statement $10^6$ times
* An optional setup parameter
* Report the time

```
    import timeit
    print(timeit.timeit('math.sqrt(2.0)', setup='import math'))
    0.288702964783
    print(timeit.timeit('sqrt(2.0)', setup='from math import sqrt'))
    0.2072930336
```

---

### Summary

* `profile`  module  for function-level profiling your code
* `line_profiler` module for line-level profiling your code
* `timeit` module for timing short code snippets

Do not ever optimize your code without profiling

