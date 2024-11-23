#Note
2024-10-07
Tags: [[Java]] [[Programmierung]] [[OOP]]

### Klassen, Objekte und Methoden

Eine Klasse wird durch ihr Member aufgebaut. Hinter dem Begriff "Member"
verstecken sich die wesentlichen Bestandteile einer Klasse. 

+ Variablen (auch Attribute genannt)
+ Konstruktoren
+ Methoden  

Attribute enthalten Informationen über des Zustands der Objekte, die sie beschreiben.
Methoden enthalten den Code einer Klasse und beschreiben somit ihr Verhalten.
Sie können den Zustand und die Werte von Attributen verändern. Konstruktoren sind 
spezielle Methoden, welche zur Objekterzeugung aufgerufen werden.

```Java
[Modifizierer] class Klassenname [extends Basisklasse] [implements Interface-Liste] {

//Attribute
private int zahl;
private String name;

//Konstruktoren
public Klassenname() {
    this.zahl = 100;
    this.name = "Tom";
    };

//Methoden
public void printHelloWorld() {
    System.out.println("Hello World!");
    };

public static void main(String[] args) {
    Klassenname meineNeueKlasse = new Klassenname();
    meineNeueKlasse.printHelloWorld();
    };
}
```  
Es ist darauf zu achten, dass pro Datei nur eine Klasse den Modifizierer
*public* besitzt. Am besten ist es, wenn man pro Datei nur eine Klasse verwendet. Der Dateiname
muss der Name der Klasse entsprechen.

#### Objekterzeugung mit *new*

Mit dem *new* Operator lässt sich ein neues Objekt einer Klasse erzeugen.
Dieser Prozess wird Instanziierung genannt und die Objekte werden folglich auch als Instanzen
der Klasse bezeichnet.

#### Identität


### Konkrete Klassen, interfaces und abstrakte Klassen

#### Konkrete Klasse
In konkreten Klassen sind alle Methoden implementiert und von der Klasse kann mit dem *new*
Operator eine neue Instanz erstellt werden.

Aufbau einer konkreten Klasse:
```Java
public class Auto() {
    public String bremsen() {
        return "Auto bremst";
    }
    
    public String hupen() {
        return "Auto hupt";
    }
    
}

Auto neuesAuto = new Auto();
```

Interfaces und abstrakte Klassen können in konkrete Klassen eingebaut werden.
```Java
public class SchönesAuto extend Fahrzeug implements Fahrbar {
    public String hupen() {
        return "Auto hupt";
    }
}
```

#### Interfaces

Können als Vorlagen von Klassen verwendet werden und enthalten uninplementierte Methoden. 
```Java
interface Fahrbar {
    void bremsen();
    void hupen();
}
```

#### Abstrakte Klassen

Eine abstrakte Klasse kann unimplementierte und implementierte Methoden enthalten.
```Java
public abstract class Fahrzeug {
    public abstract String hupen();
    
    public String fahren() {
        return "Auto Fährt";
    }
}
```

### Access Modifiers

Access Modifier regeln in Java die Zugriffsrechte auf Methoden, Variablen, Konstruktoren oder Klassen.  
Eine Top-Level Klasse kann nur default oder public als Access Modifier besitzen. 
Auf dem Member-Level, also in einer Top-Level Klasse können alle verwendet werden.

#### Vergleich der Modifier Public, Protected, Default, und Private

| Modifier  | Class | Package | Subclass | World |
|:---------:|:-----:|:-------:|:--------:|:-----:|
|  public   |   Y   |    Y    |    Y     |   Y   |
| protected |   Y   |    Y    |    Y     |   N   |
|  default  |   Y   |    Y    |    N     |   N   |
|  private  |   Y   |    N    |    N     |   N   |

Der default access modifier wird auch gesetzt, wenn kein access modifier definiert wird.  
Default und package-private meinen das Gleiche.

#### Standard Reihenfolge der Modifiers

Annotation --> Access Modifier --> andere Keywords  

|  **Allgemein**  |   **Klassen**   |  **Methoden**   |
|:---------------:|:---------------:|:---------------:|
|   Annotation    |   Annotation    |   Annotation    |
| Access Modifier | Access Modifier | Access Modifier |
|    abstract     |    abstract     |    abstract     |
|     static      |     static      |     static      |
|      final      |      final      |      final      |
|    transient    |    strictfp     |  synchronized   |
|    volatile     |                 |     native      |
|                 |                 |    strictfp     |

### Konstruktoren

Konstruktoren dienen zur Initialisierung von Objekten. Ohne ihn sind haben die 
Objekte in der Regel einen *null* Wert und sorgen für Fehler.  
(NullPointerException)  

#### No-Argument Konstruktor

Die Objekte werden von einem Standard Konstruktor des Compilers auf ihren Default Wert
(für Objekte = *null*) gesetzt.
Wir können unseren eigenen No-Argument Konstruktor schreiben und so den Objekten
bestimmte Werte zuweisen. Der Konstruktor ist eine Methode.
```Java
class Handy {
    public Handy() {
        this.akku = "";
        this.created = LocalDateTime.now();
        this.model = "";
    }
}
```

#### Parameterized Konstruktor

Das ist ein Konstruktor der Argumente akzeptiert. Dennoch wird oft davor ein
No-Argument Konstruktor gesetzt, falls die Argumente leer oder nicht vorhanden sind, so
können Fehlermeldung vermieden werden.
```Java
class Handy {
    public Handy() { ... }
    public Handy(String akkuladung, LocalDateTime hergestellt, String modellbezeichnung) {
        this.akku = akkuladung;
        this.hergestellt = hergestellt;
        this.modell = modellbezeichnung;
    }
}

LocalDateTime hergestellt = LocalDateTime.of(2018, Month.JUNE, 29, 06, 30, 00);
Handy handy = new Handy("80%", hergestellt, "Iphone 15");
System.out.println(handy);

```

#### Copy Konstruktor

Ein copy Konstruktor kann sich auf einen anderen Konstruktor beziehen und neue Werte für die Objekte
vergeben.
```Java
public Handy(Handy other) {
    this.akku = other.akku; /* Übernimmt den Wert des anderen (other) Konstruktors */
    this.hergestellt = LocalDateTime.now();
    this.modell = "Iphone 14";
} 
```

#### Chained Konstruktor

Ein verketteter Konstruktor verhält sich ähnlich wie ein copy Konstruktor, mit dem
Unterschied, dass auch nur ein Wert von einem anderen Konstruktor übernommen werden kann.
```Java
public Handy(String akkuladung, LocalDateTime hergestellt, String modellbezeichnung) {
    this.akku = akkuladung;
    this.hergestellt = hergestellt;
    this.modell = modellbezeichnung;
}
public Handy(String akkuladung) {
    this(name, LocalDateTime.now(), "");
}
```
Mit dem *this* keyword wird der andere Konstruktor aufgerufen.
Achtung bei einem Konstruktor einer *superclass* muss das *super* keyword verwendet werden.

#### Value Types

Value Objects können nach ihrer Initialisierung nicht verändert und nur gelesen werden. 
Bei ihrer Erstellung wird das *final* keyword verwendet.

```Java
class HandyImBesitz {
    final Handy handy;
    final LocalDateTime date;
    final String modell;
    
    public HandyImBesitz(Handy akku, LocalDateTime date, String modell) {
        this.handy = akku;
        this.date = date;
        this.modell = modell;
    }
}
```
### Objekte in Java erstellen

#### Deklaration und Initialisierung

Deklaration bezieht sich auf die Erstellung und Benennung einer Variablen:
`int id;`  
Initialisierung bedeutet, dieser Variablen einen Wert zuzuweisen:
`int id = 1;`  

#### Objekte vs. Primitive Typen

##### Primitive Typen und Referenztypen

Die acht Arten von Variablen in Java werden als primitive Typen bezeichnet.
Die Variablen Speicher ihre Werte direkt, Referenzen hingegen enthalten nicht den Wert des Objekts, auf das sie verweisen.
```Java
public void initializedWithNew() {
    User user = new User();
}
```

##### Objekte

Schritte zur Erstellung eines Objekts:  
*new* initialisiert Objekt -->  Konstruktor wird aufgerufen --> Initialisierung in den Speicher  
Das *new* Keyword initialisiert ein Objekt in den Speicher durch einen Konstruktor.
Ein Konstruktor wird verwendet um eine Instanzvariable mit den Eigenschaften eines erstellen Objektes zu repräsentieren.  
Eine Klasse kann viele Konstruktoren haben, solange ihre Parameterlisten unterschiedlich sind.  
```Java
public User(String name, int id) {
    this.name = name;
    this.id = id;
}

User user = new User("Finn", 1);
```

### Initialisierer

#### Instanzinitialisierer

Können verwendet werden um Instanzvariablen zu initialisieren.
```Java
{
    id = 0;
}
```

#### Statischer Initialisierungsblock

```Java
privat static String forum;
static {
    forum = "Java";
}
```

#### Reihenfolge der Initialisierung

*static* Variablen und Initialisierer --> Instanzvariablen- und Initialisierer --> Konstruktoren  

### Object-Life-Cycle (OLC)

Objekte, welche nicht genutzt werden, werden von dem Java Garbage Collector beseitigt.  
Alle Objekte werden in Javas heap memory gespeichert (Dynamischer Speicher).  
Ein Objekt ist *out of reach*, wenn es entweder keine Referenz mehr hat, mit welcher auf es gezeigt wird oder wenn alle Referenzen *out of scope* sind.  
Erstellung mit *new* Keyword --> Objekt lebt und versorgt uns mit Methoden und Fields (Attributen) --> Garbage Collector zerstört es

### Reflexion, Klonen und Serialisierung

Reflexion ist eine Methode zur Objekterstellung zur Laufzeit.  
```Java
@Test
public void whenInitializedWithReflection_thenInstanceIsNotNull() 
  throws Exception {
    User user = User.class.getConstructor(String.class, int.class)
      .newInstance("Alice", 2);
 
    assertThat(user).isNotNull();
}
```
Hier wird der Konstruktor der user Klasse zur Laufzeit aufgerufen, um ein neues user Objekt zu erstellen.  

Klonen wird genutzt um eine exakte Kopie eines Objekts zu erstellen.
```Java
public class User implements Cloneable { //... }

@Test
public void whenCopiedWithClone_thenExactMatchIsCreated() 
  throws CloneNotSupportedException {
    User user = new User("Alice", 3);
    User clonedUser = (User) user.clone();
 
    assertThat(clonedUser).isEqualTo(user);
}
```
Hier wird ein clonedUser Objekt erstellt, welches die exakt gleichen Eigenschaften wie das user Objekt hat.

Serialisierung wird verwendet um ein Objekt in einen Bytestream umzuwandeln, so kann es beispielsweise über das Netzwerk versendet werden.  
```Java
import java.io.*;

public class User implements Serializable { 
    private String name;
    private int alter;

    public User(String name, int alter) {
        this.name = name;
        this.alter = alter;
    }

    // Implementiere Setter und Getter sowie andere Methoden
}

@Test
public void whenSerializedAndDeserialized_thenObjectIsSame() 
  throws IOException, ClassNotFoundException {
    User user = new User("Alice", 3);

    // Serialisierung
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    ObjectOutputStream oos = new ObjectOutputStream(baos);
    oos.writeObject(user);
    oos.close();

    // Deserialisierung
    ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());
    ObjectInputStream ois = new ObjectInputStream(bais);
    User deserializedUser = (User) ois.readObject();
    ois.close();

    assertThat(deserializedUser).isEqualToComparingFieldByField(user);
}
```

---
## Info

[[Baeldung]]