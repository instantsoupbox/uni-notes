# Transportschicht
<details>
    <summary>Welche drei Hauptaufgaben besitzt die Transportschicht?</summary>
    Multiplexing, Transportmechanismen, Mechanismen zur Stau- und Flusskontrolle
</details>

## Hauptaufgabe
Datenströmen sollen für unterschiedliche Anwendungen bzw. Anwendungsinstanzen gemultiplexed werden.

### Multiplexing
* Für unterschiedliche Anwendungen müssen die Datenströme segmentiert werden
* Diese Segmente werden in unabhängigen IP-Paketen zum Empfänger geroutet werden.
* Die Segmente müssen durch den Empfänger den einzelnen Datenströmen zugeordnet und an die jeweiligen Anwendungen weitergereicht werden. 

### Transportmechanismen

#### Verbindungsorientiert
Bei Fehlern wird die Übertragung wiederholt. Die korrekte Reihenfolge der Segmente wird garantiert.
* Sliding-Window

#### Verbindungslos (Best-Effort)
Von Sicht der Transportschicht sind die Segmente voneinander unabhängig, daher gibt es keine Sequenznummern, keine Übertragungswiederholung und keine Garantie der richtigen Reihenfolge. 

### Mechanismen zur Stau- und Flusskontrolle

#### Staukontrolle / Congestion control
* Reaktion auf drohende Überlast im Netz

* If the transport entities on many machines send too many packets into the network too quickly, the network will become congested, with performance degraded as *packets are delayed and lost*. Controlling congestion to avoid this problem is the combined responsibility of the network and transport layers.


#### Flusskontrolle / Flow control
* Laststeuerung durch den Empfänger
* Beim Empfänger sollen Überlastsituationen vermieden werden durch Vorgabe einer maximalen Größe für das Sendefenster für den S durch den E.

### Verbindungslose Übertragung
Man spricht von ungesicherter, verbindungsloser und  nachrichtenorientierter Kommunikation, da die Segmente unabhängig voneinander und aus Sicht der Transportschicht zustandslos verschickt werden. 

#### User Datagram Protocol UDP
Vorteile: 
* Geringer Overhead
* Keine Verzögerung durch Verbindungsaufbau, Retransmits und Reordering von Segmenten
* Sofern Paketverlust in Kauf genommen werden kann, ist dieses Verfahren gut geeignet für Echtzeitanwendungen
* Die Datenrate wird nicht durch Fluss- und Staukontrollmechanismen gestört

Nachteile:
* Fehlerrate kann beliebig hoch werden
* Ankunft der Datagrams out-of-order möglich
* Keine Fluskontrolle
* Keine Staukontrollmechanismen
UDP is a simple protocol and it has some very important uses, such as client- server interactions and multimedia, but for most Internet applications, reliable, sequenced delivery is needed.

### Verbindungsorientierte Übertragung
Die Segmente werden im Protokollheader mittels Segmentnummern linear durchnummeriert. 
Was ermöglichen Sequenznummern?
* Erfolgreich übertragene Sequenzen können bestägt werden
* Fehlende Segmente können identifiziert werden
* und können neu angefordert werden
* Die Segmente können in der richtigen Reihenfolge wieder zusammengesetzt werden

Challenges: Empfänger und Sender müssen sich synchronisieren und ihre Zustände aufrechterhalten. D.h. die initiale Sequenznummer muss ausgetauscht werden und die aktuelle Sequenznummer und bereits gesendeten Elemente protokolliert werden. 

Verbindungsphasen:
1. Verbindungsaufbau (Handshake)
2. Datenübertragung
3. Verbindungsabbau (Teardown)

### Sliding Window Verfahren
Dem Sender wird mitgeteit, wie viele Segmente nach dem letzten bestätigten Segment auf einmaml übertragen werden können, ohne dass der Empfänger auf eine Bestätigung warten muss. 

Vorteile:
* Effiziente Nutzung der Zeit zwischen dem Absenden eines Segments und dem Warten auf eine Bestätigung
* Flusskontrolle durch die Aushandlung der Fenstergröße
* Staukontrolle durch algorithmische Anpassung der Fenstergröße, sodass die Datenrate an die verfügbare Datenrate auf dem Übertragungspfad zwischen E und S

Challenges: 
* Zwischen S und E muss mehr Zustand gehalten werden
* Verhinderung von Missverständnissen durch endlichen Sequenznummernraum

Umgang mit Segment-Verlusten:
* Go-Back-N: Es wird nur die nächste erwartete Sequenznummer erwartet, Verwerfung aller anderer Segmente
* Selective-Repeat: Alle Segmentnummern, die in das aktuelle Empfangsfenster fallen, werden akzeptiert. Bis die fehlenden Segmente erneut übertragen werden können, müssen diese gepuffert werden (Empfangspuffer). 

#### Transmission Control Protocol (TCP)
Verwendete Verfahren: 
* Sliding Window, Selective-Repeat zur gesicherten, stromorientierten Übertragung
* Mechanismen zur Stau- und Flusskontrolle

(TCP-Header)

* *MSS* gibt die maximale Größe eines TCP-Segments (Nutzdaten ohne Header) an
* MTU gibt die maximale Größe der Nutzdaten aus Sicht von Schicht 2 an (einschließlich des IP-Headers)

##### TCP-Flusskontrolle
* Vermeidung von Überlastsituationen beim Empfänger
* Aktuelle Grösse des Empfangsfensters wird über das Feld *Receive Window* angegeben und bedeutet die maximale Anzahl an Bytes, die ohne Abwarten einer Bestätigung übertragen werden dürfen.
* Die Übertragungsrate des Senders kann durch Herabsetzen des Wertes gedrosselt werden. 

##### TCP-Staukontrolle
* Vermeidung von Überlastsituationen im Netz
* *Congestion Window* (= Staukontrollfenster)
Die 2 Phasen der Staukontrolle:
1. Slow-Start: Vergrößerung um eine MSS pro bestätigtes Segment führt zu exponentiellem Wachstum des Congestion Windows bis hin zum Congestion Threshold. Dann: Congestion-Avoidance-Phase
2. Congestion Avoidance: Lineares Wachstum des Staukontrollfensters in der RTT, 

Folie 4 - 38
