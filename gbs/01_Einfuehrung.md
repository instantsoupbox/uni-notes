# 01 Einführung

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
* Management

## 1.2 BS Betriebsarten
1. Stapelverarbeitung (batch processing)
2. Dialogbetrieb (time sharing)
3. Transaktionsbetrieb (transaction system)
4. Echtzeitbetrieb (real time system) unter
    * harten deadlines
    * weichen deadlines

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
Systemcalls dienen als Schnittstelle für Anwendungen zum Zugriff auf die Hardware. Aufruf üblicherweise über **Systembibliotheken**, die zum Anwendungsprogramm (z.B. über Linker/Loader) hinzugebunden werden.

Ablauf:
`//todo`
* **Trap-Befehl** realisiert einen Systemcall (Unterbrechung des aktuellen Ablaufs, Sprung in den BS-Kern)

## 1.5 Betriebssystem-Architekturen
### Monolithisches System
* **Betriebssystem-Kern** umfasst die vollständige Menge an Funktionen des BS
* Alle Treiber im Kern implementiert
* Kern kann durch Systemcalls betreten werden.

### Mikrokerne
* Aufteilung der Funktionen des BS in Module, nur eines davon - der **Mikrokern** - läuft im Kernel Mode.
* Mikrokern bietet nur Basismechanismen:
    * IPC
    * Scheduling
* Subsysteme als Systemdienste im User Mode:
    * Dateisystem
    * Speicherverwaltung
    * Prozessverwaltung
* Systemfunktionen als **Serverprozesse**

## 1.6 Systemprogrammierung
* **Systemprogrammierung**: Konstruktion von Algorithmen für ein Rechensystem
* Diese Algorithmen organisieren die Bearbeitung/Ausführung von Benutzerprogrammen unter Qualitätsgesichtspunkten
* **Systemsoftware**: Programme, die Dienste für Anwendungsprogramme zur Verfügung stellen
    * Betriebssysteme
    * Treiber
    * Compiler, Linker, Loader

## 1.7 Betriebssystem, Assembler, Maschinenebene
`//todo`