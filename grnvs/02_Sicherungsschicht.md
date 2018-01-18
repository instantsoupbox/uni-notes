KAP 2 - Sicherungsschicht
=====
<details>
    <summary>Welche Eigenschaften hat ein Direktverbindungsnetz?</summary>
    <ul>
        <li> Alle angeschlossenen Knoten sind direkt erreichbar </li>
        <li> Identifizierung mit einfachen Adressen der Schicht 2 </li>
        <li> keine Vermittlung, aber einfache Weiterleitung in Form von Bridging oder Switching </li>
    <ul/>
</details>

<details>
    <summary>Welche Aufgaben besitzt die Sicherungsschicht?</summary>
    <ul>
    <li> Steuerung des Medienzugriffs </li>
    <li> Prüfung übertragener Nachrichten auf Fehler </li>
    <li> Adressierung innerhalb von Direktverbindungsnetzen </li>
    </ul>
</details>

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

Die **Übertragungsrate** bestimmt die **Serialisierungszeit** /Übertragungsverzögerung (Serialization/Transmission Delay), also die notwendige Zeit, eine bestimmte Anzahl an Datenbits auf ein Übertragungsmedoim zu legen.

Die **Ausbreitungssverzögerung** (Propagation Delay) wird bestimmt von der endlichen Ausbreitungsgeschwindigkeit von Signalen, welche relativ zur Lichtgeschwindigkeit im Vakuum angegeben wird. 

Das **Bandbreitenverzögerungsprodukt** bezeichnet die Speicherkapazität bedingt durch die endliche Ausbreitungsverzögerung, sprich die Anzahl an Bits (Kapazität), die sich gleichzeitig auf der Leitung befinden können. 

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

### Rahmen und Rahmengrenzen
<details>
    <summary>Welche Methoden zur Rahmenbegrenzung gibt es?</summary>
    <ul>
        <li> Längenangaben d. Nutzerdaten </li>
        <li> Steuerzeichen (Start, Ende) </li> 
        <li> Begrenzungsfelder und Bit-Stopfen </li> 
        <li> Coderegelverletzung </li>
    </ul>
</details>

Auf L1 ist eine Nachricht lediglich eine Abfolge von Bits. Da wir uns nun auf L2 - der Sicherungsschicht befinden, sprechen wir nun von einer Nachricht, bzw. einem Frame oder zu deutsch - einem Rahmen. 

Methoden zur **Rahmenbegrenzung**:
* **Längenangaben d. Nutzdaten**
   * Länge der nachfolgenden Nutzdaten am Anfang des rahmens
   * Längenfeld muss eindeutig erkennbar sein!
* **Steuerzeichen**  (Start / Ende)
* **Begrenzungsfelder und "Bit-Stopfen"** (dadurch wird sichergestellt, dass die Markierung nicht zufällig in den Nutzdaten vorkommt)
* **Coderegelverletzung** (Auslassung bestimmter Signalwechsel, um ein ungültiges Symbol zu erzeugen, sodass den Start und das Ende eines Rahmens markiert wird)

Das Ziel ist die Erhaltung der **Codetransparenz**. Das heisst, es sollen beliebige Möglichkeiten an Zeichenfolgen übertragen und gleichzeitig unterschiedliche Rahmen voneinander abgegrenzt werden können. 

### Adressierung und Fehlererkennung auf L2

#### ADRESSIERUNG MIT MAC-ADRESSEN

In **Direktverbindungsnetzen** sind alle angeschlossene Knoten direkt erreichbar. Es findet keine Vermittlung (**Routing**) zwischen Knoten statt. 

Die Adressierung auf L2 geschieht über **MAC-Adressen** (Media Access Control). Diese sind **6 Bytes** lang und werden durch `:` getrennt in 6 Blöcken angegeben, der Aufbau lautet wie folgt: 

     [B]    0     1     2     3     4     5
         |       OUI       |    Device ID    |

Die ersten drei Byte sind die OUI, welches für **Organizationally Unique Identifier** steht. Darüber kann der Hersteller der Netzwerkkarte identifiziert werden. Die Device ID ist der gerätespezifische Teil der MAC-Adresse. 

Anforderungen:
* Eindeutige Identifizierung der Knoten innerhalb des Direktverbindungsnetzes
* **Broadcast-Adresse** `ff:ff:ff:ff::ff:ff`, welche alle Knoten im Direktverbindungsnetz anspricht
* **Multicast-Adresse**, welche einebestimmte Gruppe von Knoten anspricht

#### FEHLERERKENNUNG
I. d. R. dient die Prüfsumme eines L2-Protokolls nicht der Fehlerkorrektur, sondern der **Fehlererkennung**. Dabei soll die **Weiterleitung eines fehlerhaften Payloads** an höhere Schichten verhindert werden. 

**Fehlererkennende Codes / Prüfsummen / Checksum** 

##### Cyclic Redundancy Check CRC
CRC ist nur ein fehlererkennender Code. Es soll eine grosse Anzahl von Fehlern erkannt werden. Die zugefügte Redundanz soll gering sein. Fehler sollen lediglich erkannt, aber nicht korrigiert werden. 

Die Summe zweier Datenwörter entspricht einer bitweisen `XOR`-Verknüpfung. 

Wenn der Divisionsrest zwischen der eingehenden Nachricht und dem Reduktionspolynom gleich null ist, so ist mit hoher Wahrscheinlichkeit kein Übertragungsfehler aufgetreten. Wenn dieser ungleich 0 ist, so ist mit Sicherheit ein Fehler aufgetreten. 

Folgende Fehler werden erkannt (mit N = `grad(r(x))`):
* 1-bit Fehler
* isolierte 2-bit Fehler
* Burst-Fehler, die länger als `N` sind
* alle Burst-Fehler, deren Länge kleiner ist als `N`
* Fehlermuster mit ungerader Anzahl an Fehlerbits

Folgende Fehler werden nicht erkannt:
* Fehler, die länger als `N` sind
* Fehler bestehend aus mehreren Bursts
* Fehler, die ein Vielfaches des Reduktionspolynoms sind


### Verbindung auf L1 und L2

#### Hubs (Physikalische Schicht)
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

Kollisionsdomänen werden durch Switches bzw. Bridges unterbrochen, man spricht dabei von einer **Segmentierung**. 

Unter **Microsegmentation** versteht man ein vollständig geswitchtes Netz. Das heisst pro Switchport ist genau ein Host angeschlossen.