<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
# Parallel calculations

## 

Computational Python
KTH

---

layout: false

# Parallel computations in Python?

* MPI
* the GIL
* subprocessing
* threaded calculations 

---

## Processes

* A running program is a process
* Each process has its own state (memory, variables files)
* Subprocess run as independent programs
* Processes can communicate (IPC), messages or memory
* Multiple process can solve independent tasks at the same time

---

## Threads

* Like process, but more "lightweight"
* Independent control flow
* Threads in a process share data and system resources
* Python Global Interpreter Lock (GIL) prevents thread concurrency
* Fine for IO-bound problems
* Not useful for CPU-bound problems

---

## MPI

~~~
#!/usr/bin/env python
"""
Parallel Hello World
"""

from mpi4py import MPI
import sys

size = MPI.COMM_WORLD.Get_size()
rank = MPI.COMM_WORLD.Get_rank()
name = MPI.Get_processor_name()

sys.stdout.write(
    "Hello, World! I am process %d of %d on %s.\n"
    % (rank, size, name))
~~~
---


## The `multiprocessing` module

Example:

The `map` function applies a function to a sequence of objects

Serial
```
map(f, tasks)

```

parallel version
```
import multiprocessing
pool = multiprocessing.Pool()
pool.map(f, tasks)
```

* The `Pool` class represents ...
* The instances of the `Pool` class have  a `map` method which behaves like the builtin `map` function

---

## The `concurrent`module (python3)

```
import concurrent.futures
with concurrent.futures.ProcessPoolExecutor() as executor:
    executor.map(f, tasks)
```
or

```
with concurrent.futures.ThreadPoolExecutor() as executor:
    executor.map(f, tasks)
```

