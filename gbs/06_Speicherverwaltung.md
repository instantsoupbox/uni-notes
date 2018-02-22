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

* Strategien zur **Frei-Speicherverwaltung**
    
    * First-Fit, Next-Fit, Best-Fit, Worst-Fit
    * Buddy-Algorithmus

* **Interne Fragmentierunt**:
    * Bei Allokierung wird mehr Speicher vergeben als benötigt.

* **Externe Fragmentierung**:
    * Löcher im HS durch Dynamik von Anlegen und Freigeben von Speicherblöcken 

## 6.5 Virtueller Speicher
Der Virtuelle Adressraum von Prozessen kann ggf. viel größer sein als der reale physische HS. 

Das BS übernimmt also die Rolle, bestimmte Adressbereiche in den HS automatisiert ein- und auszulagern. 

### Paging

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
* Bei der Adressabbildung wird also der `s`-Teil der virt. Adresse durch die Frame-Nummer ersetzt.
* Ein Page-Table-Eintrag enthält
    * Die Frame-Nummer, auf die die Page abbildet.
    * Weitere Informationen (s. Tabelle):

| | | |
|-|-|-|
|`P`|`present`| Physische Page ist vorhanden. |
|`U / S`|`user / supervisor`| Nur BS-Kern darf die Page lesen/schreiben. |
|`R`|`referenced`| Auf die Page wurde zugegriffen. |
|`M`|`modify`| Mindestens 1 Byte wurde in dem referenzierten Frame geschrieben. |
|`XD`|`execute disable`| Protection Bit: CPU darf in diesem Frame gespeicherte Instruktionen ausführen. |

* Eine Page-Table kann sehr groß sein und daher im HS viel Platz belegen. 
* Lösung: Mehrstufige Page-Table

### Page Fault
* Auslöser: `P`-Bit im Eintrag ist nicht gesetzt.
1. Interrupt-Auslösung durch HW
2. Page Fault Handler speichert Programmzustand, deaktiviert Interrupts
3. BS behandelt Page Fault
    * Seite ausgelagert? Lagere sie in freie Frame ein.
    * Seite nicht ausgelagert? BS löst Speicherschutz-Fehler aus
4. Aktualisierung d. Page-Table-Eintrage: Setze `P`, lösche `R`- und `M`-Bit.
5. Wiederherstellung, reaktiviere Interrupt
6. Verlassen des Interrupt-Kontexts.

### Seitenersetzungsstrategien
`//todo:`
