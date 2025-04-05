#Note
27-05-2024
Tags: [[Zettel/obsidian/3 - Tags/Mathe]] [[Grundlagen-&-Infinitesimalrechnung]] [[Python]] [[Data-Science]]

### Funktionen mit einer Variablen
Einfache lineare Funktion in Python.
z.B. $f(x)=2x\ +\ 1$


```python
def f(x):
    return 2 * x + 1

x_values = [0, 1, 2, 3, 4]

for x in x_values:
    y = f(x)
    print(y)
```

Einfache Funktionen mit  Sympy in Python plotten. 
! Sympy basiert auf matplotlib, deshalb muss dieses Packet ebenfalls installiert sein


```python
from sympy import *

x = symbols('x')
f = x*2 + 1
plot(f)
```


```python
from sympy import *

x = symbols('x')
f = -x**3 + 1
plot(f)
```

### Funktionen mit zwei Variablen

Funktionen können auch zwei unabhängige Variablen als Eingabewerte enthalten. 

$f(x, y)= 2x\ +\ 3y$

ein Beispiel ist die Wellenfunktion    $s(x,t)=\hat{s}\times \sin (2\pi(\frac{t}{T}-\frac{x}{\lambda}))$  

Auch diese Funktionen lassen sich in Python plotten.


```python
from sympy import *
from sympy.plotting import plot3d

x, y = symbols('x y')
f = 2*x + 3*y
plot3d(f)
```


```python
from sympy import *
from sympy.plotting import plot3d
import matplotlib.pyplot as plt

auslenkung = 2 # centimeters
periodendauer = 7 # seconds
wellenlaenge = 2*pi

x, t = symbols('x t')
f = auslenkung * sin(2 * pi * ((t/periodendauer) - (x/wellenlaenge)))
plot3d(f)
```





---
## Info

[[Mathe Basics für Data Scientists]]