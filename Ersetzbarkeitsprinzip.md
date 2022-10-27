### Ersetzbarkeitsprinzip
- $Obertyp: T, Untertyp: U$  
- Ein Typ U ist Untertyp eines Typs T , wenn jedes Objekt von U überall verwendbar ist, wo ein Objekt von T erwartet wird
- wichtig bei Methodenaufrufen mit Typ T
- Objekt zuweisungen z.B 
- nominale Typen gilt daneben noch die Bedingung, dass der Untertyp explizit vom Obertyp abgeleitet sein
```Java
T object = new O();
``` 

### Strukturelle Untertypbeziehung
$a \downarrow b$  a ist Untertyp von B
$a \uparrow b$  a ist Obertyp von B
- reflexiv: $\forall u \in U \to u \downarrow u$   
- transitiv: $U \downarrow A$ and $A \downarrow T \to U \downarrow T$ 
- antisym: $U \downarrow T$ and $T \downarrow U \to U=T$ 
- $U \downarrow T :\iff$ 
	- ∀ Konstante in T (Typ A) ∃ Konstante in U (Typ B)
	- ∀ Variable in T (Typ A) ∃ Variable in U (Typ B):
	- ∀ Methode in T ∃ Methode in U
		- Parammanz. und Paramart gleich
		- Paramtypen passen zueinander
		- Ergebnistyp in U Untertyp von Ergebnistyp in T
		- Methode in U wirft nicht mehr Exceptions als die in T

### Dynamisches Binden
```Java
public class A {  
	public String foo1() { return "foo1A"; }  
	public String foo2() { return fooX(); }  
	public String fooX() { return "foo2A"; }  
}  
public class B extends A {  
	public String foo1() { return "foo1B"; }  
	public String fooX() { return "foo2B"; }  
}  
public class DynamicBindingTest {  
	public static void test(A x) {  
		System.out.println(x.foo1());  
		System.out.println(x.foo2());  
	}  
	public static void main(String[] args) {  
		test(new A());  
		test(new B());  
	}  
}

Die Ausführung von DynamicBindingTest liefert folgende Ausgabe:  
	foo1A  
	foo2A  
	foo1B  
	foo2B
```

[[ClientServerBeziehung|next]]
