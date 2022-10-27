### Synchronisation
```Java
public class Zaehler {  
	private int i = 0, j = 0;  
	public void schnipp() { i++; j++; }  
}
```
Wenn schnipp von mehreren Threads aufgerufen wird kann es passieren das sie nicht mehr synchron sind → synchronized keyword zum synchronisieren
```Java
public synchronized void schnipp() { i++; j++; }
```
synchronized erlaubt es nur einem Thread die Methode aufzurufen; wenn mehrere die Funktion aufrufen werden die Threads so lange geblockt bis der Thread fertig ist mit der Methode

### Synchronisieren von Code-Blöcken
```Java
public void schnipp() {  
	synchronized(this) { i++; }  
	synchronized(this) { j++; }  
}
```
Blöcke erlauben granuläre Synchonisation von Blöcken in größeren Methoden; führt in diesem Fall dazu das **i** und **j** kurzzeitig unterschiedliche Werte haben. Lese- und Schreibzugriffe auf **volatile** Variablen sind atomar. Reicht nicht wenn wie oben mehrere Variablenzugriffe erfolgen → **AtomicInteger** erlaubt es ohne **synchronized** atomare Operationen zu machen.

```Java
public class Druckertreiber {  
	private boolean online = false;  
	public synchronized void drucke (String s) {  
		while (!online) {  
			try { wait(); }  
			catch(InterruptedException ex) { return; }  
		}  
		... // schicke s zum Drucker  
	}  
	public synchronized void onOff() {  
		online = !online;  
		if (online) notifyAll();  
	}  
	...  
}
```
Thread wartet bis online true ist; notifyAll weckt den Thread auf das sich etwas geändert hat. **wait, notify & notifyAll** funktionieren nur in **synchronized** Bereichen/Methoden.

#### !Achtung! notify weckt einen Thread auf und kann ungewollt einen Thread aufwecken → System blockiert

```Java
public class Produzent implements Runnable {  
	private Druckertreiber t;  
	
	public Produzent(Druckertreiber t) { this.t = t; }  
	public void run() {  
		String s = ....  
		for (;;) {  
			... // produziere neuen Wert in s  
			t.drucke(s); // schicke s an Druckertreiber  
		}  
	}  
}
```
```Java
Druckertreiber t = new Druckertreiber(...);  
for (int i = 0; i < 10; i++) {  
	Produzent p = new Produzent(t);  
	new Thread(p).start();  
}
```

## Nebenäufigkeit in der Praxis
- java.util.concurrent
- java.util.concurrent.atomic
- java.util.concurrent.locks

Wichtigsten Packages für die Nebenläufigkeit

### Aufgaben und Threads
**Future**: Ergebnis wird im Future abgelegt während es im Hintergrund berechnet wird → andere Berechnungen können währendessen ausgeführt werden und das Ergebnis kann ausgelesen werden wenn nötig

In Java gibt es **java.util.concurrent.FutureTask und java.util.concurrent.Future**. Oft ist es nicht immer sinnvoll für kleine Dinge Threads zu starten → **Executor & ThreadPoolExecutor**

### Streams
ist ein **interner Iterator** 
```Java
HashSet<String> nums = ...; // "1", "2", ...  
int sum = nums.stream()  
	.mapToInt(Integer::parseInt)  
	.reduce(0, (i, j) -> i + j);
```

### Threadsichere Datenstrukturen
- ConcurrentHashMap statt HashMap
- Collections.synchronizedMap oder List(new HashMap oder List(...))

[[WHF4|Next]]
