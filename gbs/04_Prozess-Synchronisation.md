# 04 - Prozess-Synchronisation

## 4.2 Modellierung
* **Race condition**:
    * Mindestens 2 Prozesse greifen lesend/schreibend auf gemeinsame Ressource zu
    * Ergebnis hängt von der Ausführungsreihenfolge ab
    * Race conditions können durch mutual exclusion der konkurrierenden Aktionen vermieden werden. 

* **Kritischer Abschnitt**:
    * Ein Abschnitt, in dem Prozesse auf gemeinsame Ressourcen zugreifen.
    * Um Race conditions zu vermeiden, muss der Zugriff sequentialisiert werden.
    * Phasen des kritischen Abschnitts:
        1. Ausführung d. unkritischen Abs.
        2. Betreten d. kritischen Abs.
        3. Ausführung d. Transition des kritischen Abschnitts.
        4. Verlassen d. kritischen Abschnitts.

## 4.3 Koordinierungskonzepte
* Lösungskonzepte auf
    * HW-Ebene
        * Deaktivieren v. Interrupts / Unterbrechungssperre (P kann ohne Unterbrechung seinen krit. Abs. ausführen.)
        * Atomare Maschinenbefehle 
    * Einfache BS-System-Dienste
        * mit aktivem Warten (busy waiting): *Test-Set-Lock* (`TSL RX, LOCK`) - kontinuierliches Überprüfen einer Variable, bis ein bestimmter Wert auftaucht.
        * mit passivem Warten: `sleep` und `wakeup`, um P in Zstd. wartend oder rechenbereit überzuführen
    * BS-Konzepte auf höherem Abstraktionsniveau
        * Semaphore
            * *Ganzzahlige* Kontrollvariable
            * Operationen:
                * Initialisieren
                * up `V`
                * down `P`
        * Mutex (binäre Semaphore)
    * Programmiersprache

### Producer-consumer problem / Bounded-buffer problem
`//todo`