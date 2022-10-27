```Java
x = T.getClass(); //Abfrage der Klasse

if(t instanceof T)... // Dynamische Untertypabfrage
```
- dynamische Typumwandlung gefährlich da statische Typsicherheit umgangen wird → so selten wie möglich verwenden und wenn dann nur wenn absolut nötig
- dynmische Umwandlung nicht für elementare Typen zu Referenztypen in Java unterstützt

homogene Übersetzung einer generischen Klasse oder Methode in eine Klasse oder Methode ohne Generizität:
```Java
public interface Collection<A> {  
	void add(A elem);  
	Iterator<A> iterator();  
}  
public interface Iterator<A> {  
	A next();  
	boolean hasNext();  
}

wird zu 

public interface Collection {  
	void add(Object elem);  
	Iterator iterator();  
}  
public interface Iterator {  
	Object next();  
	boolean hasNext();  
}
```

### Homogene Generizität
- jede generische und nicht-generische Klasse wird in genau eine Klasse mit JVM-Code übersetzt
- Jeder gebundene Typparameter wird im übersetzten Code durch die erste Schranke des Typparameters ersetzt, jeder ungebundene Typparameter durch $Object$
- Typparameter als Methodenrückgabe oder -Parameter werden dynamisch zur Laufzeit in den jeweiigen Typ umgewandelt
- Mischen von Generics mit Non-Generics möglich

### Heterogene Generizität
- für jede Verwendung einer generischen Klasse oder Methode mit anderen Typparametern eigener übersetzter Code erzeugt
- erzeugt so viele nicht-generische Klassen (Methoden) wie nötig
- int, char und boolean ohne Einbußen an Laufzeiteffizienz als Ersatz für Typparameter geeignet
- keine RawTypes verwendbar
- Mischen von Generics und RawTypes nicht möglich
- Nachteil: komplizierte Fehlermeldungen

### Dynamisches Binden
```Java
if (x instanceof T1)  
	doSomethingOfTypeT1((T1)x);  
else if (x instanceof T2)  
	doSomethingOfTypeT2((T2)x);  
...  
else  
	doSomethingOfAnyType(x);
```
- Problem: bei allgemeinen Typen kann nicht alles abgedeckt werden, deklarierte Klassen können eventuell nicht erweitert werden weil $final$ und Source Code nicht bekannt

```Java
public abstract class Point {  
	public final boolean equal(Point that)  
		{ return this.getClass()==that.getClass() && uncheckedEq(that); }  
	
	protected abstract boolean uncheckedEq(Point p);  
}  

public class Point2D extends Point {  
	private int x, y;  
	protected boolean uncheckedEq(Point p)  
	{ 
		return x==((Point2D)p).x && y==((Point2D)p).y; 
	}  
}  

public class Point3D extends Point {  
	private int x, y, z;  
	protected boolean uncheckedEq(Point p)  
	{ 
		Point3D that = (Point3D)p;  
		return x==that.x && y==that.y && z==that.z; 
	}  
}
```

### Typumwandlung
- Upcast = easy peasy
- Down-Cast nach dynam. Typabfr.: fehlernanfällig, alternative If-Zweige
- Down-Cast wie Generizität: gleichförmige Ersetzung der Typparameter und  
	keine impliziten Untertypbeziehungen  
	wobei Intuition oft irreführend:  
	$List<String> !≤ List<Integer>$  
	impliziert List  !≤ List nach Ersetzung

new A(...) oder new A[...]: A darf kein Typparameter sein in Java

### Raw Types
Klassen ohne Generics z.B. List und Iterator, Typen wo die Generics "gelöscht" werden nennt man Type-Erasure
```Java
public class List<A> implements Collection<A> {  
	...  
	public boolean equals(Object o) {  
		if (o == null || o.getClass() != List.class)  
			return false;  
	Iterator<A> xi = this.iterator();  
	Iterator yi = ((List)o).iterator();  
	
	while (xi.hasNext() && yi.hasNext()) {  
		A x = xi.next();  
		Object y = yi.next();  
		if (!(x == null ? y == null : x.equals(y)))  
			return false;  
		}  
		return !(xi.hasNext() || yi.hasNext());  
	}  
}
```
Man beachte Non Generic Mehod in Generic Klasse und Generics verwendet in der Methode

[[Kovariante Probleme]]
