#Note
27-05-2024
Tags: [[Mathe]] [[Grundlagen-&-Infinitesimalrechnung]] [[Python]] [[Data-Science]]

Dieses Thema ist sehr wichtig um bei größeren Berechnungen den Überblick zu behalten. Dabei gilt die Regel Klammer > Potenz > Punkt (Multiplikation / Division) > Strich (Addition / Subtraktion)

$$ 2\times \frac{(3+2)^2}{5}-4 = 2\times \frac{(\textbf{5})^2}{5}-4 = 2\times \frac{25}{5}-4 = 2\times 5-4 = 10-4 = 6 $$

Auch in Code z.B. in Python ist es demnach von Bedeutung Klammern zu verwenden, um die richtige Reihenfolge zu gewährleisten.

```python
value = 2 * ((3+2)**2 / 5) - 4

print(value) # Ausgabe 6.0
```




---
## Info

[[Mathe Basics für Data Scientists]]