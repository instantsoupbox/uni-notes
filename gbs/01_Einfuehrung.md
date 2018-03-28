# 01 Einführung

Rechensystem:
* offen: 
    * Bestehend aus Komponenten m. Verbindungen untereinander (AbhängigkeitenSchnittstellen zur äußeren Einwirkung
    * Schnittstellen ermöglichen Sichten:
        * Black-Box-Sicht
        * White-Box-Sicht
* dynamisch
* technisch
    * realisiert durch Software-/Hardware-Komponenten

## Schichtenstruktur
```
-------------------------
Anwendungsprogramme
-------------------------
Systemprogramme
-------------------------
Betriebssystem
-------------------------
Hardware
-------------------------
```

## 1.2 Aufgaben eines BS
* Abstraktion
    * Vereinfachung, Bereitstellung abstrakter Konzepte.
* Management
    * Steuerung u. Kontrolle der Programmausführung.
    * Treffen von strategischen Entscheidungen.

## 1.2 BS Betriebsarten unter Berücksichtigung von Betriebszielen
1. Stapelverarbeitung (batch processing)
    * Keine Nutzer-Interaktion
    * Komplette Definition des Programms, anschließend geschlossene Ausführung
2. Dialogbetrieb (time sharing)
3. Transaktionsbetrieb (transaction system)
    * unter ACID-Kriterien (Atomicity, Consistency, Isolation, Durability)
4. Echtzeitbetrieb (real time system) unter
    * harten deadlines
    * weichen deadlines
Betriebsziele:
* Hohe Auslastung
* Kurze Antwortzeiten
* Geringer Energieverbrauch

## 1.4 Grundlegende BS-Konzepte
### Ressourcen-Klassifikation
* Anzahl der Nutzungen
* Parallelität
* Dauerhaftigkeit
* Zentral
* Peripher

### Prozess
* Prozess als **Programm** in Ausführung - aktive Einheit / Instanz eines Programms. Programm als passive Einheit. Ein System besteht aus einer Menge von Prozessen. Zur Ausführung eines Prozesses werden **Ressourcen** benötigt. 
* Jeder Prozess besitzt seinen eigenen Prozessadressraum
* Ausführungsarten:
    * Sequenziell
    * Multi-Threaded

### BS-Modi
* Benutzermodus (User mode / space)
* Systemmodus (Kernel mode / space)

### Systemcalls
Systemcalls dienen als Schnittstelle für Anwendungen zum Zugriff auf die Hardware/den BS-Kern. Aufruf üblicherweise über **Systembibliotheken**, die zum Anwendungsprogramm (z.B. über Linker/Loader) hinzugebunden werden.

Ablauf:
1. Anwendungsprogramm ruft **Bibliotheksfunktion** auf.
2. Bibliotheksfunktion ruft den eigentlichen Syscall auf.
3. **Trap-Call**: Unterbrechung d. aktuellen Ablaufs, Sprung in den BS-Kern.
4. Ausführung d. Syscalls im Kernel-Mode.

**Trap-Befehl** realisiert einen Systemcall (Unterbrechung des aktuellen Ablaufs, Sprung in den BS-Kern)

## 1.5 Betriebssystem-Architekturen
### Monolithisches System
* Alle BS-Funktionen sind im **Betriebssystem-Kern** zusammengefasst und laufen im Kernel Space ab.
* Alle Treiber im Kern implementiert.
* Der Kern kann durch Systemcalls betreten werden.

### Mikrokerne
* Im Mikrokern sind nur Basismechanismen wie Scheduling und IPC implementiert. Nur der Mikrokern läuft im Kernel Mode.
* Alle anderen Funktionen des BS, wie Speicherverwaltung, Dateisysteme und Prozessverwaltung sind in Subsysteme im Usermode ausgelagert. 
* (Systemfunktionen als **Serverprozesse**)

## 1.6 Systemprogrammierung
* **Systemprogrammierung**: Konstruktion von Algorithmen für ein Rechensystem
* Diese Algorithmen organisieren die Bearbeitung/Ausführung von Benutzerprogrammen unter Qualitätsgesichtspunkten
* **Systemsoftware**: Programme, die Dienste für Anwendungsprogramme zur Verfügung stellen
    * Betriebssysteme
    * Treiber
    * Compiler, Linker, Loader

## 1.7 Betriebssystem, Assembler, Maschinenebene

* **ISA**: Instruction Set Architecture als Befehlssatz des Prozessors.
    * **RISC**
    * **CISC**
* **Assembler**: Maschinennahe Programmiersprache. Angelehnt an die ISA, soll sie die Nutzung der ISA vereinfachen.
* **Compilieren**: Übersetzen von C in Assembler
* **Assemblieren**: Übersetzung von Maschinensprache in ISA
* **Link** (Binden): Binden einzelner Module
* **Load**: Laden der gebundenen Module in den Speicher
