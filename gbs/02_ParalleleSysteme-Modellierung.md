# 02 Parallele Systeme - Modellierung
* Sequentielle / Parallele Programmausführung
* Nebenläufige / Echt-parallele Programmausführung

## 2.1 Einführung
* Deterministisches Programmverhalten bei sequentieller Programmausführung
* Nichtdeterminismus: Unterschiedliches Systemverhalten bei gleichen Ausgangsbedingungen und gleichen Eingaben, i.e. parallelen Programmausführung.


* Sequentiell
* Parallel
    * Nebenläufig
    * Echt-parallel
* Interaktionen zwischen parallelen Programmen (i.e. Prozessen) können zu nicht-deterministischem Verhalten führen, sie beeinflussen sich gegenseitig:
    1. Kausale Beziehungen
    2. Kommunikation - Nachrichtenaustausch
    3. Koordinierung - Beziehung zw. Auftraggeber/-nehmer
    4. Konkurrenz - Konkurrenz um gemeinsame Ressourcen

## 2.2 Modellierung paralleler Systeme
* 2 Sichten, die sich zur Verhaltensbeschreibung ergänzen:
    * Aktionen - Sicht auf Tätigkeiten
    * Ereignisse - Sicht auf Veränderungen
* Eigenschaften:
    1. Determiniertheit
    2. Störungsfreiheit
        * Das Ergebnis wird unter Einhaltung einer festgelegten Ausführungsreihenfolge paralleler Ereignisse und deren Aktionen nicht beeinflusst.
    3. Wechselseitiger Ausschluss (mutual exclusion)
        * Sicherstellen v. exklusiver Verwendung von Resourcen
    4. Verklemmungsfreiheit
        * keine zyklische Wartesituation auf Ereignisse, die nur von Prozessen der selben Mengen verursacht werden können.
    5. Kein Verhungern
        * kein unendlich langes Aufschieben von Prozessen, obwohl sie sich nicht in einem Deadlock befinden. 
* **Sequentielles System** führt elementare Aktionen sequentiell aus
* **Paralleles System** umfasst seq. und nebenl. Aktionen

## 2.5 Petri-Netze
* **Lebendigkeit / liveliness**: Für jede erreichbare Markierung gibt es eine andere Markierung, die aus dieser Markierung aus erreichbar ist und in der  
* **Verklemmung / deadlock**: 
    * Vollständige Verklemmung: Von einer erreichbaren Markierung aus kann keine weitere Transition schalten
    * Lokale Verklemmung: Von einer erreichbaren Markierung aus gibt es eine Transition, sodass von dieser ausgehend keine Folgemarkierung erreichbar ist, in der diese transitionsbereit ist.
* Wenn ein Petrinetz lebendig ist, dann ist es auch verklemmungsfrei.
* **Fairness**: Ein Petrinetz ist unfair für eine Transition, wenn
    * diese unendlich oft transitionsbereit ist
    * es eine unendliche Sequenz gibt, in der diese nur endlich oft auftritt
* **Verhungern**: Eine Transition verhungert, wenn in einer unendlichen Sequenz diese trotz der eigenen Transitionsbereitschaft niemals schaltet.