#Note
28-05-2024
Tags: [[Zettel/obsidian/3 - Tags/Mathe]] [[Data-Science]] [[Python]] [[Grundlagen-&-Infinitesimalrechnung]]

Mit Sigma $\Sigma$ wird impliziert, dass die angegebenen Elemente ***summiert*** werden.

Möchte man die Zahlen 1 bis 8 durchlaufen und jede Zahl dabei mit 5 multiplizieren, würde die Formel lauten:    $\sum\limits_{i=1}^8 5i$

Das lässt sich natürlich auch in Python umsetzen.


```python
# Die Stopp integer wird nicht mitgezählt, weshalb sie eins über 8 gewähltwerden muss.
start = 1
stop = 8

summation = sum(5*i for i in range(start, stop + 1))
print(summation) # Ausgabe 180

# Demonstration der range() Funktion
numbers =[]

for i in range(start, stop + 1):
    numbers.append(i)

print(numbers) 
numbers = []

for j in range(start, stop):
    numbers.append(j)
    
print(numbers)    
```


Allgemein geschrieben sieht sie so aus: $\sum\limits_{i=1}^n x_i$   Dabei ist $n$ die *Anzahl der Elemente* und $x_i$ ein iteriertes Element. $i=1$ gibt die *Schrittweite* 1 an.



---
## Info

