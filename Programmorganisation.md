### Modul
- wird vom Compiler in einem Stück behandelt
- z.B. Interface oder Klasse
- Getrennte Module werden erst zur Laufzeit von einem Linker verbunden
	- Vorteil: 
		- wird ein Modul verändert muss nur dieses neu compiliert werden und nicht der gesamte Code
		- lassen sich getrennt entwickeln, arbeiten im Team leichter möglich
- Können nicht zyklisch Importiert werden (im Regelfall)
	- Zyklen können aufgelöst werden indem das Modul in 2 Teile gespalten wird in einen Teil welcher eine Schnittstelle bildet und in ein zweites Modul welches die Implementierung beinhaltet

##### Schnittstelle
- zusammengefasste Information über den Inhalt des Moduls
- nicht nur Interfaces
- halten Informationen über die Module und deren Sichtbarkeit (public, private)
	- Vorteil:
		- Private Variablen können vom Compiler besser optimiert werden

### Objekt
- werden erst zur Laufzeit erzeugt
- keine starken Einschränkungen bezüglich zyklischer Abhängigkeit
- Datenkapselung + Data-Hiding = Datenabstraktion
- Identität, Zustand, Verhalten
- identisch: Wenn zwei Variablen die selbe Objektreferenz halten
- gleich (Kopie): Wenn sie den selben Zustand und Verhalten haben

### Klasse
- Schablone zur Objekterzeugung
- Java-Interfaces sind auch Klassen
- Abstrakte Klassen
- Interfaces sind spezielle Abstrakte Klassen
- Zyklische Abhängigkeiten verboten

### Komponente
- Ist eine flexibleres Modul
- Importiert Inhalte welche zur Compiletime noch nicht bekannt sind
- Werden zuerst hinzugefügt und dann wird festgelegt von wo importiert wird, erlaubt dadurch zyklische Abhängigkeiten

### Namensraum
- fassen mehrere Modularisierungseinheiten zusammen ohne die getrenne Übersetzbarkeit zu beeinflussen
- erlaubt dadurch die Schachtelung von Einheiten → es entsteht eine Art Verzeichnisstruktur

[[Parametrisierung|Next]] 