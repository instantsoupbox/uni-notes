KAP 3 - Vermittlungsschicht
=====
Die Vermittlungsschicht soll mehrere unterschiedliche Direktverbindungsnetze miteinander koppeln. Ausserdem sollen kleine Gruppen an Geräten in kleinere **Subnetze** gruppiert und strukturiert aufgeteilt werden. Diese Geräte sollen logisch und global eindeutig adressierbar sein und über mehrere Geräte hinweg soll eine Wegwahl möglich sein. 

Die Hauptaufgabe der Vermittlungsschicht (Schicht 3) ist es, Pakete von der Ursprungsmaschine an die Ziel-Maschine zu routen.
***

## Vermittlungsarten
<details>
    <summary>Aufgaben der Vermittlungsschicht</summary>
    <ul>
        <li>Kopplung untersch. Direktverbindungsnetze</li>
        <li>Struktur. Aufteilung in kleinere Subnetze</li>
        <li>logische, global eindeutige Adressierung von Geräten</li>
        <li>Wegwahl zw. Geräten über mehrere Hops hinweg</li>
    </ul>
</details>

<details>
    <summary>Vermittlungsarten</summary>
    <ul>
        <li>Leitungsvermittlung</li>
        <li>Nachrichtenvermittlung</li>
        <li>Paketvermittlung</li>
    </ul>
</details>

### Aufgaben der Vermittlungsschicht
* Kopplung unterschiedlicher Direktverbindungsnetze
* Strukturierte Aufteilung in kleinere **Subnetze**
* Logische, global eindeutige Adressierung von Geräten
* Wegwahl zw. Geräten über mehrere Hops hinweg. (Pfadlänge: Hop-Count)

### Leitungsvermittlung
Reserviere eine dedizierte Leitung zwischen Sender und Empfänger

Phasen während einer verbindungsorientierten Übertragung:
   1. Verbindungsaufbau:
    
      Austausch von **Signalisierungsnachrichten** zum Aufbau einer dedizierten Verbindung zwischen S und E (Wegwahl!)

   2. Datenaustausch: 
   
      Exklusive Nutzung des Kanals durch Kommunikationspartner, keine Adressierung nötig (Punkt-zu-Punkt-Verbindung)

   3. Verbindungsabbau: 
      
      Austausch von **Signalisierungsnachrichten** zum Abbau der Verbindung, Freigabe der Ressourcen für nachfolgende Verbindungen

| Vorteile      | Nachteile     |
| ------------- |---------------|
| Gleichbleibende Güte der dedizierten Verbindung nach dem Verbindungsaufbau     | Ressourcenverschwendung durch exklusive Nutzung |
| Schnelle Datenübertragung      | Komplexer Verbindungsaufbau      |
|  | Hoher Aufbau beim Schalten physikalischer Verbindungen      |

### Nachrichtenvermittlung
Wähle für jede Nachricht individuell einen Weg und speichere Nachrichten in Zwischenknoten als Ganzes (z.B. Email)

Modifikation ggü. Leitungsvermittlung:
Header (Adressinformationen) vor der gesamten Nachricht, Übertragung der ganzen PDU

Asynchrone Kommunikation möglich, Zeitersparnis, da kein Auf-/Abbau der Verbindung nötig. Gemeinsame Nutzung von Teilstrecken (Zeitmultiplex - Time Division Multiplex TDM)

| Vorteile      | Nachteile   |
| ------------- |---------------|
| Flexibles Zeitmultiplex von Nachrichten     | Pufferung von Nachrichten bei Auslastung von (i,j) |
| Bessere Ausnutzung der Kanalkapazität | Nachrichtenverlust durch begrenzten Puffer möglich |
| Keine Verzögerung beim Senden durch Verbindungsaufbau | Mehrfache Serialisierung der ganzen Nachricht |

### Paketvermittlung
Teile eine Nachricht in mehrere kleine Pakete auf und versende jedes Paket unabhängig von den anderen (unterschiedliche Wege möglich). 

Für jedes Paket - einen eigenen Header (Informationen zur Weiterleitung und Reassemblierung) 

#### Multiplexing auf Paketebene
Engpässe werden fairer genutzt, da nur kleine Pakete vermittelt werden. Bei Paketverlust müssen nur Teile einer größeren Nachricht wiederholt werden. 
***

## Adressierung im Internet

### IPv4
Länge: 32 bits (4 Bytes)

Dotted-decimal notation, jedes Byte der Adresse wird mit einem Punkt getrennt

Ein Teil der IP-Adresse wird bestimmt durch das Subnetz. 

Jedes Paket muss mit einer Absender- und Ziel-IP-Adresse (im IP-Header) versehen werden

MAC-Adressen dienen zur Adressierung innerhalb eines Direktverbindungsnetzes und werden beim Routing verändert.

IP-Adressen dienen der End-zu-End-Adressierung zwischen mehreren Direktverbindungsnetzen und werden beim Routing nicht verändert. 

Ein Router muss adressierbar sein und verfügt auf jedem Interface über eine eigene MAC-Adresse, sowie eine eigene IP-ADresse. 

Offene Probleme:
* **Adressauflösung**: Wie kann die MAC-Adresse des Next-Hops bestimmt werden?
* **Routing-Table**: Woher weiss ein Host, dass er ein anderes Netz über einen bestimmten Router erreichen kann? Woher kennt der Router die korrekte Richtung für den Next-Hop?
* **Statisches Routing / Routing-Protokolle**: Wie wird eine Routing-Table erzeugt?
* **Longest-Prefix-Matching, Border Gateway Protocol Path Selection**: Wie ermittelt der R den Weg ans Ziel, wenn er mehrere Wege kennt?
* **Classful Routing, Classless Routing, Subnetting**: Unterteilung der IP-Adresse in Netz- und Hostanteil

#### HEADERFORMAT
Die Länge des IP-Headers muss ein Vielfaches von 4B betragen!

Vor dem Versenden von Daten müssen Daten aus dem Host-Byte Order in Network Byte Order (Big Endian) konvertiert werden, dies muss auch beim Empfang von Daten durchgeführt werden. 

1. Version: 
    
    Verwendete IP-Version

2. IHL := IP-Header Length

    Länge des IP-Headers **in Vielfachen von 32 bit**. Dies ist wichtig, da der IPv4-Header durch Optionsfelder eine variable Länge hat.

3. TOS := Type of Service

    Zur Klassifizierung und Priorisierung von Paketen und bietet die Möglichkeit zur Staukontrolle

4. Total Length 

    Gesamtlänge des IP-Paketsm also `Header + Daten` **in Bytes**. Es gibt eine maximale Paketlänge, sodass keine Fragmentierung notwendig ist. Diese bezeichnet man als MTU (Maximum-Transmission Unit). Diese hängt von Schicht 2/1 ab und beträgt 1500 B für FastEthernet.

5. Identification

    Zufällig gewählter 16 bit langer Wert und dient zur Identifikation zusammengehörtiger Fragmente.

6. Flags

    Aussage über fragmentierbarkeit. 
    * Bit 16: RES - Reserviert und wird auf 0 gesetzt.
    * Bit 17: DF := Don't fragment
    * Bit 18: MF := More Fragments - `1` für "es kommen noch mehr Fragmente", `0` für "nein"/"unfragmentiert"

7. Fragment Offset
    * Absolute Position der Daten in diesem Fragment bezogen auf das unfragmentierte Paket **in ganzzahligen Vielfachen von 8B**
    * **Reassemblieerung der fragmentierten Pakete in der richtigen Reihenfolge** ist zusammen mit Identifier und MF-Bit (s. 6. Flags) möglich.

8. TTL := Time to Live
    * Wird um 1 dekrementiert, wenn ein Router ein IP-Paket weiterleitet.
    * Wenn ein Router ein Paket mit `TTL = 1` bekommt, so wird es verworfen und eine Benachrichtigung wird an den Absender gesendet (ICMP Time Exceeded)
    * Verhinderung von **Routing Loops** (endlos kreisende Pakete)

9. Protocol
    * Identifikation des Protokolls auf Schicht 4 (Transportschicht), das im Payload des IP-Packets enthalten ist. 
    * TCP: `0x06`
    * UDP: `0x11`

10. Header Checksum
    * Schützt nur den IP Header (einfach, geschwindigkeitsoptimiert)
    * Lediglich Fehlererkennung (aber keine Korrektur!)

11. Source Address (IP des Absenders, duh)

12. Destination Address (IP des Empfängers)

13. Options / Padding

#### ADRESSAUFLÖSUNG (Bestimmung der MAC-Adresse des Next-Hops)
**ARP (Address Resolution Protocol)** 
1. **ARP-Request** - (Who has `dest-ip`? Tell `source-ip` at `source-mac`.) gesendet an MAC-Broadcast-Adresse `ff:ff:ff:ff:ff:ff`
2. **ARP-Reply** - (`Dest-ip` is at `dest-mac`.) an MAC-Unicast-Adresse

Das Ergebnis einer Adressauflösung wird i.d.R. im **ARP-Cache** eines Hosts zwischengespeichert, um nicht bei jedem zu versendenen Paket erneut eine Adressauflösung durchführen zu müssen. 

(Genaues Schema: Folie 46)

#### Internet Control Message Protocol (ICMP)
IP-Protokoll unterstützt das Senden von Paketen an Ziel-Hosts. Das Internet Control Message Protocol dient dazu, den Absender über Probleme zu benachrichten, die auf dem Weg zum Empfänger auftreten könnten (z.B. Routing-Loops)

* Benachrichtigung des Absenders über ein Problem: 
    * Routing-Schleife
    * Router kennt kein Weg zum Ziel
    * letzter Router zum Ziel kann die MAC-Adresse des Empfängers nicht auflösen)
* Zusätzliche Möglichkeiten:
    * **Ping**: Zur  Überpfüfung der Erreichbarkeit von Hosts (Host1 an Host2)
        * **ICPM-Echo-Request**: 16 bit Identifier, Sequence Number wird für jeden gesendeten Echo-Request um eins dekrementiert
        * Weiterleitung des Requests durch Router wie jedes IP-Paket
        * **ECPM-Echo-Reply**: Kopie der Daten aus dem Request werden zurückgeschickt
        * Bei fehlgeschlagener Weiterleitung wird eine ICPM-Nachricht mit Fehlercode an Host1 zurückschickt
    * **Redirect**: Umleitung von Paketen

    * **ICMP Time Exceeded**: 
        * TTL-Feld wird bei der Weiterleitung eines Pakets um eins dekrementiert. 
        * Sobald es den Wert 0 erreicht, so wird das betroffene Paket verworfen. Also wenn das Paket mit `TTL = 1` beim R ankommt.
        * Der R generiert ein ICMP Time Exceeded und schickt es an den Absender des verworfenen Pakets zurück. 

    * **Traceroute**
        * TTL wird zu Beginn auf 1 gesetzt
        * Schrittweise Erhöhung des TTL-Felds um 1 entlang des Pfads von Host1 nach Host2
        * Router verwerfen die Pakete schrittweise und senden jeweils ein TTL-Exceeded an Host1 zurück. 
        * Pfad zu Host2 kann anhand der IP-Quelladresse der Fehlernachrichten nachvollzogen werden

#### Dynamic Host Configuration Protocol (DHCP)
Dynamische Zuweisung von IP-Adressen an Hosts durch einen **DHCP-Server**, da man dies nicht per Hand durchführen will.

Die durch den DHCP-Server zugewiesene Adresse ist in ihrer Gültigkeit zeitlich begrenzt. Clients erneuern ihr Lease in regelmäßigen Abständen beim DCHP-Server

Ablauf:
    
    Client                        Server
    |                                  |
     --------- DHCP-Discover --------->
    |                                  |
     <--------- DHCP-Offer ------------
    |                                  |
     --------- DHCP-Request ---------->
    |                                  |
     <-------- DHCP-ACK/NACK ----------
    |                                  |
    v                                  v

1. Client sendet **DHCP-Discover** (Layer 2 Broadcast)
2. DHCP-Server antwortet mit **DHCP-Offer**, bietet dem Client eine IP-Adresse an
3. C antwortet mit **DHCP-Request**, fordert die angebotene Adresse an
4. DHCP-Server antwortet mit **DHCP-ACK**, gibt die angeforderte Adresse (**Lease** mit zeitlicher Begrenzung) zur Nutzung frei, sonst **DHCP-NACK**

In privaten Netzwerken übernimmt oft der Router die Rolle des DHCP-Servers. 

#### ADRESSKLASSEN (Classful Routing)
IP-Adressraum wird in Klassen unterteilt (historisch: fünf Klassen). Grafisch kann dies durch ein Quadrat dargestellt werden. 

Problem: Verschwendung des Adressraums führt zu ineffiziente Aufteilung und Nutzung des Adressraums

#### SUBNETTING (Classless Routing)

Lösung: Unterteilung von IP-Netzen in Subnetzen

32 bit lange Subnetzmaske unterteilt die IP-Adresse in einen **Netzanteil** und einen **Hostanteil**. 

Eine logische 1 in der Subnetzmaske bedeutet Netzanteil, eine logische 0 Hostanteil. 

IP && Subnetzmaske = Netzwerkadresse

**PRÄFIXNOTATION**: Nur die Länge des Netzanteils (Anzahl führender 1en in der Subnetzmaske) wird angegeben


### IPv6
* Die Adressen sind nun `128 bit` lang. 
* Headerformat wurde vereinfacht
* Änderung bei der IP-Fragmentierung
* **Extension-Header** ermöglichst Flexibilität
* **SLAAC** (Stateless Address Autoconfiguration) mittels ICMPv6 (Internet Control Message Protocol)
* **Stateful Autoconfiguration** durch DHCPv6
* **Multicast**-Einsatz, um alle Router in einem Segment zu adressieren

Beispiel und Hinweise zur IPv6-Adress-Notation:

    // Colon-separated hexadecimal notation
    2001:0db8:0000:0000:0001:0000:0000:0001   // unabgekuerzt
    2001: db8:   0:   0:   1:   0:   0:   1   // Schritt 1.
    2001:db8:0:0:1:0:0:1   
    2001:db8  :: 1:0:0:1                      // Schritt 2., 3., 4.
    2001:db8::1:0:0:1                         // Viel kürzer

1. Weglassen von führenden Nullen in den einzelnen Blöcken

2. Höchstens 1 Gruppe konsekutiver 0er-Blöcke können abgekürzt werden!

3. Bei mehreren Möglichkeiten bei 2. wählt man die längste Sequenz von Nullen. Bei mehreren gleichlangen Seq. wählt man die 1. Möglichkeit! Dies haben wir in 2. bereits getan

4. Ein einzelner 0er-Block darf nicht mit :: abgekürzt werden

#### HEADERFORMAT
Die Länge des IPV6-Headers inkl. seiner Extension Header muss immer ein Vielfaches von `8B` sein. 
1. Version

2. Traffic-Class:
    
    Verwendung zur Verkehrspriorisierung / Quality of Service

3. Flow-Label:

    * Zur Erkennung zusammengehöriger Pakete (Flows) auf Schicht 3
    * Gleichbehandlung von zum selben Flow gehörigen Paketen (z.B. Routing über den selben Pfad)

4. Payload Length:
    * Länge der auf den IPv6-Header folgenden Daten **in Vielfachen von `1B`**

5. Next Header:
    * Typ des nächsten Headers:
         * L4-Header: (TCP / UDP)
         * ICMPv6-Header
         * IPv6 Extension Header

6. Hop Limit
    * analog zum TTL-Header
    * ICMPv6 Time Exceeded, falls Wert 0 erreicht wird

7. Source Address

9. Destination Address

#### IPv6 Extension Header
Diese erlauben es, zusätzliche Layer 3 Informationen in einem extra IPv6 Paket anzufügen. 

##### IPv6 FRAGMENTATION HEADERFORMAT
1. Next Header

     Nächster Extension-Header oder verwendetes Layer 4 Protokoll

2. Reserved

     Hat derzeit beim Fragmentation Header keine Bedeutung

3. Fragment Offset

     Offset der fragmentierten L3-SDU in Vielfachen von `8B`

4. MF := More Fragments

     Folgen auf das aktuelle Paket noch mehr Fragmente? Oder handelt es sich um das letzte Fragment gemäß Fragment Offset?

#### Adressierungsarten auf Schicht 3/2
1. **Unicast**
    * Adressierung von Paketen nur an ein einziges Ziel
    * Diese werden von allen anderen Knoten verworfen bzw. nur ans Ziel weitergeleitet
2. **Broadcast** (mittels Broadcast-Adressen)
    * Adressierung von Paketen an alle Stationen im Netzwerk
    * Auf Schicht 3: lediglich auf das lokale Netzsegment begrenzt (Broadcast-Domain)
3. **Multicast** (mittels Multicast-Adressen)
    * Adressierung von Paketen an best. Gruppe von Knoten
    * auf Schicht 2: Multicast v. Switches = Broadcast
    * auf Schicht 3: Spezielle Protokolle, die Multicast-Adressierung auf über das lokale Netzsegment hinaus ermöglicht
4. **Anycast** 
    * Adressierung an eine beliebige Station einer bestimmten Gruppe

#### Multicast

     ff02::1
All Nodes - adressiert werden alle Knoten auf dem lokalen Link

     ff02::2
All Routers - adressiert werden alle Router auf dem lokalen Link

     ff02::1:2
All **DHPC-Agents** - adressiert werden alle Router auf dem lokalen Link.

     ff02::1:ff00:0/104
**Solicited-Node Address** - diese wird im **Neighbor Discovery Protocol** bei der Adressauflösung generiert.

    // Beispiel zur Generierung der Solicited-Node Adresse
    // Praefix
    ff02::1:ff00:0/104

    // Global Unique
    2001:0db8:1ee7:2ea2:0921:2e11:d2c6:938b
                                    ^^ ^^^^
    // Solicited-Node Adresse
    ff02::1:ffc6:938b
              ^^ ^^^^

    // Beispiel zum Mapping von Multicast iPv6 auf MAC-Adressen
    ff02::1:ffc6:938b ---> 33:33:ff:c6:93:8b
            ^^^^ ^^^^            ^^ ^^ ^^ ^^

Die letzten 4 Oktette der Ethnernetadresse werden die letzten 4 Oktette der IPv6 Multicastadresse. 

#### SLAAC Stateless Address Autoconfiguration

Ein Host generiert sich die für ein Interface benötigte link-local IPv6-Adresse wie folgt:
    
     | Prefix     | Subnett Identifier    | Interface Identifier                  |
     | fe80:10    | ::/54                 | OUI      ff:fe      Device Identifier |
                    ^ auf 0 gesetzt!!!      ^ 
                                         !!!Invertiere das 2. Bit des 1. Oktett des OUI!!!!

#### ICMPv6 HEADERFORMAT
1. Type:
      
      Art der ICMP-Nachricht (z.B. Echo, Time Exceeded usw.)

2. Code:

      Type der ICMP-Nachricht (z.B. Response / Request für ICMP-Echo)

3. Checksum

      Fehlererkennende Checksumme (IPv6-Pseudo-Header)

4. Payload

#### Neighbor Discovery Procotocol (NDP)
NDP ist ein Bestandteil von ICMPv6. Dessen Funktionen sind beispielsweise:
* **Neighbor Solicitations / Advertisements**
    * Adressauflösung
    * Duplicate Address Detection
    * Neighbor Unreachability Detection
* **Router Discovery / Advertisements**
    * Automatisches Auffinden von Routern innerhalb des lokalen Netzsegments
    * Adress-Präfix 
    * Parameter-Konfiguration
* **Redirects**: Umleitung zu anderen Gateways

#### NDP-HEADERFORMATE

**Neighbor Solicitation (Request)**
1. ICMPv6 Header: Type und Code
2. Neighbor Discovery Body: Die Target Address ist die Ziel-IPv6-Adresse, zu der die entsprechende MAC-Adresse gesucht wird.
3. Neighbor Discovery Options: Die Source Link Address ist die L2-Adresse des anfragenden Knotens.

**Neighbor Advertisement (Reply)**
1. ICMPv6 Header: Type und Code
2. Neighbor Discovery Body:
    * R := Router-Flag: 1, wenn der antwortende Knoten ein Router ist. 
    * S := Solicited-Flag: 1, wenn das Advertisement infolge einer Solicitation geschickt wird.
    * O := Override Flag: 1, falls das Advertisement eine möglicherweise gecached LL-Adresse beim Empfänger aktualisieren soll
3. Neighbor Discovery Options: Die Target Link Address ist die L2-Adresse, die der anfragende Knoten erfahren wollte.

## Wegwahl (Routing)

Wie entscheidet ein Router, an welchen Next-Hop ein Paket weitergeleitet werden soll?

### STATISCHES ROUTING

#### ROUTING TABLE
Router / Host speichert in der Routing Tabelle:
* Netzadresse eines Ziels
* Länge des Präfixes
* den zugehörigen Next-Hop (Gateway)
* das Interface des Next-Hop
* Kosten bis zum Ziel (bezogen auf EINE Metrik - z.B. Hop-Count, Bandbreite, Verzögerung etc.)

Routen zu entfernten Netzen müssen gelernt werden, also durch statisches oder dynamisches Routing (**Routing Protokolle**).

##### Longest Prefix Matching
Durchsuchung der Routing-Tabelle von längeren Präfixen hin zu kürzeren Präfixen durchsucht. Der erste passende Eintrag liefert das Next-Hop (Gateway) eines Pakets.
1. Zieladresse d. Pakets AND Subnetzmaxen 
2. Vergleiche des Ergebnisses mit dem Eintrag i. d. Spalte "Destination"
3. Bei Übereinstimmung: Bestimmung d. Gateway und des zugehörigen Interfaces
4. Nach Auflösung d. MAC-Adresse d. Gateways (z.B. via ARP) wird das Paket mit einem neuen Ethernet-Header versehen und weitergeleitet

### DYNAMISCHES ROUTING
Router können miteinander mittels **Routing Protokollen** kommunizieren und untereinander Routen austauschen. 

Gruppierung nach ihrer Funktionsweise:
* **Distanz-Vektor-Protokolle**
    * Basis: **Bellman-Ford-Algorithmus**
    * Next-Hop und Entfernung (Kosten) zu einem Ziel sind jeweils dem Router bekannt
    * Austausch von kumulierten Kosten untereinander
    * Keine Informationen über Netzwerktopologie

* **Link-State-Protokolle**
    * Basis: **Dijkstra-Algorithmus**
    * Austausch von Informationen zum genauen Erreichen des Ziels - komplexe Nachbarschaftsbeziehungen und Update-Nachrichten
    * Vollständige Topologieinformationen

#### Routing Information Protocol (RIP)
RIP ist ein einfaches Distanz-Vektor-Protokoll. Die einzige Metrik ist der Hop-Count (Kantengewicht 1 auf allen Kanten). Das Hop-Count-Limit ist 15 - weiter entfernte Ziele sind nicht mehr erreichbar. 

Funktionsweise: 
* Router senden in regelmäßigen Abständen den Inhalt ihrer Routingtabelle an die Multicast-Adresse 224.0.0.9
* Akzeptierung des Updates durch alle Geräte mit dieser Multicast-Adresse
    - Inkrementierung der Kosten der enthaltenen Routen um 1
    - Vergleich der Routen mit bereits vorhandenen Routen aus der Routingtabelle
* Wenn 5 aufeinanderfolgende Updates von einem Nachbarn ausbleiben, so werden ale Routen über diesen Next-Hop aus der Routingtabelle entfernt. 

Lösung zum Verzögerungsproblem: Die maximale Entfernung zwischen zwei Routern in Hops ist die obere Schranke an Nachrichten, die jeder Router senden muss. Die maximale Verzögerung beträgt maximale Entfernung mal 30 s.
* **Triggered Updates**: 
    * Senden eines Updates, sobald ein Router eine Änderung an seiner Routingtabelle vornimmt
    * Reduzierung der Konvergenzzeit durch eine Welle von Updates durch das Netzwerk, was jedoch zu einer hohen Belastung des Netzwerks während der Updates führt

* **Count to Infinity**
    * Ausfall einer Verbindung führt zur Verbreitung von fehlerhaften Routen und der Inkrementierung der Metrik bis hin zum Erreichen des Hop-Count-Limits.
    * Lösungen: 
        * **Split Horizon**: Sende dem Nachbarn, von dem du die Route zu X gelernt hast, keine Route zu X. 
        * **Poison Reverse**: 
        * **Path Vector**: 