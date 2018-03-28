# 05 Prozesskommunikation
* Breitbandig vs. schmalbandig
* Implizit vs. explizit
* Nachrichtenorientiert vs. speicherorientiert
* Antwortverhalten: Nachricht vs. Auftrag

## 5.2 Kommunikationsformen

### **Schmalbandige Kanäle** 
Zur Meldung von Ereignissen bei wenigen Bits an Information.

### **Breitbandige Kanäle**
Übertragung großer Datenmengen.

* **implizite Komm.** über *gemeinsame Betriebsmittel* (z.B. Speicher, Register, Dateien, Ringpuffer), aber keine Unterstützung durch das BS.
* **explizite Komm.** über *dedizierte Interaktion mit dem BS*, wenn die beteiligten Prozesse in disjunkten Adressräumen liegen (**message passing**).

* Kopplungsgrad:
    * **synchron**: 
        * Synchronisation beider Prozesse zur Nachrichtenübertragung, blockierend
        * Empfänger sendet nach Erhalt der Nachricht eine Bestätigung (Synchronisation)
        * Sender blockiert bis zum Erhalt einer Empfangsbestätigung
    * **asynchron**:
        * Entkopplung von Sender u. Empfänger, non-blocking
        * Nützlich für Echtzeit-Anwendungen, falls ein sendender Prozess nicht blockiert werden darf.

* Muster:
    * **Meldung** / Signal (unidirektional): Wenige Daten werden übertragen, aber i.d.R. ein `ACK` erforderlich.
        * synchron: Sender wartet auf Empfangsbestätigung (und blockiert), `ACK` nur zur Synchronisation.
        * asynchron: **Nachrichtendienst** (BS) puffert die Nachricht, E empfängt die Nachricht mit `recv` und blockiert, bis Nachricht empfangen wird.
    * **Auftrag** / Request (bidirektional): Interaktion zwischen Sender u. Empfänger.
        * synchron: Transaktion bestehend aus Nachrichtenbearbeitung und Senden d. Resultats (z.B. Aufruf einer Website).
        * asynchron: Auftrag und Resultat als unabhängige Meldungen, `send` und `recv` entkoppelt.

Asynchrones Senden nützlich für Realzeitanwendungen, zur Signalisierung von Ereignissen, parallele Abarbeitung durch Sender und Empfänger möglich. *BUT*: Verwaltungsoverhead im Zuge des Nachrichtenpuffers des BS, schwierige Fehlerbehandlung durch Entkoppelung von S und E. 

Producer-Consumer-Problem kann auch durch synchronem Message Passing gelöst werden, sodass Semaphore überflüssig ist, denn es muss kein gemeinsamer Speicherbereich synchronisiert werden.

## 5.3 Nachrichtenbasierte Kommunikation

### **Stream** (Strom)

* Buffering der Nachricht auf dem Kommunikationsweg.
* Vereinigung der versendeten Nachrichten als Bytestrom.
* Abstraktion nachrichtenbasierter Verbindung, Verdeckung von Nachrichtengrenzen durch Übertragung als logischen Bytestream.

### **Pipes**

* Zur Realisierung von Streams.
* *Unidirektionaler* Stream zwischen zwei Kommunikationspartnern.
* Unterstützte Operationen:
    * `open pipe`
    * `read`
    * `write`

### **Ports**

* Logische Abstraktion von *Kommunikationsendpunkten*.
* Ein Port ist zu einer gegebenen Zeit eindeutig einem Prozess zuzuordnen. 
* Zustände:
    * offen
    * geschlosen
    * blockiert
* Ports entsprechen **FIFO-Warteschlangen**.
* **Well-known-ports**: 0-1023
    * `21`: FTP
    * `22`: SSH
    * `25`: SMTP
    * `80`: HTTP
    * `443`: HTTPS
    * `465`: SMTPS
    * `993`: IMAPS

Zu **verbindungsorientierter** Kommunikation muss ein logischer Kanal zwischen den Endpunkten aufgebaut werden!

### **Socket** (Kanal)

* Bidirektionaler, strombasierter, Kommunikationkanal zwischen zwei Endpunkten, der einseitig geschlossen werden kann.
* Zuordnung von zwei Endpunkten, aber auch *broadcast* oder *multicast*.
* *verbindungsorientiert*, *strombasierte* Kommunikation.

## 5.4 Berkeley Sockets
### Basisoperationen:
* `socket()`: Baue einen neuen (zunächst ungebundenen) Socket auf.
* `bind()`: Assoziiere einen Socket mit einem Port (Server).
* `listen()`: Warte auf eintreffende Daten (Server).
* `accept()`: Akzeptiere Verbindungswünsche von Clients (Server).
* `connect()`: Initiiere Verbindung zum Server (Client).
* `send()`: Sende Daten (Alternativ: `read()`).
* `recv()`: Empfange Daten (Alternativ: `write()`).
* `close()`: Beende die Verbindung einseitig.

### Ablauf aus Server-Sicht:
Server ist ein dedizierter, langlebiger Prozess, der einen Dienst zur Verfügung stellt. Die Aufträge werden i.d.R. in Threads (Worker Threads) abgearbeitet.
1. `socket()`
2. `bind()`
3. `listen()`: Warten auf Verbindungswünsche.
4. `accept()`
5. `close()`: Serverseitiges Schließen der Verbindung.

### Ablauf aus Client-Sicht:
Der Client initiiert Aufträge an einen vom Server angebotenen Dienst. Clients sind einem Server i.d.R. a-priori nicht bekannt.
1. `socket()`
2. `connect()`: Verbindung mit einem Port des Servers.
3. `close()`

## 5.5 Software Bussysteme
* In Linux: `DBus`
* Kommunikation über einen gemeinsamen Nachrichten-Bus zur Kommunikation mit mehreren Kommunikationspartnern
* Adressierung über **well-known names**.
* Grundlage für Activities bei Android.