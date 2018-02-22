# 05 Prozesskommunikation
* Breitbandig vs. schmalbandig
* Implizit vs. explizit
* Nachrichtenorientiert vs. speicherorientiert
* Antwortverhalten: Nachricht vs. Auftrag

## 5.2 Kommunikationsformen
* **Schmalbandige Kanäle** zur Meldung von Ereignissen bei wenigen Bits an Information.
* **Breitbandige Kanäle**
    * **implizite Komm.** über *gemeinsame Betriebsmittel* (z.B. Speicher, Register, Dateien, Ringpuffer), aber keine Unterstützung durch das BS.
    * **explizite Komm.** über *dedizierte Interaktion mit dem BS*. wenn die beteiligten Prozesse in disjunkten Adressräumen liegen (message passing).
        * Kopplungsgrad:
            * **synchron** (Synchronisation der Prozesse zur Nachrichtenübertragung, blockierend)
            * **asynchron** (Entkopplung von Sender u. Empfänger, non-blocking)
        * Muster:
            * **Meldung** / Signal (unidirektional): Wenige 
            * **Auftrag** / Request (bidirektional): Interaktion zwischen Sender u. Empfänger

## 5.3 Nachrichtenbasierte Kommunikation
* **Stream** (Strom)
    * Buffering der Nachricht auf dem Kommunikationsweg.
    * Vereinigung der versendeten Nachrichten als Bytestrom.
    * Abstraktion nachrichtenbasierter Verbindung, Verdeckung von Nachrichtengrenzen durch Übertragung als logischen Bytestream.
* **Pipes**
    * Zur Realisierung von Streams.
    * *Unidirektionaler* Stream zwischen zwei Kommunikationspartnern.
    * Unterstützte Operationen:
        * `open pipe`
        * `read`
        * `write`
* **Ports**
    * Logische Abstraktion von *Kommunikationsendpunkten*.
    * Ein Port ist zu einer gegebenen Zeit eindeutig einem Prozess zuzuordnen. 
    * Zustände:
        * offen
        * geschlosen
        * blockiert
    * Ports entsprechen FIFO-Warteschlangen.

Zu **verbindungsorientierter** Kommunikation muss ein logischer Kanal zwischen den Endpunkten aufgebaut werden!
* **Socket** (Kanal)
    * Bidirektionaler, strombasierter, Kommunikationkanal zwischen zwei Endpunkten.
    * Bidirektional, kann aber einseitig geschlossen werden. 
    * Zuordnung von zwei Endpunkten, aber auch *broadcast* oder *multicast*
    * *verbindungsorientiert*, *strombasierte* Kommunikation

## 5.4 Berkeley Sockets
* Basisoperationen:
    * `socket()`: Baue einen neuen Socket auf.
    * `bind()`: Assoziiere einen Socket mit einem Port (Server).
    * `listen()`: Warte auf eintreffende Daten (Server).
    * `accept()`: Akzeptiere Verbindungswünsche von Clients (Server).
    * `connect()`: Initiiere Verbindung zum Server (Client).
    * `send()`: Sende Daten (Alternativ: `read()`).
    * `recv()`: Empfange Daten (Alternativ: `write()`).
    * `close()`: Beende die Verbindung einseitig.
* Ablauf aus Server-Sicht:
    1. `socket()`
    2. `bind()`
    3. `listen()`
    4. `accept()`
    5. `close()`
* Ablauf aus Client-Sicht:
    1. `socket()`
    2. `connect()`
    3. `close()`

## 5.5 Software Bussysteme
* In Linux: `DBus`
* Kommunikation über einen gemeinsamen Nachrichten-Bus zur Kommunikation mit mehreren Kommunikationspartnern
* Adressierung über **well-known names**.
* Grundlage für Activities bei Android.