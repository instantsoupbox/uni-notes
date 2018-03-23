# 08 - Ein-/Ausgabe

Das BS soll
* Ein- und Ausgabegeräte steuern und überwachen
* Kommandos an die Geräte senden
* Unterbrechungsereignisse empfangen
* Fehlerbehandlungen durchführen
* Einfach zu nutzende, **geräte-unabhängige Schnittstelle** zu den Geräten bereitstellen

## 9.1 Einführung

**Arten von Geräten**:

* **Blockorientierte Geräte** (block devices):
    * mit adressierbaren Inhalten
    * gespeicherte Information in Blöcken fester Größe
    * **Random access** auf jeden Block möglich
    * z.B. Blu-ray-Disk, Festplatte
* **Zeichenorientierte Geräte** (character device):
    * nicht adressierbare Inhalte
    * Lieferung / Empfang von Zeichenströmen
    * z.B. Maus, Tastatur, Drucker, Netzkarte
* Andere:
    * Clock, die Unterbrechungen erzeugt.
    * Screen

## 9.2 Schichten eines E/A-Systems

0. **Gerät**
1. **Controller**
    * Bestandteil der Geräte HW (mech., elektron. Komponente)
    * Schnittstelle zum Prozessor
2. **Interrupt-Handler**
    * Behandlung d. Geräte-Rückmeldungen
3. **Gerätetreiber**
    * Ausführung d. geräteabhängigen Steuersoftware
    * Treiber greift über Controller auf die Geräte zu
4. **Geräteunabhängige Software**
    * Puffern von Informationen
5. **User-Level Software**
    * Bibliotheksroutinen f. E/A, Spooling

# 9.3 HW-Basis

## Geräte-Controller

* überträgt einen Bitstrom zum Gerät
* Komponenten:
    * **HW-Schnittstelle** zum Gerät
    * **Register** zur Kommunikation mit CPU
        * Datenübertragung zum Gerät: BS schreibt ins Register
        * Zustandsabfrage: BS liest aus Register
    * **Datenpuffer**, die das BS lesen/schreiben kann

## Direct Memory Access DMA

Wie kann man Daten vom E-/A-Gerät in den Speicher übertragen, denn Datenübertragung via Register als Zwischenspeicher würde viele CPU-Zyklen kosten.

**DMA-Controller** (Direct Memory Access):

* Zugriff auf Speicherbus unabhängig von der CPU
* Ablauf:
    1. Disk-Controller stellt Daten im Puffer bereit (Startkommando)
    2. DMA-Controller initiiert übertragung von Disk in RAM
    3. DMA-Controller meldet Beendigung der übertragung (ACK)
    4. BS kann zugreifen

# 9.4 Unterbrechunsgbehandlung

1. Device meldet Beendigung einer E-/A-Aktivität.
2. **Interrupt Controller Chip** erkennt das Signal, informiert CPU.
3. BS unterbricht aktuellen Prozess, wechselt in Kernel-Mode.
4. Ausführung d. Unterbrechungsbehandlungsroutine d. Gerätetreibers im privileg. Modus
5. Gerät beendigt Ausführung, signalisiert diese
6. BS versetzt auf E/A wartenden P in rechenwilligen Zustand

# 9.5 Geräte-Treiber

Definition:
* Gerätetyp-spezifisch
* Übermittlung von Befehlen an das Gerät
* Bestandteil des BS-Kern
* `1 : n`: `1` Treiber verwaltet `n` Geräte einer Klasse

Aufgaben:
* Definition d. Geräts ggü. d. BS, Aktivierung d. Geräts
* Initialisierung d. Controllers u. das Gerät beim Systemstart
* Umwandlung v. E/A-Anforderungen in gerätespezifische Befehle
* Antworten auf HW-SIgnale d. Gerät bzw. d. Controllers
* Meldung von Geräte-/Controllerfehlern
* Empfang/Senden von Daten u. Zustandsinformationen
* Puffern von Daten bei E/A

Zusammenspiel zwischen Geräte-Treiber und Controller:
1. Treiber schickt Kommandos an Controller, blockiert sich
2. Controller signalisiert Ende d. Datenaustauschs
3. Interrupt Handler entsperrt den Treiber
4. Treiber verarbeitet empfangene Daten, schickt weitere Daten

## 9.6 Geräte-unabhängige Software

Der Aufwand, eine individuelle BS-Schnittstelle für jedes Gerät zu definieren, wird umgangen, indem das BS eine einheitliche Schnittstelle definiert, die die Treiber unterstützen müssen. Dies erleichtert die Programmierung von Treibern und erleichtert die Einbindung neuer Treiber-Software.

**Aufgaben**:
* Bereitstellung einer **einheitlichen Schnittstelle** zwischen Treibern und dem Rest des BS
* Pufferung (falls nicht schon auf HW-Ebene im Treiber-Controller geschehen)
* Fehlerbehandlung
* Festlegung geräteunabh. Blockgröße bei Blockgeräten

Beispiel UNIX/Linux:
* Funktionen d. Dateisystems ermöglicht Zugriff auf E/A-Geräte. Diese sind Teil einer Systembibliothek. 
* Geräte werden i.d.R. unter de Teilbaum `/dev` verwaltet.
* i-node einer Gerätedatei enthält
    * **Major Device Number**: Gerätetreiber.
    * **Minor Device Number**: Gerät, auf das zugegriffen werden soll.

## 9.7 E/A-Funktionen auf Nutzerebene
**Spooling**:
* Simultaneous Peripheral Operations Online.
* Zur Einfachen Verwendung exklusiv nutzbarer Geräte.
* P geben Auftrag an **Spooler-Dämon**, nur dieser interagiert direkt mit dem Gerät.
* In einem **Spooling-Verzeichnis** werden Aufträge der Benutzerprozesse gespeichert.

## 9.8 RAID
Redundant Array of Inexpensive (Independent) Disks
| RAID Level | Feature | Eigenschaft|
|---|---|---|
|**0**|Block-level Striping| Aufteilung d. Speicherbereichs in gleich große Abschnitte nach RR-Strategie|
|**1**|Mirroring|Mirroring: Spiegelung (Verdoppelung d. Platten)|
|**2**|Bit-level Striping & Parity-Platten|Hamming Code zum Erkennen und Korrigieren von Bitfehlern|
|**3**|Byte-level Striping & Parity Disk| |
|**4**|Block-level Striping & Parity Disk | |
|**5**|Block-level Striping & Distributed Parity| |
|**6**|Block-level Striping & Double Distributed Parity| |

## 9.9 Clock
Komponenten einer Hardware-Uhr:
* Zähler
* Uhrenquarz
* Batterieversorgung

Aufgaben:
* Verwaltung v. Tageszeit und Datum
* Umschalten nebenläufiger P
* Messung v. Rechenzeitverbrauch eines P's
* Laufzeitüberwachung

### Soft Timer
**Soft-Timer** (Software-based timer) zur Vermeidung von teuren/aufwändigen Interrupts.

Bevor das Betriebssystem den Kernel Mode verlässt, überprüft es ob einer der Soft-Timers abgelaufen ist. Falls ja, so wird das zugehörige Event behandelt.