# 10 - Betriebssystem-Sicherheit

## 10.1 Einführung

|Schutzziel|Beschreibung|Maßnahmen|
|---|---|---|
|**Authentizität**|Gewährleistung d. Korrektheit der Akteure|Konzepte zur Idenfizierung von Prozessen, Benutzern, Diensten, Authentifizierung: Identitäts-Überprüfung, Sichere Speicherung von Identitätsinformationen (z.B. Passwort)|
|**Daten-Integrität**|Schutz der Daten gegen Manipulation|Rechtevergabe, Zugriffskontrolle, Isolierung, Speicherschutz, Prüfsummen (kryptografische Hashfunktionen)|
|**Vertraulichkeit**|Schutz vor unbefugten Zugriffen|Verschlüsselung v. gespeicherten Daten, Zugriffskontrolle, Isolierung, Virtualisierung|
|**Verfügbarkeit**|Gewährleistung der Verfügbarkeit von IT-Diensten|RAID-Systeme zur Erhöhung der Ausfallsicherheit, Fehlererkennung und -korrektur durch z.B. Hamming-Codes|

## 10.3 Trusted Computing Base

* fasst sicherheitskritische Funktionen der HW und des BS zusammen
* Umfasst:
    * einen Teil der HW
    * BS-Kern (Subsysteme wie Prozess- und Speicherverwaltung)
    * Programme mit gesetztem `suid`-Bit
* idealerweise klein, damit leicht zu verifizieren.

## 10.4 Kryptographie
Sicherstellung v. Vertraulichkeit (Schutz gg. unbef. Zugriff).
* **Symmetrische Verschl.**: Kurze Schlüssel, 1 Schlüssel pro Kommunikationspartner
* **Asymmetrische Verschl.**: Lange Schlüssel, 1 privater und 1 öffentlicher Schlüssel pro Teilnehmer

Durchsetzung v. Integrität (Schutz d. Daten gg. Manipulation):
* **Message Digest** (Kryptographische Hashfunktion)
    * Einweg-Eigenschaft: Aus dem Hash darf die Nachricht nicht effizient bestimmbar sein.
    * Schwache + starke Kollisionsresistenz

Durchsetzung v. Daten-Authenzität (Prüfung d. Korrektheit d. Identität d. Akteure):
* **HMAC** (Message Authentication Code)
* **Digitale Signatur** (asymmetrisch)

## 10.5 Authentifizierung
Authentifizierungsklassen: Basiernd auf
* **Wissen** (Passwort, PIN)
* **Besitz** (Smartcard, Token, SIM-Karte)
* **Biometrie** (Fingerabdruck, Retina)

## 10.6 Zugriffskontrolle
Konzepte d. BS:
* **Objekte**
* Eindeutige Identifikation / Beschreibung d. Objekte durch das BS (z.B. i-node einer Datei)
* Ausführbare **Operationen** auf dem Objekt sind durch das BS festgelegt
* **Subjekte** als agierende Entitäten, die eindeutig identifizierbar sein müssen
* Rechtevergabe an die Subjekte

## 10.7 Speicherschutz
`//todo`

## 10.8 Secure Boot
