# 08 - Linker / Loader

## 8.1 Systemprogrammierung
Systemprogrammierung:
* Konstruktion von Algorithmen für ein Rechensystem.
* Diese Algorithmen sollen die Ausführung von Benutzerprogrammen organisieren, steuern und selbst durchführen.
* Dabei müssen Qualitätsgesichtspunkte, wie Performanz, Zuverlässigkeit berücksichtigt werden.

System-Software:
* stellen Dienste für (Anwendungs-)programme zur Verfügung.
* hardwarenah programmiert
* optimiert auf effiziente Ausführung
* Beispiele: BS, Gerätetreiber, Compiler, Linker, Loader.

## 8.2 Hardware-nahes Programmieren
* Problem: Fehleranfälligkeit, Aufwand
* Assembler als abstraktere, maschinennahe Ebene für die Programmierung.

## 8.3 Assembler
Assembler:
* maschinennahe Programmiersprache
* angelehnt an ISA der Zielarchitektur
* Vereinfachung d. Nutzung der ISA
* Aufgaben:
    * Assemblerbefehle in Maschinencode transformieren
    * Zuweisung v. Maschinenadressen zu symbolischen Namen
    * Erzeugung eines **object files**

Transformation von Hochsprache zu Maschinencode:
1. Preprocessor: Aufbereitung
2. Compiler: C zu Assembler
3. Assembler: Maschinensprache zu ISA
4. Linker (Binder): Nachbearbeitung

## 8.4 Linker
Der Assembler erzeugt auf einer einzelnen Compile Unit (Segment) Informationen für den Linker (Relocation-Einträge / Lade-Objekte / Relocatables / object file). Er erzeugt auch bestimmte Informationen für den Linker (**Relocation Einträge**).

Der Linker fasst dann einzelne Lade-Objekte in einer gemeinsamen Binärdatei für eine Ausführung zusammen während er externe Referenzen auflöst. Diese Informationen bekommt er vom Assembler durch Relocation Einträge.

**Link-Time-Relocations**:
* Informationen für den Linker, z.B.
    * nicht-statische Funktionen, die exportiert werden müssen
    * verwendete externe Symbole

### Statisches Linken
* Alle Module werden in einer ausführbaren Datei zusammengefasst.
* Das Linken findet dann zur *compile time* statt.

### Dynamisches Linken
* Einzelne Module (shared libraries) werden vorher zusammengefasst.
* Auflösen von Referenzen wird bis zum Laden des Programms verschoben. 
* Während der *load time* des Programms werden durch das BS vollständige Bibliotheken oder Unterprogramme aus Programmbibliotheken geladen.

## 8.5 Loader
Der Loader fügt die vollständigen Binärdateien / Bibliotheken des Linkers im Speicher zusammen und startet die Prozessausführung.

**Load-Time-Relocations**:
* Informationen zu referenzierten Bibliotheksfunktionen.
* Tabelle mit Load-Time-Relocations (Relokationstabelle) in einem geladenen Objekt liefern Informationen zum Schreiben von Adressen best. Symbole an best. Adressen.