# Introduction to scipy

## Olav Vahtras

Computational Python

---

layout: false

## SciPy

* Collection of scientific libraries in different disciplines
* http://www.scipy.org
* http://www.scipy-lectures.org

---

### Optimization

* minimizing
* curve fitting
* least-square

### Integration
---

### Minimizing

<img src="morse.png" height="300" />

~~~
>>> import math
>>> import numpy
>>> def f(r, d=2.5, a=1.5, r0=0.5):
...    e=numpy.exp(-a*(r-r0))
...    return d*(1-e)**2

~~~

---

### The Brent method

* Bracketing approach
* No derivatives


~~~
>>> import scipy.optimize
>>> minx = scipy.optimize.brent(f)
>>> print(minx, f(minx))
0.5000000017938965 1.8101612436464225e-17

~~~

---

### Conjugent gradient


* Gradient function can be given
* Approximate gradient used if not


```
>>> minx = scipy.optimize.fmin_cg(f, [1])
Optimization terminated successfully.
         Current function value: 0.000000
         Iterations: 2
         Function evaluations: 27
         Gradient evaluations: 12
>>> print(minx, f(minx))
[0.49999999] [3.18759508e-16]

```

--

### Supply gradient

```
>>> def g(r):
...   d=2.5; a=1.5; r0=0.5
...   e=math.exp(-a*(r-r0))
...   return numpy.array([2*(1-e)*a*e])
>>> minx = scipy.optimize.fmin_cg(f, [1], fprime=g)
Optimization terminated successfully.
         Current function value: 0.000000
         Iterations: 12
         Function evaluations: 59
         Gradient evaluations: 59
>>> print(minx, f(minx))
[0.50000188] [1.9786096e-11]



```
    
---


### The Newton method

* Based on a local second-order expansion
$$
    f(x) = f(x_0) + (x-x_0)f'(x_0) + \frac 1 2 (x-x_0)f''(x_0)
$$

* Minimized by
$$
    f'(x) = f'(x_0) + (x-x_0)f''(x_0) = 0
$$

* Iterative method
$$
    x = x_0 - f'(x_0)/f''(x_0)
$$

---

### using the Newton method

* Hessian is optional - requires more gradient evaluations


```
>>> minx = scipy.optimize.fmin_ncg(f, [1], fprime=g)
Optimization terminated successfully.
         Current function value: 0.000000
         Iterations: 7
         Function evaluations: 14
         Gradient evaluations: 21
         Hessian evaluations: 0
>>> print(minx, f(minx))
[0.5] [6.43644251e-19]

```    

---

### supply the Hessian

```
>>> def h(r):
...   d=2.5; a=1.5; r0=0.5
...   e=math.exp(-a*(r-r0))
...   return 2*a*d*(-a*e*(1-e) + e*(a*e))
>>> minx = scipy.optimize.fmin_ncg(f, [1], fprime=g, fhess=h)
Optimization terminated successfully.
         Current function value: 0.000000
         Iterations: 18
         Function evaluations: 26
         Gradient evaluations: 26
         Hessian evaluations: 18
>>> print(minx, f(minx))
[0.50001252] [8.82312709e-10]


```

---

### using quasi-Newton method

```
* some approximation of the Hessian is maintained
* when the Hessian is too expensive
```

```
>>> minx = scipy.optimize.fmin_bfgs(f, [1], fprime=g)
Optimization terminated successfully.
         Current function value: 0.000000
         Iterations: 6
         Function evaluations: 10
         Gradient evaluations: 10
>>> print(minx, f(minx))
[0.49999996] [9.2095537e-15]



```

---

### using  lBFGS

* inprinciple as BFGS - does not store explicit Hessians


```
>>> minx, fminx, dic = scipy.optimize.fmin_l_bfgs_b(f, [1], fprime=g)
>>> print(minx, f(minx))
[0.5] [9.58482288e-18]

```
```
>>> for k in dic:
...    print(k, dic[k])
grad [-5.87412653e-09]
task CONVERGENCE: NORM_OF_PROJECTED_GRADIENT_<=_PGTOL
funcalls 11
nit 6
warnflag 0

```

---

### Curve-Fitting

* Consider the Morse example ``d=2.5, a=1.5 r0=0.5``
* Add numerical noise


```
>>> numpy.random.seed(0)
>>> x = numpy.arange(0, 5, .1)    
>>> y = f(x) + 0.01*numpy.random.normal(len(x))
>>> print(scipy.optimize.curve_fit(f, x, y))
(array([ 3.82190257,  0.37120316, -1.9718352 ]), array([[ 0.67662808, -0.15619802, -0.73417703],
       [-0.15619802,  0.03843448,  0.19185841],
       [-0.73417703,  0.19185841,  1.03203265]]))

```

---

* Lower the noise


```
>>> x = numpy.arange(0, 5, .1)    
>>> y = f(x) + 0.001*numpy.random.normal(len(x))
>>> print(scipy.optimize.curve_fit(f, x, y))
(array([2.54802944, 1.51797673, 0.49517601]), array([[ 1.74697392e-05, -1.28378073e-05,  2.54218571e-06],
       [-1.28378073e-05,  2.55613581e-05, -5.87018430e-06],
       [ 2.54218571e-06, -5.87018430e-06,  1.99021615e-06]]))


```
---

### Least-Square methods


#### Example

Simple least square fitting to a straight line

* Initialize arrays

```
>>> import numpy
>>> from scipy.optimize import leastsq
>>> x = numpy.arange(0, 10, .01)
>>> k0 = 3.0
>>> l0 = 1.0
>>> y = k0 * x + l0 + numpy.random.randn(len(x))

```

---

* Define residual function
* The parameters to be optimize for the first tuple

```
>>> def resid(p, y, x):
...    k,l = p
...    return y - k*x - l

```

---

* The arguments to ``leastsq`` are the residual function, the initial parameters ``p0`` and the data set ``(y, x)`` in ``args`` 


```
>>> p0 = numpy.array([k0, l0])
>>> plsq = leastsq(resid, p0, args=(y, x))
>>> k, l = plsq[0]
>>> print(k, l)
3.000414056662475 0.9519593064930558


```
---

### Integration

* uses package `scipy.integrate`
* definite integrals in 1-3 dimensions

---

#### Example


The integral

$$   \int_{-\infty}^\infty e^{-x^2} = \sqrt{\pi} $$


```
>>> from scipy.integrate import quad
>>> sqrtpi = math.sqrt(math.pi)
>>> def e2(x):
...    return math.exp(-x*x)
>>> res, err = quad(e2, -10, 10)
>>> print(res, sqrtpi, abs(res-sqrtpi))
1.772453850905516 1.7724538509055159 2.220446049250313e-16


```

---

Other modules in scipy

* Machine learning `scikit-learn`
* Image processing `scikit-image`
* Statistics
* Symbolic mathematics `sympy`
* 3D-plotting 
