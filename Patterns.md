## Grundsätzliches
Anit-Pattern ist nicht das Gegenteil von einer Design-Pattern. Anti-Patterns sind Patterns mit mehr negativen als positiven Effekten auf den Programmcode, oft sind aber die wenigen guten Effekte so wichtig das eine Anti-Pattern trotzdem verwendet wird.

### Name
- ist wichtig, damit man in einem einzigen Begriff ein Problem, dessen Lösung und Konsequenzen daraus ausdrücken kann

### Problemstellung
- Beschreibung des Problems mit dessen Umfeld → Bedingungen zur Verwendung
- z.B. Visitor Pattern: viele unterschiedliche, nicht verwandte Operationen auf einer Objektstruktur realisiert werden sollen, sich die Klassen der Objektstruktur nicht verändern, häufig neue Operationen auf der Objektstruktur integriert werden müssen oder ein Algorithmus über die Klassen einer Objektstruktur verteilt arbeitet, aber zentral verwaltet werden soll.

### Lösung
- Beschreibung einer bestimmten Lösung der Problemstellung
- Beschreibung ist allgemein damit die Lösung leicht angepasst werden kann

### Konsequenzen
- Liste von Eigenschaften der Lösung
- Manche Eigenschaften sind je nach Situation ein Vorteil oder Nachteil
- z.B. Visitor-Patterns besteht darin, unerwünschte dynamische Typabfragen und Typumwandlungen durch dynamisches Binden zu ersetzen

## Iterator
Ein Iterator, auch Cursor genannt, ermöglicht den sequentiellen Zugriff auf die Elemente eines Aggregats (das ist eine Sammlung von Elementen, beispielsweise eine Collection), ohne die innere Darstellung des Aggregats offenzulegen.

- Auf den Inhalt vom Aggregat zugreifen ohne die innere Struktur offen zu legen
- mehrere (gleichzeitige bzw. überlappende) Abarbeitungen der Elemente in einem Aggregat zu ermöglichen
- eine einheitliche Schnittstelle für die Abarbeitung verschiedener Aggregatstrukturen zu haben, das heißt, um polymorphe Iterationen zu unterstützen.

![[Pasted image 20220310212253.png]]
- ConcreteAggregate is Untertyp/Vererbung von Aggregate selbe gilt für Iterator und Concrete Iterator
- ConcreteAgg. kann ein Objekt von ConcreteIter. erzeugen
- ConcreteIter. referenziert ConcreteAgg.

- Algo zum Iterieren muss nicht im Iterator definiert sein ist ab besser wenn schon da oft auf private Attribute zugegriffen werden muss (Iterator als innere statische Klasse)
- Es kann gefährlich sein Aggregate während der Verarbeitung zu ändern da sie dann vom Iterator doppelt oder gar nicht verarbeitet werden → robuster Iterator (braucht viel Erfahrung)
-  oft ist es praktisch wenn ein Iterator von einem leeren Aggregat erzeugt werden kann

Iteratoren unterstützen 3 wichtige Eigenschaften:
- Unterstützen unterschiedliche Varianten der Abarbeitung (z.B. Bäume)
- vereinfachen Schnittstellen von Aggregate, da Zugriffsmöghlichkeiten über Iterator verfügbar sind und nicht vom Aggregate kommen müssen (z.B. remove im Iterator)
- Auf einem Aggregat können gleichzeitig mehrere Abarbeitungen stattfinden

#### interne vs externe Iteratoren
interne (Streams):
	- kontrollieren selbst wann die nächste Iteration erfolgt
	- enthälht die Schleife selbst → Merthoden wie next und hasNext sind nicht von außen zugänglich
	- sind oft einfacher zu verwenden, da die Logik (Schleife) nicht gebraucht wird außen
	- bieten einfache Möglichkeit zu parallelen Verarbeitung

externe (Iterator):
	- die Andwendung kontrolliert die Iteration
	- sind flexibler als interne
	- erschweren Parallele Verarbeitung
	- schwierig zu verwenden bei Objekten mit komplexen Beziehungen da die Struktur verloren geht; man kann nicht mehr sagen an welcher Stelle sich ein Blatt/Knoten im Baum befindet

## Factory Method (virtual constructor)
- Schnittstelle für die Objekterzeugung wobei **Unterklassen entscheiden von welcher Klasse Objekte erzeugt werden sollen**
- Die Erzeugung wird in die Unterklassen verschoben

```Java
public abstract class Document { ... }  

public class Text extends Document { ... }  
... // classes Picture, Video, ...  

public abstract class DocCreator {  
	protected abstract Document create();  
}  
public class TextCreator extends DocCreator {  
	protected Document create() { return new Text(); }  
}  
... // classes PictureCreator, VideoCreator, ...  

public class NewDocManager {  
	private DocCreator c = ...;  
	public void set(DocCreator c) { this.c = c; }  
	public Document newDoc() { return c.create(); }  
}
```

- Argumente von Interesse können an newDoc übergeben werden und über create and den noch nicht bekannten Konstruktor weitergeleitet werden
- Man kann Argumente an zentraler Stelle ablegen
- Am einfachsten ist es, für bestimmte Dokumente spezifische aber unveränderliche Argumente fix in die Methode create einzucodieren

Verwendbar wenn:
	- eine Klasse Objekte erzeugen soll, deren Klasse aber nicht kennt
	- eine Klasse möchte, dass ihre Unterklassen die Art der Objekte bestimmen, welche die Klasse erzeugt
	- Klassen Verantwortlichkeiten an eine von mehreren Unterklassen delegieren, und man das Wissen, an welche Unterklasse delegiert wird, lokal halten möchte
	- die Allokation und Freigabe von Objekten zentral in einer Klasse verwaltet werden soll

![[Pasted image 20220310221501.png]]
- Product ist meistens eine abstrakte Klasse (wie Dokument) und gemeinsamer Obertyp aller Objekte
- ConcreteProduct: Text,...
- abstrakte Klasse Createor: enthält die Factory-Method (meistens abstrakt)
- ConcreteCreator: implementiert die Factory-Method → die Methode erzeugt neue Concrete Products
- bieten Hooks (Anknüpfungspunkte) für Unterklassen → wird erwartet das sie überschrieben werden → umkehrung der Abhängigkeit; Oberklassen hängen von Unterklassen ab
- verknüpfen parallele Klassenhierachien; die Createor mit der Product Hierachie → Document, Text; DocCreateor, TextCreator

#### lazy initialization
```Java
public abstract class Creator {  
	private Product product = null;  
	protected abstract Product createProduct();  
	public Product getProduct() {  
		if (product == null)  
			product = createProduct();  
		return product;  
	}  
}
```

## Prototype
- Art eines neu zu erzeugenden Objekts durch ein Prototyp-Objekt zu spezifizieren
- Neue Objekte werdern durch Copy des Objekts erzeugt

Anwendbar wenn:
	- System soll unabhängig sein davon: wie sein Prod. erzeugt, zusammengesetzt, und dargestellt werden soll
	- Klasse der Objekte erst zur Laufzeit bekannt ist
	- Hierachie von Creator/Product Klassen vermieden werden soll
	- jedes Objekt einer Klasse nur wenige Unterschiede haben kann

![[Pasted image 20220312131148.png]]

- clone wird in Prototype aufgerufen oder im ConcreteProto durch dynamisches Binden
- verstecken konkrete Produktklassen → Client braucht deshalb weniger Klassen zu kennen
- Protos können zur Laufzeit dazugegeben und wegnommen werden → Klassenstruktur darf in der Regel nicht geändert werden
- Spezifikation neuer Objekte durch änderbare Werte → z.B. Objektkomp. statt Definition neuer Klassen
- vermeiden große Anzahl von Unterklassen
- dynamische Konfiguration von Programmen
- clone schwer zu implementieren in zyklischen Strukturen (z.B. Double linked List) → "tiefe" Kopien muss clone selbst implementiert werden

## Singletone
Anwendbar wenn:
	- es soll genau ein Objekt existieren und Global verwendbar sein
	- die Klasse durch Vererbung erweiterbar sein soll, und Anwender die erweiterte Klasse ohne Änderungen verwenden können sollen

- hat eine statische Methode instance(); private/protected Konstruktor
- kontorllierter Zugriff auf das einzige Objekt
- vermeiden Globale Variablen
- unterstützen Vererbung
- verhindern Instanzen außerhalb der Kontrolle der Klasse
- man kann auch mehrere Instanzen erlauben
- felxibler als statische Methoden weil die nicht dynamische Binden erlauben

```Java
public class Singleton {  
	private static Singleton singleton;  
	protected Singleton() { // kein Aufruf von außen  
		singleton = null;  
	}  
	
	public static Singleton instance() {  
		if (singleton == null)  
			singleton = new Singleton();  
		return singleton;  
	}  
}
```
```Java
public class Singleton {  
	private static Singleton singleton = null;  
	protected Singleton() { ... }  
	public static Singleton instance(int kind) {  
		if (singleton == null)  
			switch (kind) {  
				case 1: singleton = new SingletonA(); break  
				case 2: singleton = new SingletonB(); break  
				default: singleton = new Singleton();  
			}  
		return singleton;  
	}  
}  
public class SingletonA extends Singleton {  
	protected SingletonA() { ... }  
}  
public class SingletonB extends Singleton {  
	protected SingletonB() { ... }  
}
```
Einfach Lösung aber nicht die Beste; nach erzeugung des ersten Objekts hat Kind keine bedeutung mehr

```Java
public class Singleton {  
	protected static Singleton singleton = null;  
	
	protected Singleton() { ... }  
	
	public static Singleton instance() {  
		if(singleton==null) singleton = new Singleton();  
	return singleton;  
	}  
}  
public class SingletonA extends Singleton {  
	protected SingletonA() { ... }  
	
	public static Singleton instance() {  
		if(singleton==null) singleton = new SingletonA();  
		return singleton;  
	}  
}
```

[[Patterns Struktur & Verhalten|Next]]
