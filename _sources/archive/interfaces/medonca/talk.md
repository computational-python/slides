~~~
$ f2py -c source.f90 -m fortran
~~~

~~~
>>> import fortran
>>> import numpy as np
>>> a = np.array([1, 2, 3], np.float32)
>>> fortran.fmodule.fast_reverse(a, 2)
>>> a
array([2., 1., 3.], dtype=float32)

~~~
