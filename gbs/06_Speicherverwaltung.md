# 06 - Speicherverwaltung
## 6.1 Einführung
* Hauptspeicher = RAM = Main Memory
* **Von-Neumann-Flaschenhals**:
    * Prozessoren werden zwar schneller, aber Speicherzugriffszeiten können damit nicht Schritt halten.
    * Lösung: Einführung einer Speicherhierarchie.
* Speicherhierarchie:
    1. Register
    2. Cache
    3. Hauptspeicher
    4. Festplatten
    5. Bandlaufwerke

## 6.2 Adressraumkonzept
* Jedes Programm belegt potentiell den gesamten HS des Rechners.
* P benötigen Kenntnis über *Struktur* des HS.
* P können das ebenfalls im HS befindliche BS korrumpieren.
* **Adressraum**: 
    * Abstrakte, isolierte Menge an Speicherzellen, die ein Prozess adressieren kann. 
    * Zweck: Schutz des Betriebssystems, das im physischen Hauptspeicher liegt, vor Programmen, falls diese auf den gesamten physischen Hauptspeicher zugreifen könnten.
* **Physischer Adressraum** des Hauptspeichers (bestehend aus physischen Speicheradressen)
* **Logischer Adressraum** eines Prozesses (bestehend aus logischen / virtuellen Adressen)

## 6.3 Speicherabbildung (Memory Address Translation)

* Direkt
* Basis
* Segment
* (Seiten-Adr.)
* (Segment-Seiten-Adr.)

### Direkt-Adressierung

Jede logische Adresse entspricht ihrer physischen Adresse.

### Basis-Adressierung

`physische_adr = logische_adr + basis_adr`

### Segment-Adressierung

Unterteilung des Adressraums in logische Segmente untersch. Länge und mit untersch. Zugriffsberechtigungen:

* Stack
* Daten
* Code

**Global Descriptor Tabelle** (GDT) verwaltet die Infos für jedes der Segmente:

* Basisadresse
* Segmentlänge
* Flags (Zugriffsberechtigungen, Code/Daten etc.)

## 6.4 Frei-Speicherverwaltung

Wir brauchen folgende Dinge, um Hauptspeicherbereiche zu Adressräumen zuzuordnen:

* Datenstrukturen zur Frei-Speicherverwaltung
    
    * **Bitmap** (`1` := Block belegt, `0` := Block frei)
    * **Verkettete Liste**
        * Nodes: Zuordnung (`P` := einem Prozess, `F` := frei), Anfangsadresse, Länge, Zeiger auf nächsten Eintrag / Terminator

* Strategien zur **Frei-Speicherverwaltung** (also zur Belegung der freien Speicherbereiche / Speicherallokation. Dabei sind die Speicherbereiche als verkettete Liste implementiert.)
    
    * **First-Fit**: Suche ab Listenkopf, erster passender Freibereich wird belegt.
    * **Next-Fit**: Suche ab letztem belegten Bereich, erster passender Freibereich wird belegt. 
    * **Best-Fit**: Belegung d. Freibereichs m. kleinstem Verschnitt.
    * **Worst-Fit**: Belegung d. Freibereichs m. größtem Verschnitt.
    * **Buddy-Algorithmus**

|Belegungsstrategie|Funktionsweise|Such-/Verwaltungsaufwand|Fragmentierung|
|-|-|-|-|
|First-Fit|Suche ab Listenkopf, erster passender Freibereich wird belegt.|Ansammlung vieler kleiner Freispeicherbereiche am Anfang d. Liste, dadurch hoher Such und Verwaltungsaufwand| hohe Fragmentierung am Anfang d. Liste|
|Next-Fit|Suche ab letztem belegten Bereich, erster passender Freibereich wird belegt.|Rotation durch Suche ab letztem belegten Bereich|abgeschwächter Effekt von FF|
|Best-Fit|Belegung d. Freibereichs m. kleinstem Verschnitt|Sortierung d. Liste nach **a)** Größe des Freibereichs: hoher Aufwand beim Einfügen, geringer Aufwand bei Belegung; **b)** nicht: lin. Suchaufwand| **a)** höhere Fragmentierung als bei FF|
|Worst-Fit|Belegung d. Freibereichs m. größtem Verschnitt|wie bei Best-Fit|Abspaltung und damit Stellung möglichst großer Bereiche|

* **Interne Fragmentierunt**:
    * Beim Allokieren wird mehr Speicher vergeben als benötigt.
    * Dieser (zu viel belegte) Speicherbereich kann weder vom belegten Prozess, noch von anderen Prozessen genutzt werden.
    * Es kommt zum Verschnitt.

* **Externe Fragmentierung**:
    * Löcher im HS durch Dynamik von *Anlegen und Freigeben von Speicherblöcken*. 
    * Es steht kein ausreichend großer zusammenhängender Bereich zur Verfügung, um eine Anfrage zu bedienen. 

## 6.5 Virtueller Speicher
Der Virtuelle Adressraum von Prozessen kann ggf. viel größer sein als der reale physische HS. 

Das BS übernimmt also die Rolle, bestimmte Adressbereiche in den HS automatisiert ein- und auszulagern. 

### Paging
The basic idea behind virtual memory is that each program has its own address space, which is broken up into chunks called **pages**.

* **Pages**: Einteilung d. virtuellen Adressraums. (Seiten)
* **Frames**: Einteilung d. HS. (Kacheln)
* Aufgabe d. BS: Abbilden von Pages auf Frames (Page Adressen auf physische Adressen in einer Frame) - dies wird von der **Memory Management Unit** (MMU) übernommen. 
* **Page Table** verwaltet die Zuordnung von Page zu Frame und liegt im HS.
* **Page Fault** (Seitenfehler): Page ist in keiner Frame eingelagert, also löst die HW einen Interrupt (page fault) aus. 

### Memory Management Unit
* Abbildung von Pages auf Frames = **Adressrechnung**.
* `( CPU )--Virtuelle Adressen-->( MMU )--Physische Adressen-->( Arbeitsspeicher )`
* Die MMU muss zur Adressrechnung auf die **Page Table** zugreifen, welche im HS liegt. Dies ist eine teuere Operation.
* Lösung: **Translation Lookaside Buffers** (TLB)
    * Cache für Abbildung von virt. Adresse auf phys. Adresse.
    * Wenn virt. Adresse einen Eintrag im TLB hat, dann kann die phys. Adresse direkt ermittelt werden. 
    * Sonst ist eine Adressabbildung durch BS notwendig.

### Page Table
* Virtuelle Adresse `v = (s, w)`
    * `s` := Page-Number (Index in die Page Table)
    * `w` := Offset
* Physikalische Adresse `p = (k, w)`
* Bei der Adressabbildung wird also der `s`-Teil der virt. Adresse durch die Frame-Nummer ersetzt.
* Ein Page-Table-Eintrag enthält
    * Die Frame-Nummer, auf die die Page abbildet.
    * Weitere Informationen (s. Tabelle) in einzelnen Bits:

| | | |
|-|-|-|
|`P`    |`present`            | Physische Page ist vorhanden.                                                  |
|`U / S`|`user / supervisor`  | Nur BS-Kern darf die Page lesen/schreiben.                                     |
|`R`    |`referenced`         | `1`, sobald auf die Page zugegriffen wurde.                                    |
|`M`    |`modify`             | Mindestens 1 Byte wurde in dem referenzierten Frame geschrieben.               |
|`XD`   |`execute disable`    | Protection Bit: CPU darf in diesem Frame gespeicherte Instruktionen ausführen. |

* Eine Page-Table kann sehr groß sein und daher im HS viel Platz belegen. 
* Lösung: Mehrstufige Page-Table

### Page Fault (= Seitenfehler)
* **Auslöser: `P`-Bit im Eintrag ist nicht gesetzt.**
1. Interrupt-Auslösung durch HW
2. Page Fault Handler speichert Programmzustand, deaktiviert Interrupts
3. BS behandelt Page Fault
    * Seite ausgelagert? Lagere sie in freies Frame ein.
    * Seite nicht ausgelagert? BS löst Speicherschutz-Fehler aus
4. Aktualisierung d. Page-Table-Eintrage: Setze `P`, lösche `R`- und `M`-Bit.
5. Wiederherstellung, reaktiviere Interrupt
6. Verlassen des Interrupt-Kontexts.

### Seitenersetzungsstrategien
Strategische Aufgaben:
1. **Wann** ist eine Seite zu laden?
    * **On Demand Paging**: Laden v. Pages, wenn auf sie zugegriffen wird und sie noch nicht im HS sind.
    * **Prefetching**: Laden im Voraus, um bei Bedarf verfügbar zu sein.
2. **Wohin** ist eine Seite zu laden?
3. **Welche Seite** ist auszulagern?
**Optimalerweise** wird diejenige Seite ausgelagert, deren nächster Zugriff am weitesten in der Zukunft liegt. Dies ist jedoch nicht möglich, da man künftige Seitenzugriffe nicht voraussagen kann.

Ersetzungsstrategien:
* FIFO
* Second-Chance
* Clock-Algorithmus
* Least Recently Used
* Working Set

#### FIFO
* Verwaltung nach Ankunftszeiten in FIFO Queue
* Page-out Ereignis: **Auslagerung der Seite, die am längsten im Speicher gehalten worden ist** 
* Einzulagernde Seite ans Ende der Liste

#### Second-Chance
* Page-out Ereignis: Auslagerung der Seite, die das `R`-Bit nicht gesetzt haben. Alle anderen (`R` gesetzt), bekommen eine zweite Chance.

#### Clock-Algorithmus
* Seiten in einer zirkulären Liste.
* Zeiger zeigt auf die älteste Seite.

Seitenersetzung:
* Solange keine Seite mit `R=0` gefunden wurde
    * Falls `R=1`: Reset `R`
    * Inkrementiere Zeiger
* `R=0`: Ersetze Seite durch eine neue

#### Least-Recently-Used
* Auslagerung der Seite, welche am Längsten nicht genutzt wurde.

#### Not-Frequently-Used
* Auslagerung der Seite mit dem kleinsten Software-Counter. 
* Aging-Verfahren:
    * Zähler `A_p` spiegelt das Alter des letzten Zugriffs wider.
    * Page-fault: Auslagerung der Seite mit niedrigstem Zählerstand.

##### Aging-Algorithmus
* LRU-Vergröberung
* Zähler: `b`-bit - Beschränkter zeitlicher Horizont (nur `b`-Zeitintervale), ältere Zugriffe vor b-Intervallen werden vergessen.
* Nach Ablauf des Zeit-Intervalls: Timer-Interrupt
    * Right-Shift des Zählers um 1.
    * Addition des `R`-Bits der Seite auf das höchste Bit.

#### Working Set
Die Menge der Seiten, die der Prozess aktuell zur Berechnung benötigt. 

**Lokalitätsprinzip**: Der Prozess arbeitet immer mit dem gleichen Working-Set.

**Thrashing** (Seitenflattern): Ein Prozess verursacht sehr häufig Seitenfehler, falls das Working-Set nur zu geringem Teil im Speicher eingelagert ist.

**Pre-paging**: Halte das Working-Set für den rechnenden Prozess bereit.

#### Working-Set-Ersetzungs-Verfahren
* Page-fault: Ersetzung der Seite, die sich nicht im aktuellen Working-Set befinden.
* Approximation der letzten $k$-Speicherzugriffe über die Ausführungszeit $\tau$ des Prozesses.
* Neuer Zähler `Z` pro Prozess, der die Rechenzeit d. Prozesses approximiert.
* `R`-Bit wird durch das BS geprüft
    * `R == 1`: Seite gehört zum Working-Set und wird nicht enfernt, `Z_p = Z`
    * `R == 0`: 
        * `Z - Z_p > tau`: Page gehört nicht mehr zum Working-Set, also Auslagerung
        * `Z - Z_p <= tau`: Page gehört zum Working-Set, wurde aber nicht verwendet
    * Ersetze älteste Seite, wenn keine passende Seite gefunden wurde.

## Fallbeispiele

### Windows Fallbeispiel
32-Bit Architektur
* 4 GB Virtueller Adressraum
    * 2 GB für Prozess
    * 2 GB für Kern

`//todo`

Seitenersetzungsstrategie: Aging-artige LRU-Approximation.

### Linux Fallbeispiel
32-Bit Architektur
* 4 GB Virtueller Adressraum
    * 1 GB Seitentabelle und Kernelobjekte

Virtueller Adressraum:
* Text (read-only)
* Daten-Segment (dynamisches Speicherallokieren mit `malloc` auf Heap-Bereich)
* Stack-Segment

4-stufige Page Table

Speicher-Allokation:
* Buddy-Allokator (Kleiner Block: Page)
* Slab-Allocator

Seitenersetzungsstrategie:
* Swap-Bereiche - Seiten werden nicht direkt ausgelagert
* Freihalten einer Menge von Frames, um schnell einlagern zu können.
* Variante des Clock-Algorithmus
