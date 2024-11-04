# dask

## h2

### h3

titlepage

---

layout: false

## What is Dask?

- Scale common data science workflows to many processors
- Numpy, pandas operations in parallel
- Flexible, lightweight
- Used in many libraries
- Similar API:s

---

##  

* Big data
* Environment
* Dask Dataframes
* Diagnostics

---

* Execute code
* Errors
* Scroll to bottom of traceback
* Useful info about error
* Stack overflow
* Issue tracker on github

---

## What is big data?
* Small RAM
* Medium Exceed RAM on disk
* Large, not on physical local drive only cloud

Any data that need extra handling

4 vs of big data:

volume
velocity
veracity - uncertainty of data
variety - type of data
---

## setup

~~~
from dask.distributed import Client
client = Client(workers=4)
# now do some work in parallel
client.close()
~~~

---

## delayed execution

~~~
from dask import delayed

x = delayed(f)()
y = delayed(f)()
z = delayed(g)(x, y)
z.compute()

~~~

* lazy evaluation

## visualize the task graph

~~~
z.visualize()
~~~

  
