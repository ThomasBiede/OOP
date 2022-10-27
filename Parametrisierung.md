### Parametrisierung
- man lässt bewusst Lücken die man erst später befüllt (Konstruktor, Generics,...)

#### Dynamisches Befüllen (Runtime)
- Konstruktor:
	- Man gibt den Typ der Daten vor mit welchen ein Objekt befüllt wird und übergibt diese erst zu einem späteren Zeitpunkt
- Initialisierungsmethode:
	- Objekte werden im ersten Schritt erzeugt und im zweiten initialisiert bevor sie verwendbar sind
	- nur imperative Paradigmen
- Zentrale Ablage:
	- z.B. Globals oder Konstanten
	- Werden erst zum Zeitpunkt der Objekterzeugung abgeholt
	- "Abholen" z.B. durch Vererbung möglich

##### Dependency-Injection
- Übertragen der Verantwortung zur Erzeugung und Initialisierung von Objekten an eine zentrale Stelle

#### Statisches Befüllen
- Generizität:
	- Lücken sind bereits konzeputell zur Compiletime befüllt
	- Alle [[Programmorganisation |Modularisierungseinheiten]] außer Objekte können theoretisch mit Generics befüllt werden 
	- Stehen meist an stellen wo nicht [[First-Class-Entities]] stehen also meistens zum ersetzen von Typen
	- Wird an stellen verwendet wo das befüllen zur Laufzeit nicht funktioniert
	- Oft werden Einschränkungen mit den Typen angegeben, lassen sich oft schwer ausdrücken wenn es sich nicht um [[First-Class-Entities]] handelt
- Annotation:
	- optionale Parameter
	- wirkt sich statisch auf den Compiler aus
	- kann meist auch dynamisch verwendet werden
	- Spezielle Kommentare welche wenn nötig den Programmablauf beeinflussen
- Aspekte (ähnlich wie Makros)
	- man fügt Aspekte außerhalb vom Programm hinzu
	- Aspect-Weaver:
		- modifiziert das Programm entsprechen bevor es kompiliert wird, manche erst zur Laufzeit
	- Problem: Wenn Lücken sich ändern muss sich oft auch die Art der Befüllung ändern

[[Ersetzbarkeit|Next]] 

