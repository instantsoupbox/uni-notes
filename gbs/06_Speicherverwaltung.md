# 06 - Speicherverwaltung
# 6.1 Einführung
* Hauptspeicher = RAM = Main Memory
* Von-Neumann-Flaschenhals: 
    * Prozessoren werden zwar schneller, aber Speicherzugriffszeiten können damit nicht Schritt halten.
    * Lösung: Einführung einer Speicherhierarchie. 
* Speicherhierarchie:
    1. Register
    2. Cache
    3. Hauptspeicher
    4. Festplatten
    5. Bandlaufwerke

# 6.2 Adressraumkonzept
* **Adressraum**: 
    * Abstrakte, isolierte Menge an Speicherzellen, die ein Prozess adressieren kann. 
    * Zweck: Schutz des Betriebssystems, das im physischen Hauptspeicher liegt, vor Programmen, falls diese auf den gesamten physischen Hauptspeicher zugreifen könnten.
* **Physischer Adressraum** des Hauptspeichers `-->` physische Speicheradresse 
* **Logischer Adressraum** eines Prozesses `-->` logische / virtuelle Adresse
* **Speicherabbildung** (Memory Address Translation)
    * Direkt
    * Basis
    * Segment
    * Seiten
    * Segment-Seiten Adressierung.

