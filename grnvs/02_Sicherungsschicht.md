KAP 2 - Sicherungsschicht
=====
<details>
    <summary>Modell des Direktverbindungsnetzes</summary>
    Alle angeschlossenen Knoten sind direkt erreichbar, Identifizierung mit einfachen Adressen der Schicht 2, keine Vermittlung, aber einfache Weiterleitung in Form von Bridging oder Switching
</details>

<details>
    <summary>Aufgaben der Sicherungsschicht</summary>
    Steuerung des Medienzugriffs, Prüfung übertragener Nachrichten auf Fehler und die Adressierung innerhalb von Direktverbindungsnetzen</br>
</details>

#### Steuerung des Medienzugriffs
Hubs, alle angeschlossenen Conputer werden zu einem Bus verbunden, gleichzeitiges Senden führt zu Kollisionen (Nachrichtenverlust)

#### 

### Darstellung von Netzwerken als Graphen

#### Netztopologien
Es gibt die physikal. und die logische Topologie. 

Wichtige Netztopologien:
* Punkt-zu-Punkt
* Kette
* Stern
* Vermaschung
* Baum
* Bus

### Verbindungscharakterisierung, Mehrfachzugriff, Medienzugriffskontrolle

<details>
    <summary>Übertragungsrate</summary>
    Bestimmung der notwendige Zeit, um L Datenbits auf ein Übertragungsmedium zu legen. (Serialisierungszeit / Serialization Delay / Transmission Delay) 
</details>

Verbindungscharakterisierung anhand:
* Übertragungsrate
* Übertragungsverzögerung
* Übertragungsrichtung
* Mehrfachzugriff (Multiplexing) 

Die **Übertragungsrate** bestimmt die Serialisierungszeit/Übertragungsverzögerung (Serialization/Transmission Delay)

Die **Ausbreitungssverzögerung** (Propagation Delay) wird bestimmt von der endlichen Ausbreitungsgeschwindigkeit von Signalen, welche relativ zur Lichtgeschwindigkeit im Vakuum angegeben wird. 

**Bandbreitenverzögerungsprodukt**: Speicherkapazität durch die endliche Ausbreitungsverzögerung, Anzahl an Bits (Kapazität), die sich gleichzeitig auf der Leitung befinden können. 

Charakterisierung der **Übertragungsrichtung**
* Simplex
* Halbduplex
* Vollduplex

Die Art der Verbindung hängt dabei ab von der Fähigkeit des Übertragungskanals, dem Medienzugriffsverfahren und den Anforderungen der Kommunikationspartnern ab.

#### Multiplexverfahren

* Zeitmultiplex TDM
* Frequenzmultiplex FDM (Kanal wird in unterschiedliche Frequenzbänder aufgeteilt)
* Raummultiplex SDM (mehrere parallele Übertragungskanaläle)
* Codemultiplex CDM (orthogonale Alphabete, Zuweisung der Alphabete an die Kommunikationspartner)

#### Medienzugriffsverfahren
****
**TDMA:** 
* synchron (burst-artiger Datenverkehr)
* asynchron (zufälliger, konkurrierender / dynamisch geregelter MA)

**Asynchrone TDMA:** Konkurrierender Zugriff / Geregelter Zugriff

**Konkurrierender Zugriff:** Random Access (ALOHA), CSMA/CD (Ethernet), CSMA/CA (WLAN) 

**Geregelter Zugriff:** Token Passing / CSMA/CA + RTS/CTS

Bewertungskriterien für Medienzugriffsverfahren: 
* Durchsatz
* Verzögerung
* Fairness
* Implementierungsaufwand

#### ALOHA

Stationen (= Nutzer) dürfen Daten senden, sobald diese vorliegen. Stationen senden an eine zentrale Station, gleichzeitiges Senden führt zu Kollisionen. Erfolgreiche Übertragungen werden Out-of-Band quittiert. 

18% Effizienz

#### SLOTTED-ALOHA
37% Effizienz


#### CSMA, CSMA/CD, CSMA/CA
Verbesserung von Slotted ALOHA: "Listen Before Talk"

3 Varianten: 
1. **1-persistentes CSMA**
    * Wenn M. frei, so beginne Übertragung
    * Wenn M. belegt, warte bis frei und beginne dann die Übertragung
2. **nicht-persistentes CSMA**
    * Wenn M. frei, so beginne Übertragung
    * Wenn M. belegt, so warte eine zufällig gewählte Zeitspanne, dann Übertragung
3. **p-persistentes CSMA**
    * Wenn M. frei, übertrage mit W'keit p oder verzögere mit 1-p um feste Zeit, dann Übertragung
    * Wenn M. belegt, warte bis frei, dann Übertragung

****
##### CSMA/CD
Collision detection: 
* Erkenne Kollisionen, Wiederholung d. Übertragung bei Erkennung
* Keine Sendebestätigung
* Keine Kollision bedeutet erfolgreiche Übertragung    

**Binary Exponential Backoff**, um zu verhindern, dass nach dem JAM-Signal beide Station gleichzeitig die Übertragung wiederholen wollen. 

***
##### CSMA/CA
Collision avoidance: basiert auf p-persistentem CSMA

Prinzip **"Hidden Station"**

### Rahmenbildung, Adressierung und Fehlererkennung
***
<details>
    <summary>Welche Methoden zur Rahmenbegrenzung gibt es?</summary>
    Längenangaben d. Nutzerdaten, Steuerzeichen (Start, Ende), Begrenzungsfelder und Bit-Stopfen, Coderegelverletzung
</details>

In Schicht 1: Nachricht ist lediglich Abfolge von Bits. 

Jetzt: Betrachtung auf der Sicherungsschicht.

Nachricht = Frame / Rahmen

Methoden zur **Rahmenbegrenzung** 
* **Längenangaben d. Nutzdaten**
   * Länge der nachfolgenden Nutzdaten am Anfang des rahmens
   * Längenfeld muss eindeutig erkennbar sein!
* **Steuerzeichen**  (Start / Ende)
* **Begrenzungsfelder und "Bit-Stopfen"** (dadurch wird sichergestellt, dass die Markierung nicht zufällig in den Nutzdaten vorkommt)
* **Coderegelverletzung** (Auslassung bestimmter Signalwechsel, um ein ungültiges Symbol zu erzeugen, sodass den Start und das Ende eines Rahmens markiert wird)

Ziel: Erhaltung der **Codetransparenz** - Ermöglichung der Übertragung beliebiger Zeichenfolgen

### Adressierung und Fehlererkennung
Wir halten uns an IEEE 802-Standards

#### ADRESSIERUNG

Zunächst: Adressierung in **Direktverbindungsnetzen** (keine Vermittlung zwischen Knoten (Routing))

**MAC-Adressen** (Media Access Control): Adressen auf Schicht 2, sie sind 6 Bytes lang (für Ethernet und 802.11 WLAN)

Anforderungen:
* Eindeutige Identifizierung der Knoten innerhalb des Direktverbindungsnetzes
* **Broadcast-Adresse**, welche alle Knoten im Direktverbindungsnetz anspricht
* **Multicast-Adresse**, welche einebestimmte Gruppe von Knoten anspricht

#### FEHLERERKENNUNG
I. d. R. dient die Prüfsumme eines Schicht-2-Protokolls nicht der Fehlerkorrektur, sondern der **Fehlererkennung**. Dabei soll die **Weiterleitung eines fehlerhaften Payloads** an höhere Schichten verhindert werden. 

**Fehlererkennende Codes / Prüfsummen / Checksum** 

##### Cyclic Redundancy Check CRC

Folgende Fehler werden erkannt:
* 1-bit Fehler
* isolierte 2-bit Fehler
* Burst-Fehler, die länger als N sind
* alle Burst-Fehler, deren Länge kleiner ist als N
* Fehlermuster mit ungerader Anzahl an Fehlerbits

Folgende Fehler werden nicht erkannt:
* Fehler, die länger als N sind
* Fehler bestehend aus mehreren Bursts
* Fehler, die ein Vielfaches des Reduktionspolynoms sind


### Verbindung auf Schicht 1 und 2

#### HUBS (Physikalische Schicht)
* **Aktive Hubs (Repeater):** Signalverstärker auf der physikalischen Schicht, ohne Fehler-/Adressprüfung
* **Passive Hubs:** Sternverteiler

#### SWITCHES (Sicherungsschicht)
Switches verbinden verschiedene Gruppen an Hosts, die über Hubs miteinander verbunden sind.

Switches mit zwei Ports nennt man auch **Bridge**
1. Learning Phase: Hub mit zwei Ports (Switch merkt sich, über welchen Port ein Rahmen empfangen wurde)
2. MAC-Adressen-Zuordnung zu den Ports 0 und 1 in einer **Switching-Table**
3. Falls MAC in Switching-Table nicht vorhanden, wird das Frame an alle Teilnehmer geschickt

Eine **Kollisionsdomäne** umfasst alle Netzwerkgeräte, die um den Zugriff auf ein gemeinsames Übertragungsmedium konkurrieren.
Dies ist ein Teil eines Direktverbindungsnetzes, innerhalb dessen eine Kollision bei gleichzeitiger Übertragung mehrerer Knoten auftreten kann. Dieser wird häufig auch als **Segment** bezeichnet.

Kollisionsdomänen werden durch Switches bzw. Bridges unterbrochen (**Segmentierung**). 

**Microsegmentation**: Vollständig geswitchtes Netz (Pro Switchport ist genau ein Host angeschlossen).