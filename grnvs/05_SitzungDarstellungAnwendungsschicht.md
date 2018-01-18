# Sitzungsschicht
## Dienste der Sitzungsschicht
Es gibt Verbindungsorientierte oder verbindungslose Dienste.

### Verbindungsorientierte Dienste
* Aufbau einer Verbindung zwischen den Kommunikationspartnern
* Bestehen über die Dauer einzelner Transfers d. Transportschicht hinweg
* Verbindungsaufbau, Datentransfer, Verbindungsabbau

### Verbindungslose Dienste
* Durchreichen der Daten an die Transportschicht
* Kein Verbindungsaufbau und Zustandshaltung zwischen den Kommunikationspartnern

### Session
Eine Verbindung der Sitzungsschicht ist eine *Session*. Eine Session kann mehrere TCP-Verbindungen beinhalten. 
Eine Session beschreibt die Kommunikation zwischen mindestens zwei Teilnehmern mit definiertem Anfang und Ende, sowie die sich daraus ergebende Dauer. Eine Session ermöglicht *Dialogführung für die Darstellungsschicht* (also die dienstnehmende Schicht), indem sie mehrere Transportschicht-Verbindungen verwendet und kontrolliert. 

Angebotene Dienste im *verbindungsorientierten Modus*:
1. Auf-/Abbau von Sessions
2. Datentransfer (normal und beschleunigt - für Alarme und Interrupts)
3. Token-Management zur Koordination der Teilnehmer
4. Synchronisation und Resynchronisation
5. Fehlermeldung, Aktivitätsmanagement
6. Erhalt u. Wiederaufnahme von Sessions nach Verbindungsabbrüchen

#### HTTP Hyper Text Transfer Protocol
Ein Protokoll zum Transfer von Website und Dateien. **Cookies** sind kleine Datenfragmente, welche von einem Webserver (bzw. einem damit verbundenen Anwendungsprozess)an einen Webclient übertragen und dort gespeichert werden können. Dadurch kann eine Sitzung über mehrere Anfragen und Antworten, Interaktionen und TCP-Verbindungen hinweg bestehen bleibt. 

#### TLS Transport Layer Security
TLS ist ein Protokoll zur Authentifizierung und verschlüsselten Übertragung von Daten über einen verbindungsorientierten Transportdienst und ist die Grundlage für beispielsweise HTTPS. 
TLS bietet Authenifizierung, Integritätsschutz und Verschlüsselung. Beim Verbindungsaufbau werden die für die Sitzung notwendigen Kommunikationsparameter, wie Verfahren für Authentisierung &Verschlüsselung, sowie Zertifikate ausgetauscht. Diese Sitzungen können über mehrere TCP-Verbindungen hinweg mithilfe von Session-IDs erhalten bleiben. Zugeordnet zur Sitzungsschicht können TLS-Funktionen zur Verwaltung und Wiederaufnahme von Sessions, zur Darstellungsschicht hingegen können die Verschlüsselungsfunktionen zugeordnet werden. 

# Darstellungsschicht
Die Aufgabe dieser Schicht ist es, den Kommunikationspartnern eine einheitliche Interpretation der Daten, d.h. die Übertragung der Daten in einem einheitlichen Format zu ermöglichen. 
* Darstellung der Daten (Syntax)
* Datenstrukturen zur Übertragung der Daten
* Darstellung der Aktionen an diesen Datenstrukturen
* Datentransformation
* Syntaxunabhängige Kommunikation der Anwendungen untereinander
* Umwandlung d. anwendungsspezifischen Syntax in eine einheitliche Form zur Übertragung

Konkrete Funktionen d. grundlegenden Aufgaben:
* Kodierung der Daten: 
    * Übersetzung zw. Zeichensätzen und Codewörtern nach standardisierten Kodierungsvorschriften
    * Datenkompression vor dem Senden zum Entfernen von unerwünschter Redundanz
    * Verschlüsselung
* Strukturierte Darstellung von Daten
    * plattformunabhängig, einheitlich
    * Übersetzung zwischen verschiedenen Datenformaten
    * Serialisierung strukturierter Daten für die übertragung

## Zeichensätze
Daten liegen entweder in Form von Textzeichen bzw. Symbolen vor, oder aber in Form von binären Daten. 

Dabei ist ein Zeichensatz eine Menge textuell darstellbarer Zeichen, sowie deren Zuordnung zu einem Codepoint (eine eindeutige Kennzahl für höchstens ein Textzeichen). Beispiele sind ASCII, ISO-8859-15 und Unicode. Ein Zeichensatz kann druckbare Zeichen, sowie Steuerzeichen (Tab, Zeilenumbruch, Protokollzeichen) enthalten. Unicode hat das Ziel, alle Schriftkulturen und Zeichensysteme abzubilden. Der Zeichensatz bei Unicode wird regelmäßig aktualisiert und erweitert. 

Ein Datum ist eine für Computer verarbeitbare Einheit, dessen Repräsentation kontextabhängig ist. 

## Kodierung
Zeichen müssen zur Übertragung kodiert werden. Man unterscheidet zwischen Fixed-Length Codes (z.B. ASCII), bei denen alle Zeichen mit Codewörtern derselben Länge kodiert werden und Variable-Length Codes (z.B. UTF-8), bei denen Zeichen mit Codewörtern unterschiedlicher Länge kodiert werden. 

## Struktierte Darstellung
Eine einheitliche Syntax für die ausgetauschten Daten muss verwendet werden, damit Anwendungen Daten austauschen können. 
* (Gepackte) structs / serialisierte Speicherbereiche
* Ad-hoc Datenformate - Entwurf nach Bedarf
* Strukturierte Serialisierungsformate (JSON = JavaScript Object Notation, XML)

### JavaScipt Object Notation JSON
JSON ist ein sprachenunabhängiges Datenformat, bei dem die Daten strukturiert und in lesbarer Form als Text übertragen werden. Es wird im Allgemeinen als UTF-8 kodiert. 

## Datenkompression
Es gibt zwei unterschiedliche Kompressionsverfahren - verlustfreie Komprimierung und verlustbehaftete Komprimierung.
1. Verlustfreie Komprimierung
* Verlustfreie Wiederherstellung ist möglich
* z.B. ZIP, PNG
2. Verlustbehaftete Komprimierung
* Keine exakte rekonstruktion möglich durch Informationsverlust bei der Komprimierung
* Höhere und in Abhängigkeit des Verlustfaktors variable Kompressionsraten sind möglich
* z.B. MP3, MPEG, JPEG

### Huffman-Code
* Short codes are assigned to numbers that appear frequently and long codes to those that occur infrequently. 
* Zeichen werden daher nicht mit uniformer Codewortlänge kodiert. 
* Die Abbildung zwischen Zeichen und Codewörtern bleibt eindeutig und umkehrbar. 
* Es handelt sich also um ein *verlustloses Kompressionsverfahren*. 
* Auftrittswahrscheinlichkeiten müssen den Erwartungen entsprechen
* Dynamische Bestimmung der Zeichenhäufigkeiten ist möglich, eine Mitteilung des verwendeten Codebuchs an den Empfänger ist notwendig. 
* Optimaler, präfixfreier Code, Variable-Length Code, Entropy-Encoding Verfahren

#### Optimaler Präfixcode
Bei einem *präfixfreien* Code sind gültige Codewörter niemals Präfix eines anderen Codeworts desselben Codes. Ein *optimaler* präfixfreier Code minimiert die mittlere Codewortlänge. 

### Familie des Run-length Codes
Einmalige Kodierung sich wiederholender Zeichen, sowie die Anzahl der Wiederholungen (zur Rekonstruktion), denn Daten weisen häufig Wiederholungen einzelner Zeichen oder Gruppen von Zeichen auf. 

# ANWENDUNGSSCHICHT
Protokolle der Anwendungsschicht stellen spezifische Dienste bereit. 

### DNS Domain Name System
Adressierung des Ziels mittels eines hierarchisch aufgebauten Namens. 
Drei Komponenten des DNS:
1. Domain Namespace
* hierarchisch aufgebauter Namespace / Namensraum
* baumartige Struktur
2. Nameserver
* Speicherung von Informationen zum Namespace
3. Resolver
* Extraktion v. Informationen aus dem Namespace durch Anfragen an den Nameserver
* Bereitstellung dieser Informationen an anfragenden Clients bzw. Anwendungen

#### Domain Namespace
Ein *Label* ist ein beliebiger Knoten im Namespace. 
Ein *Domain Name* ist eine Sequenz von LAbels. Ein *Fully Qualified Domain Name FQDN* besteht aus der vollst. Sequenz von Labels ausgehend von einem Knoten bis zur Wurzel und endet mit einem Punkt. Ein FWDN kann als Suffix für einen nicht-qualifizierten Namen verwendet werden. 

Hierarchieebenen im Namespace:
1. Root
2. TLD Top Level Domain (vergeben von ICANN Internet Corporation for Assigned Names and Numbers)
3. SLD Second Level Domain (vergeben von Registraren)
4. Subdomain

#### Nameserver
Speicherung des Namespaces in Form einer verteilten Datenbank von einer großen Anzahl von Servern. Jeder *Nameserver* kennt nur einen kleinen Teil des gesamten Namespaces. 

Die Namespaces werden daher in *Zonen* unterteilt - zusammenhängende Teilbäume des Namespaces. Eine Zone kann daher keine Teilbäume ohne gemeinsame Wurzel umfassen. Nameserver bezeichnet man als *autoritativ* für die jeweiligen Zonen, die sie speichern. 

*Ressource Records* sind die Informationen, die in einer Zone gespeichert sind. 

#### Resolver
Resolver sind Server, die durch Anfragen an die jeweiligen Nameserver Informationen aus dem DNS extrahieren und das Ergebnis an den anfragenden Client zurückliefern. Dabei stellen sie schrittweise bei den autoritativen Nameservern der jeweiligen Zone an. 

DNS-Anfragen des C an den Router sind *rekursiv* (recursive queries).

Die eigentliche Namensauflösung erfolgt beim Resolver mittels einer Reihe von *iterativen Anfragen* (iterative queries).

#### Reverse DNS
Lookup the IP adress from 

### URL Uniform Resource Locator
Mittels Fully Qualified Domain Name FQDN können wir mithilfe von DNS das Ziel einer Verbindung auf Schicht 3 (Vermittlungsschicht) identifizieren. Das verwendete Anwendungsprotokoll und eine bestimmte Ressource können jedoch nicht adressiert werden. 

<<<<<<<<<>>>>>>>>>

### HTTP HyperTextTransfer Protocol
Protokoll, das im Internet am häufigsten zur Datenübertragung zwischen Client und Server genutzt wird. 
Es definiert, welche Anfragen ein Client stellen darf und wie Server darauf zu antworten haben. Mit einem HTTP-Kommando wird höchstens ein Objekt übertragen. Dieses wird als ASCII-kodierter Text interpretiert. 

Unterscheidung zwischen zwei Nachrichten:
1. Request enthält
* Method
* Pfad u. Query-Parameter
* weitere Headerfelder
2. Response enthält
* *Status-Line* - wie hat der Server die Request bearbeitet
* *Response Header* (z.B. Encodings)
* *Body* mit Trennzeichen CRLF (Carriage Return Line Feed), welcher die eigentlichen Daten beinhaltet

#### HTTP-Methods
* *GET*: Übertragungsanfrage für ein bestimmtes Objekt vom Server S 
* *HEAD*: Übertragungsanfrage für ein Header eines best. Objektes
* *PUT*: Objektübertragung von C zum S zum ggf. Überschreiben eines bereits existierenden
* *POST*: Objektübertragung vom C zum S zum ggf. Anhängen an ein bereits existierendes Objekt
* *DELETE*: Löschen eines Obj. vom Server

#### Response Codes
* *200*: OK
* *3xx*: Redirection
* *400*: Bad Request
* *401*: Unauthorized
* *403*: Forbidden
* *404*: Not Found
* *418*: I'm a teapot
* *5xx*: Server-Error

#### Verschlüsselung bei HTTP
HTTP bietet keine Mechanismen zur Verschlüsselung. Protokolle zwischen Transport- und Anwendungsschicht können aber verwendet werden. 

HTTPS Hyper Text Transfer Protocol Secure 
* Nutzung von TLS
* Ver-/Entschlüsselung des Datentransfers
* Verwendung von TCP 443
* Für TLS ist HTTP nur ein Datenstrom

#### HTTP Proxy
Verwendung einer dazwischenliegenden Proxy anstelle einer Ende-zu-Ende-Verbindung zu einem HTTP-Server. 
* Anfragen werden an eine HTTP-Proxy geschickt
* Entgegennahme von Anfragen durch diese Proxy
* Aufbau einer neuen Verbindung zum Zielserver / weiterer Proxy
* Proxy stellt die Anfrage (!) anstelle von C
* Senden der Antwort an die Proxy
* Zustellung der Antwort durch Proxy an Client

### SMTP Simple Mail Transfer Protocol
Dieses textbasiertes Protokoll dient dem Verwenden von E-Mails, also dem Transport vom *MUA* (Mail User Agent) zum *MTA* (MAil Transfer Agent), sowie dem Transport von Emails zwischen MTAs. 

Verwendung von *POP* (Post Office Protocol) oder *IMAP* (Internet Message Access Protocol) in Empfangsrichtung.


### FTP File Transfer Protocol
Protokoll zum Übertragen von Dateien zwischen gleichartigen / unterschiedlichen Endgeräten. 

FTP nutzt zwei getrennte TCP-Verbindungen
1. Kontrollkanal: Übertragung v. Befehlen und Statuscodes zw. C und S
2. Datenkanal: Übertragung d. eigentlichen Daten

FTP ist *stateful*, bleibt also über mehrere Datentransfers hinweg bestehen.  FTP benötigt grundsätzlich eine Art von Authentifizierung. 

FTP arbeitet in zwei Modi:
1. Active mode: mittels PORT-Kommando wird vom C aus dem S eine zufallige Portnummer mitgeteilt, auf der der S vom Quellport eine neue TCP Verbindung zum C aufgebaut wird (Verwendung als Datenkanal)
2. Passive mode: C sendet Kommando PASV über den Kontrollkanal und erhält vom S IP-Adresse und Portnummer, zu der der Client eine zweite TCP-Verbindung aufbaut (Verwendung als Datenkanal)