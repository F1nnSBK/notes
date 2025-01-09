
2025-01-09  
Tags: [[DHBW]] [[Grundlagen Lineare Algebra und Analytische Geometrie]]  [[Mathe]]

---
## Theorie
### Definitionen  
#### Vektor
- **Spaltenvektor**  
  $$\vec{v} = \begin{pmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{pmatrix}$$
- **Zeilenvektor**  
  $$\vec{v}^T = (v_1, v_2, \ldots, v_n)$$
  Ein Zeilenvektor ist ein *transponierter* Spaltenvektor.  
- **Nullvektor**
  $$\vec{0}=\begin{pmatrix}
    0\\
    0\\
    0\\
  \end{pmatrix}$$
  Alle Elemente sind 0.
- **Einheitsvektor**
  $$\vec{e}=\begin{pmatrix}
    0\\
    0\\
    1\\
  \end{pmatrix}$$
  Alle Elemente bis auf eines sind 0.

- **Skalar**:  
  Eine Zahl, die kein Vektor ist, wird als **Skalar** bezeichnet.

#### Beträge
+ Die Länge eines Vektors ist dessen Betrag. $|\vec{v}|$
+ Ein Betrag ist immer positiv.
  $$|\vec{a}|=\sqrt{\vec{a}^T\cdot\vec{a}}=\sqrt{a_1^2+a_2^2+ ... + a_n^2}=\sqrt{\sum_{i=1}^{n}{a_i^2}}$$

#### Normen
+ Eine **Norm** ist eine Funktion, die einem Vektor eine Zahl zuordnet, die als *Größe* oder *Länge* des Vektors interpretiert werden kann. $p(\vec{x})=||\vec{x}||$
+ Bedingungen von Normen:
  1. **Dreiecksungleichung**: $||\vec{x}+\vec{y}||\leq ||\vec{x}|| + ||\vec{y}||$ $\space\forall \vec{x},\vec{y}\in \R^n$
  2. **absolute Homogenität**: $||s\vec{x}||=|s|||\vec{x}||$ $\space\forall\vec{x}\in X und \space\forall s\in \R$
  3. **Definitheit**: $\forall \vec{x}\in \R^n$ gilt, wenn $||\vec{x}||= 0$ dann $\vec{x}=\vec{0}$  

+ Wichtige Normen
  + L1-Norm 
  $$||\vec{x}\|_1 = \sum_{i=1}^n |x_i| \quad \text{(Taxi-, Manhattan-Distanz)}$$
  + L2-Norm (euklidische Norm)
  $$||\vec{x}\|_2 = \|\vec{x}\| = \sqrt{\sum_{i=1}^n |x_i|^2}$$
  + p-Norm
  $$||\vec{x}\|_p = \left( \sum_{i=1}^n |x_i|^p \right)^{\frac{1}{p}}$$
  + $\infty$-Norm / Maximum-Norm
  $$||\vec{x}\|_\infty = \max(|x_1|, \dots, |x_n|) \quad \text{(Größter Wert der Elemente eines Vektors)}$$

#### Linearkombination, Lineare Hülle
+ Mulitplikative Kombination mehrere Vektoren $\vec{a_1},\vec{a_2}, ... ,\vec{a_n}$
$$\vec{b}=\sum_{i=1}^{n}{(\lambda_i \cdot \vec{a_i})}=\lambda_1 \cdot \vec{a_1}+\lambda_2 \cdot \vec{a_2} + ... + \lambda_n \cdot \vec{a_n}$$
mit $\lambda_i \in \R$ (Multiplikator)
+ $\vec{b}$ ist dann eine **Linearkombination** von $\vec{a_1},\vec{a_2}, ... ,\vec{a_n}$
+ Die lineare Hülle ist die Menge aller Linearkombinationen von $\vec{a_1},\vec{a_2}, ... ,\vec{a_n}$
##### Kollinearität
+ Vektoren sind parallel (kollinear), wenn sich Vielfache voneinander sind.
+ Es existiert ein $k\in\R$ sodass $\vec{a}=k\vec{b}$
##### Orthogonoalität
+ Vektoren sind senkrecht (orthogonal) zueinander, wenn das Skalarprodukt $\vec{a}^T\cdot\vec{b}=0$ ergibt.

#### Lineare Unabhängigkeit
+ Vektoren sind **linear unabhängig** wenn $\lambda_1\vec{a}_1 + \lambda_2\vec{a}_2+ ... +\lambda_n\vec{a}_n = \vec{0}$ nur erfüllt ist, wenn alle $\lambda_i=0.$
+ Vektoren sind **linear abhängig** wenn außer $\lambda_i=0$ noch mindestens eine weitere Lösung vorliegt.
  + Ist in der Reihe ein **Nullvektor** enthalten, sind sie immer linear abhängig.

### Geraden und Ebenen
+ Ein Ortsvektor führt vom Ursprung zu einem Punkt in den Raum.
#### Geraden
+ Eine Gerade ist eine Linie im Raum
+ Hat keinen Anfang und kein Ende
##### Parameterdarstelleung
$$\vec{x}=\vec{p} + \lambda \cdot \vec{q},\space\lambda\in\R$$
+ $\vec{p}$ ist der Stützvektor (Ortsvektor)
+ $\vec{q}$ Richtungsvektor
+ $\lambda$ ist die Schrittweite entlang des Richtungsvektor von dem Stützvektor aus
+ $\vec{x}$ ist ein beliebiger Punkt auf der Geraden

#### Ebenen
+ Wird von 2 Richtungsvektoren aufgespannt.
##### Parameterdarstelleung
$$\vec{x}=\vec{p} + \lambda \cdot \vec{q} + \mu \cdot \vec{r},\space\space\space\space\lambda,\mu\in\R$$
+ $\vec{p}$ ist der Stützvektor (Ortsvektor, Aufpunkt)
+ $\vec{q}$ Richtungsvektor 1
+ $\vec{r}$ Richtungsvektor 2, darf nicht kollinear zu dem anderen Richtungsvektor sein
+ $\lambda,\mu$ sind die Schrittweiten entlang der Richtungsvektoren von dem Stützvektor aus
+ $\vec{x}$ ist ein beliebiger Punkt auf der Ebene
##### Koordinatengleichung
+ Aus der Parameterdarstellung kann ein LGS erstellte werden, das 3 Zeilen hat. Wenn man dieses löst, erhält man die Koordinatengleichung.
$$n_0+n_1x_1+n_2x_2+n_3x_3 = 0, \space\space\space n_i\in\R$$
+ $n_0$ legt die Höhe der Ebene fest
##### Normalenvektor
+ Aus der Koordinatengleichung kann der **Normalenvektor** abgelesen werden.
$$\vec{n}=(n_1,n_2,n_3)$$
##### Kreuzprodukt
+ mit dem Kreuzprodukt $\vec{q}\times\vec{r}$ kann schnell der Normalenvektor gebildet werden.
$$
\vec{a} \times \vec{b} =
\begin{pmatrix}
a_1 \\
a_2 \\
a_3
\end{pmatrix}
\times
\begin{pmatrix}
b_1 \\
b_2 \\
b_3
\end{pmatrix}
=
\begin{pmatrix}
a_2 b_3 - a_3 b_2 \\
a_3 b_1 - a_1 b_3 \\
a_1 b_2 - a_2 b_1
\end{pmatrix}
= \vec{n}
$$
+ Um $n_0$ zu berechnen, wird ein Punkt der Ebene (Stützvektor) in die Koordinatenform eingesetzt für $x_1, x_2 \space und \space x_3$.
#### Abstände
##### Ursprung-Ebene
+ $\frac{|n_0|}{|\vec{n}|}$ ist der Abstand der Ebene zum Ursprung.
##### Punkt-Ebene
+ Punkt $\vec{p}$ in die Koordinatenform einsetzen.
$$\frac{|\vec{p}^T\vec{n}+n_0|}{|\vec{n}|}$$
---
### Operationen  

#### Skalarprodukt
- Ein Skalar $c \in \mathbb{R}$ multipliziert mit einem Vektor $\vec{v}$ ergibt:
	$$c \cdot \vec{v} = c \cdot \begin{pmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{pmatrix}= \begin{pmatrix} c \cdot v_1 \\ c \cdot v_2 \\ \vdots \\ c \cdot v_n \end{pmatrix}$$
  + Ein Zeilenvektor $\vec{a}^T$ multipliziert mit einem Vektor $\vec{b}$:
	$$\vec{a}^T \cdot \vec{b} = (a_1,a_2,...,a_n) \cdot \begin{pmatrix} b_1 \\ b_2 \\ \vdots \\ b_n \end{pmatrix}= a_1 \cdot b_1+a_2 \cdot b_2 \space +\space  ... \space +\space a_n \cdot b_n$$
	Es gilt stets, Zeilenvektor mal Spaltenvektor!

#### Addition / Subtraktion von Vektoren
+ mit $\vec{a}$ und $\vec{b}$ : 
	$$\vec{a} \pm \vec{b} = \begin{pmatrix} a_1\\a_2\\\vdots\\a_n \end{pmatrix} \pm \begin{pmatrix} b_1\\b_2\\\vdots\\b_n \end{pmatrix} = \begin{pmatrix} a_1 \pm b_1\\a_2 \pm b_2\\\vdots\\a_n \pm b_n \end{pmatrix}$$
	Die Vektoren müssen zur Addition/Subtraktion das gleiche Format aufweisen

####
---
## Anwendung  
- **Multiplikation mit \( c = 2 \) und $\vec{v} = \begin{pmatrix} 1 \\ 3 \\ -2 \end{pmatrix}$**:  
  $$
   2 \cdot \vec{v} = 2 \cdot  \begin{pmatrix} 1 \\ 3 \\ -2 \end{pmatrix} = \begin{pmatrix} 2 \\ 6 \\ -4 \end{pmatrix}$$
  
- **Transponieren eines Vektors**:  
   $$\vec{v} = \begin{pmatrix} 1 \\ 2 \end{pmatrix}
   \longrightarrow
   \vec{v}^T = (1, 2)$$