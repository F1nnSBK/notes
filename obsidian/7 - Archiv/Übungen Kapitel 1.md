#Note
28-05-2024
Tags: [[Mathe]] [[Data-Science]] [[Python]] [[Grundlagen-&-Infinitesimalrechnung]]

## Übungen nach Kapitel 1

1. Ist der Wert 62,6738 rational oder irrational?

> Der Wert ist rational, da die Anzahl der Nachkommastellen nicht unendlich ist.  
2. Werten Sie den Ausruck $10^7\times10^{-5}$ aus.

>$10^7\times10^{-5}=10^{7-5}=10^2=100$  
3. Werten Sie den Ausruck $81^{1/2}$ aus.

>$81^{1/2}=\sqrt{81}=9$
4. Werten Sie den Ausruck $25^{2/3}$ aus.

>$25^{3/2}=\sqrt{25^3}=125$


```python
from sympy import *

print(round(10**7 * 10**(-5), 2))

print(81**(1/2), sqrt(81))

print(25**(3/2), sqrt(25**3))
```

5. Nehmen Sie an, dass keine Zahlungen erfolgt sind. Wie viel wäre ein Darlehen von 1000€ bei 5% Zinsen, die **monatlich** mit Zinseszins vergütet werden, nach drei Jahren Wert?


```python
kapital = 1000
zinssatz = 0.05
jahre = 3
monate = 12 * jahre

zahlungen = monate / jahre

kontostand = kapital * (1 + zinssatz/zahlungen)**(jahre * zahlungen)
print(f"{round(kontostand, 2)}€")
```

6. Nehmen Sie an, dass keine Zahlungen erfolgt sind. Wie viel wäre ein Darlehen von 1000€ bei 5% Zinsen nach drei Jahren Wert?


```python
from math import exp

kapital = 1000
zinssatz = 0.05
jahre = 3

kontostand = kapital * exp(zinssatz * jahre)

print(f"{round(kontostand, 2)}€")
```

7. Welchen Anstieg hat die Funktion $f(x)=3x^2+1$ bei $x=3$?


```python
from sympy import *

x = symbols('x')

f = 3 * x**2 + 1

anstieg = diff(f, x)

stelle = 3

print(anstieg.subs(x, stelle))
```

8. Wie groß ist die Fläche unter der Kurve der Funktion $f(x)=3x^2+1$ im Bereich zwischen 0 und 2?


```python
from sympy import *

x = symbols('x')

f = 3 * x**2 + 1

untere_grenze = 0
obere_grenze = 2

area = integrate(f, (x, untere_grenze, obere_grenze))

print(area)
```





---
## Info

[[Mathe Basics für Data Scientists]]