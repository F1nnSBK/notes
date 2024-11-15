#Note
28-05-2024
Tags: [[Mathe]] [[Data-Science]] [[Python]] [[Grundlagen-&-Infinitesimalrechnung]]

## Ableitungen $f'(x)$

Die Ableitung gibt die *momentane Änderungsrate* an einem Punkt $x$ an.

In Python kann man mit SymPy die *diff()-Funktion* nutzen. Es sind dabei alle Ableitungsregeln enthalten.
Die *subs()-Funktion* nimmt als erstes Argument den vorhandenen Wert und als zweites, den Wert, mit dem der erste ersetzt erden soll.


```python
from sympy import *

x = symbols('x')

x_value = 2

f = x**2

dx_f = diff(f)
print(dx_f)
print(dx_f.subs(x, x_value))
```

## Partielle Ableitungen $f'(x,y)$

Bei partiellen Ableitungen wird jede Variable extra abgeleitet.

Die *momentane Änderungsraten* werden dabei als Gradienten betrachtet. 

Der *diff()-Funktion* werden dabei neben der Funktion $f$ auch die Variable, nach der abgeleitet werden soll, mitgegeben. z.B. $x$ oder $y$


```python
from sympy import * 
from sympy.plotting import plot3d

x, y = symbols('x y')

f = 2*x**3 + 3*y**3

dx_f = diff(f, x)
dy_f = diff(f, y)

print(f"Nach x abgeleitet: {dx_f}\nNach y abgeleitet: {dy_f}")

x_wert = 2
y_wert = 1

print(f"Nach x abgeleitet an der Stelle {x_wert}: {dx_f.subs(x, x_wert)}\nNach y abgeleitet an der Stelle {y_wert}: {dy_f.subs(y, y_wert)}")

plot3d(f)
```





---
## Info

[[Mathe Basics für Data Scientists]]