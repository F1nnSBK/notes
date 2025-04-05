#Note
27-05-2024
Tags: [[Zettel/obsidian/3 - Tags/Mathe]] [[Grundlagen-&-Infinitesimalrechnung]] [[Python]] [[Data-Science]]

## Eulersche Zahl $e$

Die Zahl $e$ kann in Python mit der exp() Funktion als Basis genutzt werden.kacke


```python
from sympy import *

value = exp(1)
print(value, value.evalf())

# Grafische Kontrolle
x = symbols('x')

f = E**x
g = E
plot(f, g, (x, -2, 2))
```

## Logarithmus $\log$

Der Logarithmus ist die Umkehrfunktion zur Exponentialfunktion.

Sehr häufig wird er im Zusammenhand mit $e$ verwendet. Dabei ist der *natürliche Logarithmus* ($\ln$) die Umkehrfunktion zu $e^x$. In Python gibt es dafür die Funktion log(), welche zwei Argumente - Basis und Wert - erwartet, aber auch mit einem funktoniert.
Wird nur ein Argument gegeben wird automatisch die Basis $e$ von Python ausgewählt.


```python
from sympy import *

value = log(1)

print(value)

# Grafische Kontrolle
x = symbols('x')

f = log(x)
plot(f)
```

## Grenzwerte $\underset{x\rightarrow \infty}{\lim}$

Um die Grenzwerte einer Funktion zu untersuchen können wir die Funktion limit() in Python nutzen. Das $\infty$ Zeichen wird dabei als oo geschrieben. Als Argumente werden die Funktion, die Variable, und der angestrebte Wert benötigt.

Mit $\underset{x\rightarrow \infty}{\lim}\  \frac{1}{x}=0$ und $\ \ \underset{n\rightarrow \infty}{\lim}\ (1+\frac{1}{n})^n=e$


```python
from sympy import *

x = symbols('x')

f = 1 / x
grenzwert = limit(f, x, oo)
print(grenzwert)

# Herleitung von e
n = symbols('n')

g = (1 + (1/n))**n
grenze = limit(g, n, oo)
print(grenze)
```

## Wurzeln $\sqrt{x}$

Um die Wurzel einer Zahl zu ziehen, verwenden wir die sqrt() Funktion.


```python
from sympy import *

x = symbols('x')

value = sqrt(16)
print(value)
```





---
## Info

[[Mathe Basics für Data Scientists]]