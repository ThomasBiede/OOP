### Universeller Poly.
#### Poly. durch Untertypbez.
unvorhersehbare Wiederverwendung, kann Clients von lokalen Codeänderungen abschotten, nicht immer verwendbar (kovariante Probleme)

#### param. Poly (Generizität)
kaum Einschränkungen (auch für kovariante Probleme), keine Ersetzbarkeit, Parameteränderung wirkt sich auf Clients aus

##### Generizität und Untertypbez. ergänzen einander

#### Gebunder Typparam.
```Java
public interface Scalable {  
	void scale(double factor);  
}  
public class Scene<T extends Scalable> implements Iterable<T> {  
	public void addSceneElement(T e) { ... }  
	public Iterator<T> iterator() { ... }  
	public void scaleAll(double factor) {  
		for (T e : this)  
			e.scale(factor);  
		}  
	...  
}
```

#### Rekusiver Typparam.
```Java
public interface Comparable<A> {  
	int compareTo(A that); // res. < 0 if this < that  
						   // res. == 0 if this == that  
						   // res. > 0 if this > that  
}  
class MyInteger implements Comparable<MyInteger> {  
	private int value;  
	public MyInteger(int v) { value = v; }  
	public int intValue() { return value; }  
	public int compareTo(MyInteger that) {  
		return this.value - that.value;  
	}  
}
```

Rekursiv und Gebunden kann auch kombiniert werden

Generizität $\to!$  Ersetzbarkeit
$X<A>$ kein Untertyp von $X<B>$
daher $List<Student>$ kein Untertyp von $List<Person>$
aber MyInteger Untertyp von $Comparable<MyInteger>$

```Java
void drawAll(List<Polygon> p) { ... }  
	Lesen und Schreiben des Inhalts von p  
	aber kein Argument vom Typ List<Square> oder List<Object>  

void drawAll(List<? extends Polygon> p) { ... }  
	Aufruf mit Argument vom Typ List<Square> erlaubt  
	aber nur Lesen des Inhalts von p (kein Schreiben) nur wo Typen kovariant  

void addPolygon(List<? super Polygon> to) { ... }  
	Aufruf mit Argument vom Typ List<Object> erlaubt  
	aber nur Schreiben des Inhalts von to (kein Lesen) nur wo Typen kontravariant
```

extends und super invertieren sich mit jeder zusätzlichen Ebene
extends $\to$ super auf Ebene 2 und super $\to$ extends auf Ebene 2

[[Dynamische Typabfragen|Next]]
