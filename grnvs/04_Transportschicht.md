# Transportschicht
<details>
    <summary>Welche drei Hauptaufgaben besitzt die Transportschicht?</summary>
    <ul> 
        <li> Multiplexing </li>
        <li> Transportmechanismen </li>
        <li> Mechanismen zur Stau- und Flusskontrolle </li>
    </ul>
</details>

## Hauptaufgaben
Datenströme sollen für unterschiedliche Anwendungen bzw. Anwendungsinstanzen gemultiplexed werden. Es müssen verbindungslose und verbindungsorientierte Transportmechanismen bereitgestellt werden. Wir brauchen Mechanismen zur Stau- und Flusskontrolle. 

## Hauptaufgabe 1 - Multiplexing
Für unterschiedliche Anwendungen müssen die Datenströme segmentiert werden. Diese Segmente werden in unabhängigen IP-Paketen zum Empfänger geroutet. Die Segmente müssen durch den Empfänger den einzelnen Datenströmen zugeordnet und an die jeweiligen Anwendungen weitergereicht werden. 

## Hauptaufgabe 2 - Transportmechanismen
### Transportmechanismus -  Verbindungslose Übertragung (Best-Effort)
Von Sicht der Transportschicht sind die Segmente voneinander unabhängig, daher gibt es keine Sequenznummern, also keine Garantie der richtigen Reihenfolge und keine Übertragungswiederholung bei verlorenen Paketen, da nicht sichergestellt werden kann, dass Segmente überhaupt dem Empfänger erreichen. Dies wird als **zustandslose Übertragung der Segmente** bezeichnet, da die Segmente komplett unabhängig voneinander versendet und geroutet werden. 
Die Kommunikation ist
* ungesichert
* verbindungsorientiert
* nachrichtenorientiert

#### UDP := User Datagram Protocol
UDP ist eines der beiden am häufigsten verwendeten Transportprotokolle im Internet. Es ist **verbindungslos**, **ungesichert** und **nachrichtenorientiert**. Verbindungslos bedeutet, dass kein Verbindungsaufbau und keine Synchronisation stattfinden. Ungesichert heisst, dass die Pakete nicht zwingend in der richtigen Reihenfolge bzw. ohne Verluste am Empfänger ankommen (**Best-Effort-Prinzip**). 

| Vorteile      | Nachteile   |
| ------------- |---------------|
| Geringer Overhead     | Keine Zusicherung v. Dienstqualität (da beliebig hohe Fehlerrate) |
| Keine Verzögerung      | Out-of-order Auslieferung von Datagrams      |
| Eignung für Echtzeitanwendung | Keine Flusskontrolle (Überforderung von langsamem Empfänger durch schnellen Sender) | 
| Keine Beeinflussung der Datenrate| Keine Staukontrolle (Überlast im Netz führt zu hohen Verlustraten) |

##### UDP HEADERFORMAT
1. Source Port
2. Destination Port
3. Length: Länge von Header und Daten in Vielfachen von Byte
4. Checksum: erstreckt sich über Header und Daten. 

**UDP is a simple protocol and it has some very important uses, such as client- server interactions and multimedia, but for most Internet applications, reliable, sequenced delivery is needed.**

### Transportmechanismus - Verbindungsorientierte Übertragung
Bei Fehlern wird die Übertragung wiederholt. Die korrekte Reihenfolge der Segmente wird garantiert durch lineare Durchnummerierung von Segmenten mittels Sequenznummern im Protokollheader. 

Was ermöglichen Sequenznummern?
* Bestätigung erfolgreich übertragener Sequenzen
* Identifikation fehlender Segmente
* Neuanforderung fehlender Segmente
* Zusammensetzung der Segmente in richtiger Reihenfolge

Challenges: Empfänger und Sender müssen sich synchronisieren und ihre Zustände aufrechterhalten. D.h. die initiale Sequenznummer muss ausgetauscht und die aktuelle Sequenznummer und bereits gesendeten Elemente protokolliert werden. 

**Verbindungsphasen:**
1. Verbindungsaufbau (Handshake)
2. Datenübertragung
3. Verbindungsabbau (Teardown)

Idee: Teile dem Sender mit, wie viele Segmente nach dem letzten bestätigten Segment auf einmal übertragen werden dürfen, ohne dass der Sender auf eine Bestätigung warten muss. 

### Sliding Window Verfahren
Dem Sender wird mitgeteit, wie viele Segmente nach dem letzten bestätigten Segment auf einmal übertragen werden können, ohne dass der Empfänger auf eine Bestätigung warten muss. 

Eine Bestätigung `ACK = m + 1` bestätigt alle Segmente `SEQ <= m` (**kumulative Bestätigung**). Man sieht, dass also z.B. nach `m` immer `m + 1`, also bei **erfolgreicher Übertragung** das nächste erwartete Segment bestätigt wird (**Forward Acknowledgement**). 

Vorteile:
* Effiziente Nutzung der Zeit zwischen dem Absenden eines Segments und dem Warten auf eine Bestätigung
* Flusskontrolle durch die Aushandlung der Fenstergröße
* Staukontrolle durch algorithmische Anpassung der Fenstergröße, sodass die Datenrate an die verfügbare Datenrate auf dem Übertragungspfad zwischen E und S

Challenges: 
* Zwischen S und E muss mehr Zustand gehalten werden
* Verhinderung von Missverständnissen durch endlichen Sequenznummernraum

Jetzt gibt es zwei Möglichkeiten zum Umgang mit Segment-Verlusten:
* Go-Back-N:

    Es wird **nur die nächste erwartete Sequenznummer** akzeptiert und alle anderen Segmente verworfen.

* Selective-Repeat: 

    Alle Segmentnummern, die in das aktuelle Empfangsfenster fallen, werden akzeptiert. Sie müssen also gepuffert werden, bis die fehlenden Segmente erneut übertragen werden (**Empfangspuffer**). 



## Hauptaufgabe 3 - Mechanismen zur Stau- und Flusskontrolle

#### Staukontrolle / Congestion control
* Reaktion auf drohende Überlast im Netz

* If the transport entities on many machines send too many packets into the network too quickly, the network will become congested, with performance degraded as *packets are delayed and lost*. Controlling congestion to avoid this problem is the combined responsibility of the network and transport layers.


#### Flusskontrolle / Flow control
* Laststeuerung durch den Empfänger

* Beim Empfänger sollen Überlastsituationen vermieden werden durch Vorgabe einer maximalen Größe des Sendefensters für den S durch den E.

#### Transmission Control Protocol (TCP)
Verwendete Verfahren: 
* Sliding Window, Selective-Repeat zur gesicherten, stromorientierten Übertragung
* Mechanismen zur Stau- und Flusskontrolle

##### TCP HEADERFORMAT

1. **Quellport**

2. **Zielport**

3. **Sequenznummer**

4. **Bestätigungsnummer**: Bestätigung einzelner Bytes anstatt ganzer Segmente (stromorientierte Übertragung)

5. **(Data) Offset**: Länge des TCP-Headers in Vielfachen von `4B`

6. **Reserved**: Auf 0 gesetzt, sodass künftige TCP-Versionen dies nutzen können.

7. **Flags** 

    * URG: Die Daten im aktuellen TCP-Segment werden beginnend mit dem ersten Byte bis zur Stelle des Urgen Pointers SOFORT an höhere Schichten weitergeleitet.

    * ACK: 1, falls Empfangsbestätigung. **Piggy backing**, wenn gleichzeitig Nutzdaten von A nach B übertragen und von B nach A gesendete Segmente bestätigt werden.

    * PSH: 1, falls sende- und empfangsseitige Puffer des TCP-Stacks umgangen werden.

    * RST: Abbruch einer TCP-Verbindung ohne ordnungsgemässen Verbindungsabbau

    * SYN: 1, falls **das Segment zum Verbindungsaufbau gehört**, inkrementiert Sequence und Acknowledgement Nummern

    * FIN: 1, falls das Segment zum Verbindungsabbau gehört, inkrementiert Sequence und Acknowledgment Numbers, obwohl keine Nutzdaten transportiert werden. 

14. **Window**: Grösse des aktuellen empfangsfensters **in Byte**, sodass der Empfänger die Datenrate des Senders drosseln kann.

15. **Checksum** über Header und Daten

16. **Urgent Pointer** (selten verwendet): Angabe des Endes der Urgent-Daten, werden sofort an höhere Schichten weitergereicht, falls `URG = 1`

17. **Options**: Window-Scaling, Angabe der Maximum Segment Size (MSS) 

* **MSS** (Maximum Segment Size) gibt die maximale Größe eines TCP-Segments (Nutzdaten ohne Header) an, sodass die MTU (s. u.) nicht überschritten wird. 
* MSS bei Fast-Ethernet: 1500 Byte = 20 B IP-Header + 20 B TCP-Header --> sinnvolle MSS beträgt 1460 B
* MTU (Maximum Transmission Unit) hingegen gibt die maximale Größe der Nutzdaten aus Sicht von Schicht 2 an (IP-Paket inkl. IP-Header), also der L3-PDU. 

##### TCP-Flusskontrolle
* Vermeidung von Überlastsituationen beim Empfänger
* Aktuelle Grösse des Empfangsfensters wird über das Feld *Receive Window* angegeben und bedeutet die maximale Anzahl an Bytes, die ohne Abwarten einer Bestätigung übertragen werden dürfen.
* Die Übertragungsrate des Senders kann durch Herabsetzen des Wertes gedrosselt werden. 

##### TCP-Staukontrolle
* Vermeidung von Überlastsituationen im Netz
* **Congestion Window** W_C (= Staukontrollfenster) mit Grösse w_c
    * Solange Daten verlustfrei übertragen werden: w_c >>
    * Wenn Verluste auftreten: w_c <<
    * Grösse des tatsächlichen Sendefensters: `w_s = min{w_c, w_r}`

Die 2 Phasen der Staukontrolle:
1. **Slow-Start**: Vergrößerung um eine MSS pro bestätigtes Segment führt zu exponentiellem Wachstum des Congestion Windows bis hin zum **Congestion Threshold**. Dann: Congestion-Avoidance-Phase
2. **Congestion Avoidance**: Lineares Wachstum des Staukontrollfensters in der RTT, 

##### TCP-Fazits
Protokollmechanismen zur Fehlerkontrolle wurden im Hinblick auf Überlast im Netz entwickelt. Der Verlust von Paketen wird von TCP immer als Folge einer Überlastsituation **congestion** interpretiert. In der Folge reduziert TCP die Datenrate. TCP kann überfordert sein durch Paketverluste, die nicht auf Überlastsituation, sondern auf Bitfehler zurückzuführen sind. Bereits bei 1 % Paketverlust, der nicht auf Überlast zurückzuführen ist, ist TCP überfordert. 



**Also müssen Layer 1 - 3 (L1: Physikalisch, L2: Sicherung, L3: Vermittlung) für TCP eine ausreichend geringe Paketfehlerrate gewährleisten.**

## NAT := Network Address Translation

Damit bezeichnet man Techniken, die es ermöglichen, N private (nicht global eindeutige) auf M globale (weltweit eindeutige IP-Adressen abzubilden). 

     Privat                    Global
        N      --- NAT --->       M

* `N <= M`: statisch / dynamische Übersetzung
* `N > M`: 1 öffentliche IP-Adresse wird von mehreren Computern gleichzeitig genutzt. Eine eindeutige Unterscheidung geschieht mittels Port-Multiplexing. Häufigster Fall ist `M = 1`, z.B. bei einem privaten DSL-Anschluss. 

Der SrcPort wird vom Absender üblicherweise im Bereich `[1024, 65535]` gewählt (**Ephemeral Ports**, kurzlebige Ports, die wieder freigegeben werden, sobald die Verbindung terminiert). 

Bei der NAT tauscht der NAT-Router die Absenderadresse des Pakets durch seine eigene globale Adresse aus und erzeugt einen neuen Eintrag in seiner NAT-Rabelle, um seine Änderungen an dem Paket zu dokumentieren. 

Der Antwortendende weiss i.d.R. nichts von der NAT und hält den NAT-Router Für den Sender. 

Falls ein anderer Sender S2 zufällig einen existierenden Quellport wählt, also den gleichen wie der des Senders S1, der vorher schon einmal gesendet hat, so wird für den globalen Port von S2 ein zufälliger Wert gewählt, wie beispielsweise `S1_Port + 1`. 

### Private IP-Adressen
Private Adressbereiche: 

     10.0.0.0/ 8
     172.16.0.0/18
     169.254.0.0/16
     192.168.0.0/16

Automatische Adressvergabe (Autonatic Private IP Addressing)

Router übernehmen die Aufgabe der Netzwerkadressübersetzung. 

Für die Adressauflösung muss eigentlich die Globale IP des Empfängers bekannt sein. Wenn dies aber ein privater Adressbereich ist, muss der jeweilige Router NAT bereitstellen, damit die private IP genatted werden kann.

