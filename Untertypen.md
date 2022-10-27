### Untertypen
- Ein Typ U ist Untertyp eines Typs T wenn jedes Objekt von U überall verwendbar ist wo ein Objekt von T erwartet wird
- Beim Programmieren liegt die Einhaltung dieser Bedingung zur Gänze in unserer Verantwortung
- Zusicherungen
- int nicht als Untertyp von long betrachtet werden kann, obwohl jede Zahl in int auch in long vorkommt
- Deklarierter Typ: Typ mit der die Variable deklariert wurde
- Dynamischer Typ: spezifischter Typ den der Wert in der Variable gespeichert hat
- Statischer Typ: wird vom Compiler ermittelt (auto in C++)
- Dynamisches Binden:
	- auszuführende Methode wird erst zur Laufzeit bestimmt
	- Compiler kennt nur den statischen und deklarierten Typ des Empfängers
	- bei static, final, private kennt der Compiler die Methode → statisches Binden
- Vererbung
	- Übernehmen von Code aus der Oberklasse
	- Erweiterung:
		- Subclass erweitert die Baseclass um Variablen und Methoden
	- Überschreiben:
		- Methoden aus der Baseclass werden überschrieben durch neue Körper
- Single inheritance: Klassen kann nur von einer anderen Klasse erben
- Merhfachvererbung: $-\bullet\bullet-$ 

[[OOP|Next]] 