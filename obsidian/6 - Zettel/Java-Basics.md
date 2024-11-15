#Note
03.10.2024
Tags: [[Data-Science]] [[Java]] 

### Kommentierung in Java

Die Kommentierung erfolgt in Java nach einem bestimmten Schema.
```Java
/** (begin-comment delimiter)
* Kleine Zusammenfassung der Methode.
* Mehrere Absätze mit <p> Tag trennen.
* Beschreibung was die Methode macht.
* Kommentierung grundsätzlich im HTML-Format.
*
*@param --> Nennt und beschreibt die verwendeten Parameter.
*@return --> Beschreibt den Rückgabewert der Methode.
*@author --> Autor dees Programms.
*@version --> Version (1.0.0).
*@see --> Kann auf andere Methoden verweisen (Hilft beim Verständins)
*/ //(end-comment delimiter)
```

### Primitive Datentypen

integer --> int, long, byte, short  
floating point --> float, double  
character --> char  
logical --> boolean

Die Abstufungen der integer werden genutzt um Speicher zu sparen, da
sie von long --> byte immer kleiner werden und so weniger Ressourcen benötigen.

***int***  
+ Standardwert ohne Zuweisung ist 0.
+ Wird die Variable in einer Methode definiert muss ihr ein Wert zugewiesen werden bevor sie benutzt werden kann.
+ Nachkommastellen werden abgeschnitten-

***
***float***
+ Standardwert ohne Zuweisung ist 0.0
+ Nach 6 Nachkommastellen wird es ungenau --> "Big Decimal".
+ mit *f* designation kann die float definiert werden. (vgl. bei double mit D)

```Java
float dezimal = 3.41f;
```
double ist doppelt so groß wie float und nutzt 64 statt 32 bits als Speicher.
***
***char***
+ Steht für die Unicode der character. 
```Java
char c = 'a';
char c = 61; //61 in der Unicode Tabelle ist das Äquivalent zu klein a. 
```
#### Overflow

Trifft ein wenn die maximale Grenze des Typs überschritten wird 
und gibt infinity $\infty$.  

#### Underflow

Das Gegenteil zum Overflow trifft auf wenn die unterste Grenze
(Im negativen Bereich) unterschritten wird.  
In diesem Fall wird 0.0 ausgegeben.

### Objekt/Refernz - Typen

Zeigen auf Werte oder Objekte oder dem Wert *null*.
Ein *String*s ist beispielsweise ein Referenztyp.


### Variable deklarieren

Typ + Name der Variable (Identifier) + Semicolon

```Java
int a;
```  
Variable deklarieren mit Zuweisung:
Typ + Name + Zuweisungsoperator "=" + Semicolon

```Java
int a = 10;
String name = "Tobi";
```
Der Unterschied zwischen *char* und *String* ist die Anzahl der quotes "a" ist ein *String*
während 'a' ein *char* darstellt.

### Identifier  

Ein identifier ist ein Name für dessen Erstellung folgende Regeln gelten:
1. Startet mir einem Buchstabe, underscore oder dollar Zeichen
2. Darf kein Schlüsselwort sein.
3. Darf nicht *true*, *false*, oder *null* sein.  

### Arrays

Schema:
***typ[] identifier = new typ[länge];***
  
Um auf ein Wert im Array zuzugreifen wird der Name des Arrays und
der Index einer Variablen zugewiesen.

```Java
int[] numbers = new int[100];

numbers[0] = 1;

int firstElement = numbers[0];
```
Array Index startet bei 0 (vgl. Python & co.)  
Die Länge eines Array kann mit *numbers.length* abgerufen werden.

```Java
int lengthOfNumbersArray = numbers.length;
```

### Java Keywords (Schlüsselwörter)

Haben eine besondere Bedeutng und können nicht als Identifier benutzt werden. (vgl. Python (for, break, etc.))

```Java
public
static
class
main
new
instanceof
```  

### Operatoren in Java

Zuweisungsoperator =

#### Arithmetische Operatoren

(+) --> Addition und String Konkatenierung  
(-) --> Subtraktion  
(*) --> Multiplikation  
(/) --> Division
(%) --> Modulus (Rest einer Division)

#### Logische Operatoren

(&&) --> AND  
(||) --> OR  
(!) --> NOT  

#### Vergleichsoperatoren

(<) --> Kleiner als  
(>) --> Größer als  
(<=) --> Kleiner als oder gleich  
(>=) --> Größer als oder gleich  
(==) --> Gleich  
(!=) --> Nicht gleich zu

### Programmstruktur in Java

Das Grundelement ist die *Class*, welche mehrere *properties*, 
*methods* und andere *inner classes* enthalten kann.  
Damit eine Klasse ausführbar ist muss sie die *main* Methode besitzen.

```Java
public class SimpleAddition {

    public static void main(String[] args) {
        int a = 10;
        int b = 5;
        double c = a + b;
        System.out.println( a + " + " + b + " = " + c);
    }
}
```
Als Codeblock bezeichnet man den Code zwischen den geschweiften Klammern einer Klasse.

#### Common Signature

Die gewöhnliche Vorlage der main Methode ist:
```Java
public static void main(String[] args) {}
```

Bedeutung der Keywörter:
+ *public* --> access modifier, public ist global sichtbar
+ *static* --> auf die Methode kann direkt von der Klasse aus zugegriffen werden.
Es muss kein Objekt mit einer Referenz erstellt werden um den Codeblock auszuführen.
+ *void* --> diese Methode gibt kein Wert zurück.
+ *main* --> name der Methode, wird von der JVM gesucht.  

Der *args* Parameter steht für Werte die von der Methode empfangen werden.  
*args* ist ein *array of Strings*.

#### Verschiedene Arten der *main()* Methode

```Java
public static void main(String []args) {}
//
public static void main(String args[]) {}
// Argmuente können als varargs dargestellt werden
public static void main(String...args) {}
// strictfp wird für kompabilität zwischen Prozessoren verwendet
public strictfp static void main(String[] args) {}
```

*synchronized* und *final* sind weiter Schlüsselwörter.  
*final* kann genutzt werden um zu verhindern, dass das args array nochmal verändert wird.  

```Java
public static void main(final String[] args) {}
// mit allen Keywörtern
final static synchronized strictfp void main(final String[] args) { }
```

Es können auch mehrere *main()*  Methoden verwendet werden. In der *MANIFEST.MF* Datei kann
der JVM (Java Vitrual Machine) gesagt werden, welche *main()* Methode zuerst ausgeführt werden soll.  
Meisten genutzt wird das bei der Erstellung einer ausführbaren .jar Datei.


### Compiling und Ausführung

Java Runtime Environment JRE muss installiert sein.  

```Java
javac Programm.java
```
Erstellt eine *.class* Datei welche dann mit dem *java* Befehl
ausgeführt werden kann.

```Java
java Programm
```

### Java Kontrollstrukturen

#### Conditional Branches

***if / else / else if***  
```Java
if (Bedingung) {
    Codeblock
} else { codeblock }
```
***Ternary Operator***  
kann den if/else Block ersetzen und den code lesbarer gestalten.
```Java
System.out.println(count > 2 ? "Count ist größer als 2" : "Count is kleiner oder gleich 2");
```
***Switch***  
Ein switch kann genutzt werden wenn es mehrere Fälle auf die Abfrage einer Bedingung gibt.  
Damit kann das Überfluten des codes mit if/else statement verhindert werden.

```Java
int count = 3;
switch(count) {
case 0:
    System.out.println("Count ist 0");
    break;
case 1: 
    System.out.println("Count ist 1");
    break;
default:
    System.out.println("Count ist negativ oder größer als 1");
}
```

### Schleifen in Java

Schleifen führen ihren Codeblock mehrmals aus

#### for-Schleife
Eine for Schleife ist in Java durch folgendes Schema aufgebaut.  
```Java
for (Init, Bedingung, Schritt) {
    Anweisung;
    }
```
for-Schleifen können auch gelabeld werden.  
(xx: for ...)  
Mit *enhanced for-loops* kann über verschiedene Java Datenstrukturen iteriert werden. 
```Java
int[] intArr = { 0,1,2,3,4 }; 
for (int num : intArr) {
    System.out.println("Enhanced for-each loop: i = " + num);
    }
```

#### while-Schleife
```Java
while (Ausdruck) {
    Anweisung;
    }
```
#### do while-Schleife

Beide Schleifen rufen die Methode methodToRepeat() 50 mal auf.
Funktionen und Methoden meinen das Selbe. In Java spricht man von Methoden und in Python von Funktionen.

#### Break 

Mit *break* kann eine Schleife verlassen werden.

Bis 50 zählen aber stoppen, sobald die erste Zahl kommt, welche durch 12 ohne Rest teilbar ist, aber nicht die Zahl selbst ist.

```Java
for (int i = 1; i <= 50; i++) {
    System.out.println("Zahl " + i);
    if (i > 12) {
        if (i % 12 == 0) {
            break;
        }
    }
}
```

#### Continue

*continue* funktioniert ähnlich wie *break* nur mit dem Unterschied, dass der Rest der Schleife nicht abgebrochen
sondern übersprungen wird.  

---
## Info
[[Baeldung]]