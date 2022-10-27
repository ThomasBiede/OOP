## Decorator (Wrapper)
- gibt Objekten dynamisch zusätzliche Verantwortlichkeiten ohne andere Objekte dadurch zu beeinflussen
- für Verantwortlichkeiten, die wieder entzogen werden können
- Wenn Vererbung unpraktisch ist, oder nicht möglich ist (final Klassen)

![[Pasted image 20220312163231.png]]

- Decorator hält ein Objekt von Component; ist meistens Abstract außer es gibt nur eine theoretische Vererbung die eigentlich dirket im Decorator implementiert werden könnte
- Die ConcreteDecorator fügen oberflächliche aber nicht inhaltliche Funktionen hinzu

Eigenschaften:
	- mehr Flexi. als statische Vererbung
	- vermeiden Klassen, die schon oben mit Methoden und Variablen überladen sind; Concrete Components müssen nicht die volle Funktionalität implementieren
	- Objekte von Decorator und ConcreteDeco sind nicht identisch
	- führen zu vielen kleinen Objekten → schwerer zu warten und zu verstehen

```Java
public interface Window {  
	void show(String text);  
}  

public class WindowImpl implements Window {  
	public void show(String text) { ... }  
}  

public abstract class WinDecorator implements Window {  
	protected Window win;  
	public void show(String text) { win.show(text); }  
}  

public class ScrollBar extends WinDecorator {  
	public ScrollBar(Window w) { win = w; }  
	public void scroll(int lines) { ... }  
	public Window noScrollBar() {  
		Window w = win;  
		win = null; // no longer usable  
		return w;  
	}  
}  

Window w = new WindowImpl(); // no scroll bar  
ScrollBar s = new ScrollBar(w); // add scroll bar  
w = s; // s aware of scroll bar, w not  
w.show("Text"); // no matter if scroll bar or not  
s.scroll(3); // works only with scroll bar  
w = s.noScrollBar(); // remove scroll bar
```

## Proxy (Surrogate)
- Platzhalter für anderes Objekt und kontrolliert Zugriff

Beispiel: Erzeugen teurer Objekte dann wenn sie gebraucht werden
```Java
public interface Something {  
	void doSomething();  
}  

public class ExpensiveSomething implements Something {  
	public void doSomething() { ... }  
}  

public class VirtualSomething implements Something {  
	private ExpensiveSomething real = null;  
	public void doSomething() {  
		if (real == null)  
			real = new ExpensiveSomething();  
		real.doSomething();  
	}  
}
```
ExpensiveSome. wird einmal erzeugt und erst dann wenn doSomething aufgerufen wird

### Remote Proxies
- Platzhalter für Objekte in anderen Namensräumen
- Nachrichten werden von den Proxies über komplexe Kanäle weitergeleitet

### Virtual Proxies
- erzeugt Objekte bei Bedarf

### Protection Proxies
- kontrollieren Zugriffe auf Objekte. Derartige Proxies sind sinnvoll, wenn Objekte je nach Zugreifer oder Situation unterschiedliche Zugriffsrechte haben sollen

### Smart References
- ersetzen einfache Zeiger
- Mitzählen der Referenzen (Reference Counting)
- Laden von persistenten Objekten in den Speicher, wenn das erste mal darauf zugegriffen wird
- Zusichern, dass während des Zugriffs auf das Objekt kein gleichzeitiger Zugriff durch einen anderen Thread erfolgt

![[Pasted image 20220312165431.png]]

Proxies verwaltet:
	- eine Ref. "realSubj" über die Proxy auf Objekte von "RealSubj" zugreift
	- Stellt Schnittstelle bereit die der von Subject entspricht
	- kontrolliert Zugirffe auf das eigenliche Objekt und Erzeugung/Entfernung
	- hat weiter Verantwortungen, je nach Art

## Template Method
- definiert das Grundgerüst eines Algorithmus in einer Operation, überlässt die Implementierung einiger Schritte aber einer Unterklasse
- Abhängigkeit: Oberklasse von Unterklasse abhängig

Anwendbar:
	- um den unveränderlichen Teil eines Algorithmus nur einmal zu implementieren und es Unterklassen zu überlassen
	- gemeinsames Verhalten mehrerer Unterklassen in einer Oberklasse zusammengefasst
	- mögliche Erweiterungen in Unterklassen zu kontrollieren über **Hooks**

![[Pasted image 20220312172722.png]]

Eigenschaften:
	- fundamentale Technik zur direkten Wiederverwendung von Programmcode
	- Hollywood Prinzip("Don’t call us, we’ll call you"); Oberklasse ruft Methode in Unterklasse auf

primitiven Operationen, die von der Template-Methode aufgerufen werden, sind in der Regel protected Methoden.

[[WHF5|Next]]
