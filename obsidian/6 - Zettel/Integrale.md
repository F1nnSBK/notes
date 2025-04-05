## Integrale $\int$
#Note
28-05-2024
Tags: [[Zettel/obsidian/3 - Tags/Mathe]] [[Data-Science]] [[Python]] [[Grundlagen-&-Infinitesimalrechnung]]

Das Integral ist die Umkehrung der Ableitung, statt *abgeleitet*, wird *integriert* oder *aufgeleitet*. Ein Integral einer Funktion $\int f(x)\ dx$ gibt die *Fläche unter dem Graphen* an.

Integralberechnung mit der *integrate()-Funktion*. Sie nimmt die Funktion $f$ und eine range aus ([Variable nach der aufgeleitet wird], [Untere Grenze], [Obere Grenze]) als Argumente.


```python
from sympy import *

x = symbols('x')

f = x**2 + 1

area = integrate(f, (x, 0, 1))

print(area)
```

Integralberechnung mit der Approximation (Herleitung).


```python

def approx_integral(a, b, n, f):
    delta_x = (b - a) / n
    total_sum = 0
    
    for i in range(1, n + 1):
        midpoint = 0.5 * (2 * a + delta_x * (2 * i -1))
        total_sum += f(midpoint)
    
    return total_sum * delta_x

def my_function(x):
    return x**2 + 1 

area = approx_integral(a=0, b=1, n=1000000, f=my_function)    

print(area)
```





---
## Info

[[Mathe Basics für Data Scientists]]