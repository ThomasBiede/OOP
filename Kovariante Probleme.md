Eingabeparameter sind Kontravariant aber manchmal will man Kovariante Eingabeparameter → kovariante Probleme. 
Lösung: dynamische Typabfragen, Typumwandlungen

```Java
abstract class Futter { ... }  
class Gras extends Futter { ... }  
class Fleisch extends Futter { ... }  
abstract class Tier {  
	public abstract void friss(Futter x);  
}  
class Rind extends Tier {  
	public void friss(Gras x) { ... }  
	public void friss(Futter x) {  
		if (x instanceof Gras)  
			friss((Gras)x);  
		else  
			erhoeheWahrscheinlichkeitFuerBSE();  
	}  
}  
class Tiger extends Tier {  
	public void friss(Fleisch x) { ... }  
	public void friss(Futter x) {  
		if (x instanceof Fleisch)  
			friss((Fleisch)x);  
		else  
			fletscheZaehne();  
	}  
}
```
- friss wird überladen Problem Gras und Fleisch sind Futter → Nebeneffekte wenn Tiger Gras oder Rinder Fleisch bekommen.

#### Faustregel: Kovariante Probleme soll man vermeiden.

### Binäre Methoden (Kovariantes Problem)
- mindestens ein formaler Parameter vom selben Typ wie die Klasse
```Java
abstract class Point {  
	public final boolean equal(Point that) {  
		if (that != null && this.getClass() == that.getClass())  
			return uncheckedEqual(that);  
		return false;  
	}  
	protected abstract boolean uncheckedEqual(Point p);  
}
```

### Überladen vs. Multimethoden
```Java
A x = new B(); // A ist deklarierter(statischer) Typ, B ist dynamischer Typ
x.equal(y);
```
Auszuführende Methode wird von $x$ festgelegt, dynamischer Typ von $y$ ist dabei irrelevant. Der deklarierte Typ von y ist bei der Methoden auswahl wichtig.

Generell  (nicht bei Java) ist es möglich den dynamischen Typ von $y$ in der Methodenauswahl mit einbezogen wird. Dabei legt das Laufzeitsystem erst zu Laufzeit die zu wählende Methode fest und nicht der Compiler (**Multimethoden**).

#### Überladen $\ne$ Multimethoden

#### Deklarierte & dynamische Argumenttypen
```Java
Rind rind = new Rind();  
Futter gras = new Gras();  
rind.friss(gras);       // Rind.friss(Futter x), weil statischer Typ Futter
rind.friss((Gras)gras); // Rind.friss(Gras x)
```

#### Faustregel: Man soll Überladen nur so verwenden, dass es keine Rolle spielt, ob bei der Methodenauswahl deklarierte oder dynamische Typen der Argumente verwendet werden.
Wenn es:
- eine Parameterposition an der sich die Typen unterscheiden, die Typen aber nicht in Untertyprelation stehen und keinen gemeinsamen Untertypen haben
- oder alle Parametertypen der einen Methode Obertypen der Parametertypen der anderen Methode sind, und beim Aufruf nichts anderes passiert als eine Verzweigung auf die andere Methode (falls die dynamischen Typen es erlauben)

ist die strikte Unterscheidung zwischen dekl. und dyn. Typen bei der Methodenwahl nicht wichtig

### Simulation von Multimethoden
- Nachteil: hohe Komplexität (m * n Methoden, M Tiere, N Futterarten)
- Simulieren von mehrfachen dynamischen Binden, durch wiederholtes einfaches Binden

```Java
/**
 * Visitor Pattern
*/
public abstract class Tier { // Elementklasse (auch Unterklassen sind Elementklassen)  
	public abstract void friss(Futter futter);  
}

public class Rind extends Tier {  
	public void friss(Futter futter) {  
		futter.vonRindGefressen(this);  
	}  
}  

public class Tiger extends Tier {  
	public void friss(Futter futter) {  
		futter.vonTigerGefressen(this);  
	}  
}  

public abstract class Futter { // Visitorklassen (auch Unterklassen) 
	abstract void vonRindGefressen(Rind rind);  // Visitormethode
	abstract void vonTigerGefressen(Tiger tiger); // Visitormethode 
}  

public class Gras extends Futter {  
	void vonRindGefressen(Rind rind) { ... }  
	void vonTigerGefressen(Tiger tiger) {  
		tiger.fletscheZaehne();  
	}  
}  

public class Fleisch extends Futter {  
	void vonRindGefressen(Rind rind) {  
		rind.erhoeheWahrscheinlichkeitFuerBSE();  
	}  
	void vonTigerGefressen(Tiger tiger) { ... }  
}
```
Tiger/Rind ruft die jeweilige Fressen Methode in Futter auf → Es gibt in Gras eine **vonTigerGefressen** und in Fleisch eine **vonRindGefressen** Methode welche die Ausnahmen behandlen müssen.
Bei einem Aufruf von **tier.friss(futter)** wird 2 mal dynamisch gebunden.

[[WHF3]]