
2025-01-09  
Tags: [[DHBW]] [[Grundlagen Lineare Algebra und Analytische Geometrie]]  

---
## Theorie
### Definitionen  
#### Vektor
- **Spaltenvektor**  
  $$\vec{v} = \begin{pmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{pmatrix}$$
- **Zeilenvektor**  
  $$\vec{v}^T = (v_1, v_2, \ldots, v_n)$$
  Ein Zeilenvektor ist ein *transponierter* Spaltenvektor.

- **Skalar**:  
  Eine Zahl, die kein Vektor ist, wird als **Skalar** bezeichnet.

#### Beträge
+ Die Länge eines Vektors ist dessen Betrag. $|\vec{v}|$
+ Ein Betrag ist immer positiv.
  $$|\vec{a}|=\sqrt{\vec{a}^T\cdot\vec{a}}=\sqrt{a_1^2+a_2^2+ ... + a_n^2}=\sqrt{\sum_{i=1}^{n}{a_i^2}}$$
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