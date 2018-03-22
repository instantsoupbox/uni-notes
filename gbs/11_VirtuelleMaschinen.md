# 11 - Virtuelle Maschinen

Möglichkeiten der Abstraktion von Hardware-Details durch Interfaces, die unterschiedliche Software-Sichten auf die HW ermöglichen.

* **ISA** (Instruction Set Architecture) zwischen BS (Software) und HW
    * `User-ISA + System-ISA`
    * `USER-ISA`: ISA, der für User-Space sichtbar/zugänglich ist
    * Sicht für das BS auf das physische System
* **ABI** (Application Binary Interface) zwischen Applikationen und BS
    * `Syscall-Interface + User-ISA`
    * Erlaubt Zugriff auf BS und HW durch Applikationen
    * Sicht für User Space Applikationen auf das physische System 

**Virtuelle Laufzeitumgebung**
* Implementiert durch VM.
* Abbildung von virtuellen auf physischen Ressourcen.

Wir werden jetzt System und Process Virtual Machines betrachten.

## 11.2 System Virtual Maschines
* **VMM (Virtual Machine Monitor / Hypervisor)** bietet Laufzeitumgebung für Betriebssysteme
* Durch Implementierung einer **virtuellen ISA** (User-/System ISA) erhalten wir eine Sicht auf eine virtuelle Maschine.
    * Typ 1 VMM - Native System VM: Ausführung direkt auf HW
    * Typ 2 VMM - Hosted System VM: Verwendung v. Diensten d. Host BS
* Vorteile:
    * Emulation von BS auf verschiedenen ISAs
    * Isolierte Ausführung verschiedener BS auf der gleichen HW
    * Security (?)
    * Bessere Ausschöpfung v. HW-Ressourcen
* **VMI (Virtual Machine Introspection)**
    * Idee: Verschiebung d. Security Mechanismen vom BS-Kern in den VMM, sodass Security Applikationen das System von außerhalb des BS analysieren u. schützen können.
    * VMM hat im Vergleich zum BS-Kern
        * Kleinerer Angriffsvektor
        * Höhere Privilegien
        * Uneingeschränkte Sicht auf Zustand aller VMs
    * Malware kann keine Mechanismen auf BS-Kern Ebene umgehen oder deaktivieren, wenn die Security Mechanismen als Teil der VMM ausgeführt werden.
* **Semantic Gap Problem**
    * Zustand einer VM besteht aus immensen Mengen an Binärinformationen.
    * Dieser muss auf BS-Datenstrukturen abgebildet werden, um diese interpretieren zu können. Dies erfordert zusätzliches **Semantisches Wissen**.

VMM kontrolliert und virtualisiert HW-Ressourcen des Systems (CPU, Memory.
* CPU
* Memory
* I/O

### CPU-Virtualisierung
**Para-Virtualization**:
* Bereitstellung eines Hypercall-Interfaces 
* Kommunikation mit dem VMM über Hypercalls
**Binary Translation**:
* VM interpretiert/emuliert Binärcode der Gast VM in Software
* Hohe Komplexität
**Hardware-assisted Virtualization**:
* Ermöglichung d. Implemtation effizienter VMs

**System-Mode**: Modus d. VMMs
**User-Mode**: Modus d. Gast-BS, Instruktionen werden hier durch den VMM im System Mode abgefangen, sodass VMM die Kontrolle über die VM behält

**Instruktionsklassen** einer ISA:
* Privileged Instructions: 
    * nur ausführbar im System Mode
    * im System Mode abgefangen, wenn sie im User Mode ausgeführt werden
* Sensitive Instructions:
    * Control-sensitive Instructions modifizieren System-Konfigurationen
    * Behavior-sensitive Instructions verhalten sich anders, wenn sie im User-Mode ausgeführt werden.
* Innocuous Instructions: sonstige Instruktionen

### Memory Virtualisierung
VMM teilt und isoliert den physischen Speicher zwischen den VMs.
Ansatz 1: **SPT (Shadow Page Tables)**
* Gast BS verwaltet Page Tables, die der MMU nicht bekannt sind.
* VMM bildet in der SPT virtuelle Adressen d. VM auf physische Adressen ab.
Ansatz 2: **SLAT (Second Level Address Translation)**
* Zweite Stufe d. Address-Translation durch SLAT Tables (neben Page Tables).
* SLAT Tables bildet Gast-Physische Adressen GPA auf Host-Physische-Adressen HPA ab
* VMM fängt Zugriff auf Speicherbereich, der nicht in SLAT-Tables gemappt ist, ab.

### I/O Virtualisierung
Es muss verhindert werden, dass Gast VMs die Kontrolle über I/O-Geräte übernehmen, daher wird der Zugriff auf I/O-Geräte entweder durch Soft- oder Hardware gemultiplext.

Virtualisierungstechniken:
* Full Virtualization / Emulation
    * VMM multiplext/emuliert virtuelle I/O Geräte in Software
    * I/O-Anfragen durch Gast wird durch VMM abgefangen und an I/O-Geräte geleitet
* Para-Virtualization
    * Split-Driver-Architektur
        * Frontend Treiber: Treiber in Gast-VM kommuniziert mit Backend-Treiber
        * Backend Treiber: Direkter Zugriff auf Geräte durch privilegierte VM
* Direct I/O
    * HW-unterstütztes Multiplexing von I/O-Geräten
    * I/O MMU verwaltet Geräte-Speicher in Gast VMs

## 11.3 Process Virtual Machines
* Implementierung einer 
    * **virtuelle Laufzeitumgebung** für User-Space-Prozesse
    * **ABI** zwischen Applikationen und BS bestehend aus Systemcall-Interface und User ISA
* Vorteile: 
    * Emulation von Programmen auf unterschiedlichen BS
    * Platformunabhängigkeit: Ausführung von Programmen auf verschiedenen BS
    * Performance-Optimierung:

### High-level-Language VMs
* Definition einer platform-unabhängigen virtuellen ISA.
* Verhinderung von Abhängigkeit von BS und HW-Architektur.

### OS-level Virtualization: Container
* Bereitstellung einer Laufzeitumgebung für User Space Prozesse.
* Leichtgewichtige Virtualisierung.
* Container verwenden die selbe ISA wie der Host (Services d. BS-Kernels).
* BS-Kernel wird geteilt zwischen Host und Gast.
* Bestandteile:
    * Linux-Namespaces
    * Linux Control Groups
    * Secure Computing Mode

## 11.4 Linux Namespaces
[More info here](http://man7.org/linux/man-pages/man7/namespaces.7.html)

A namespace wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource. Changes to the global resource are visible to other processes that are members of
the namespace, but are invisible to other processes. One use of namespaces is to implement containers.

* Globale Verwaltung von Ressourcen in traditionellen Unix-basierten Systemen. (Isolierung von Ressourcen)
* Namespaces als Grundlage für **Container** zur Abstraktion auf globale Ressourcen des BS.
* 6 Namespace-Typen
    * **UTS-Namespaces**
    * **IPC-Namespaces**
    * **Network Namespaces**
    * **PID Namespaces**
    * **Mount Namespaces**
    * **User Namespaces**

### UTS Namespaces
* UTS := Unix Time Sharing
* Isolation von System Identifiers.

### PID Namespaces
* Isolation von PID-Räumen.
* Eindeutige PID im PID-Namespace.
* Einfache Migration von Containern auf andere Systeme, ohne dass sich die PIDs der Prozesse ändern.
* PID Namespace `init` Prozess
    * Initialisierung des PID Namespaces
    * Vater aller verwaisten Prozesse im PID Namespace
* Prozesse sind nur innerhalb d. selben PID Namespaces und als Child Namespaces sichtbar.
* Nested (verschachtelte) PID Namespaces sind möglich.

### IPC und Network Namespaces
* IPC Namespaces isolieren IPC Ressourcen.
* Network Namespaces isolieren Netzwerk Ressourcen.

### Mount Namespaces
* Mount Namespaces isolieren Mount-Points des Filesystems.

## 11.5 Linux Control Groups (CGroups)
Das `memory` Cgroup Subsystem
* Unterteilung von Prozessen in hierarchische Gruppen
    * Allokierung und Verteilung von Systemressourcen unter Prozessgruppen
    * Repräsentation von Systemressourcen durch unabhängige Cgroups Subsysteme
* Accounting
* Kontrolle
    * Weiche Memory Grenzen einer Prozessgruppe: Pages können wieder zurückgegeben werden
    * Harte Memory Grenzen einer Prozessgruppe: 
        * Trigger eines Out-of-Memory Killers bei überschreitung einer Grenze
        * Einfrieren v. Prozessen einer Gruppe
        * Anpassung von Memory-Grenzen oder Terminierung von Prozessen
        * Fortsetzung von eingefrorenen Prozessen
    * Einsatz von Memory Grenzen für Kernel oder Physischen Speicher

## 11.6 Secure Computing Mode
Mechanismus im Linux Kernel zur Einschränkung von Systemcalls für Prozesse.
### Modi
* **Strict Mode** `SECCOMP_MODE_STRICT`
    * Einschränkung d. Prozesses auf `read`, `write`, `exit`, `sigreturn`
    * Alle weiteren Syscalls triggern `SIGKILL`
* **Filter Mode** `DECCOMP_MODE_FILTER`
    * Kontrolle d. erlaubten Syscalls für den Prozess
    * Filter basierend auf BPF
* **Berkeley Packet Filter** BPF
    * Implementiert als VM / Interpreter im Linux Kernel mit sehr kleiner ISA mit festen Instruktionsgrößen
    * Implementiert Filter in Form von kleinen BPF Programmen
    * Ursprünglich: Filtern und Abfangen von ungewollten Paketen bereits im Kernel Space, sodass keine Übergabe in dern User Space erfolgt.
    * Einsatz:
        * Container Securlty
        * System Tracing
        * Netzwerkpaket Filtering
    * Im Secure Computing Mode: 
        * Testen von Systemcalls gegen installierte Filter, welche nicht deaktiviert werden können.
        * Filterung nach Systemcall Nummern und Argument Werten.
