### Client-Server-Beziehung
Server bietet Dienst an, Client nutzt Dienst
Client erfüllt Precondition
Server erfüllt Postcondition
Client + Server erfüllen History Constraints

### Precondition
- Bedingung die *vor* Methodenaufruf erfüllt sein muss
- sind in Untertypen schwächer oder gleich

### Postcondition
- Bedingung die *nach* Methodenaufruf erfüllt sein muss
- nicht nur Methodenbeschreibung sondern auch Werteeinschränkungen
- sind in Untertypen stärker oder gleich

### Invariante
- verantwortlich Server
- sind in Untertypen stärker oder gleich
- muss vor und nach Methodenaufruf erfült sein
- direkt bei Variable angeschrieben
```Java
//@invariant 0 > x > 100
private int x = 1;
```
- wenn z.B. Setter existieren ist auch der Client verantwortlich

### History Constraints
#### Server
- ähnlich invarianten; aber auch zeitliche Einschränkung
- sind in Untertypen gleich stark; wenn T verhindert das x → y wird dann muss auch U verhindern das x → y wird; es gilt das selbe wie für Invarianten

#### Client
- Einschränkung auf Reihenfolge der Methoden aufrufe
- gleiche Einschränkung wie bei Preconditions

### Zusicherungen
- gehören zu nominalen Typen
- Objekttyp besteht aus: Schnittstelle (Signatur), Name oder Klasse/Interface, Zusicherungen
- Änderung von Zusicherung == Typänderung → Auswirkung auf andere Programmteile

[[Klassenbeziehungen|next]]