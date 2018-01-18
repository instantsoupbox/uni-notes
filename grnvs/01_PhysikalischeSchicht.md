KAP 1 - Physikalische Schicht
=====
### Signale, Informationen und deren Bedeutung
#### Information und Entropie

***
<details>
    <summary>Signale, Symbole</summary>
    Signal: zeitabhängige, messbare physikalische Größe. <br>
    Symbol: Definierte, messbare Signaländerung <br>
</details>

<details>
    <summary>Informationstheorie nach Shannon, Informationsgehalt</summary>
    Der Informationsgehalt eines Zeichens drückt aus, wieviel Information durch das Zeichen übertragen wird.
</details>

<details>
    <summary>Informationstheoretisches Modell eines gedächtnislosen Kanals (zeichnen)</summary>
    siehe Skript
</details>

<details>
    <summary>Empfangsentropie</summary>
    ist die Sendeentropie abzüglich eines Informationsverlusts durch den Kanal zuzüglich der Fehlinformation 
</details>

<details>
    <summary>Transinformation</summary>
    ist die Sendeentropie abzüglich des Informationsverlusts bzw. Empfangsentropie abzüglich der Fehlinformation </br>
    Mutual Information, die von Sender zu Empfänger über einen gedächtnislosen Kanal transportiert wird. 
</details>

*Informationsgehalt* I eines Zeichens x mit einer bestimmten Aufenthaltswahrscheinlichkeit (Einheit: bit!!!). Je seltener ein Zeichen auftritt, desto höher ist sein Informationsgehalt. Der Informationsgehalt eines vorhersagbaren Zeichens ist 0.

Die *Entropie* ist der mittlere Informationsgehalt einer Quelle. Daraus lässt sich sagen, wie viele Bits zur Kodierung eines Zeichens notwendig sind.

*Bedingte Entropie* zweier Zufallsvariablen $X$ und $Y$ entspricht der verbleibenden Unsicherheit über $Y$, wenn $X$ bekannt ist.

#### Neue Begriffe:

Zeitdiskretisierung, Abtasttheorem, Quantisierung, 

Leitungskodierung (Abbildungs-/Interpretationsvorschrift), 

Rauschen, Dämpfung, Verzerrung (Störfaktoren), 

Kanalkodierung (Fehlererkennung/-korrektur), 

Impulsformung, Modulation (Erzeugung eines Signals)

### Signaldarstellung
***
#### Fourierreihe
Mittels einer Fourierreihe lassen sich periodische Signale darstellen. 
Überlagerung von sin-/ cos-Schwingungen unterschiedlicher Frequenzen = Periodische Zeitsignale

#### Einfache Signaleigenschaften

#### Fouriertransformation
Der Frequenzbereich eines nicht-periodischen, kontinuierlichen Signals lässt sich mittels Fouriertransformation bestimmen. 

### Abtastung, Rekonstruktion und Quantisierung
***
<details>
    <summary> Zeitdiskretisierung von Signalen </summary>
    Abtastung
</details>

<details>
    <summary> Abtasttheorem von Shannon und Nyquist </summary>
    Ein auf B bandbegrenztes Signal muss mind. mit der Frequenz 2B abgetastet werden, um das Signal verlustfrei rekonstruieren zu können, d.h. damit keine Information verloren geht. 
</details>

<details>
    <summary> Wertdiskretisierung von Signalen </summary>
    Quantisierung
</details>

<details>
    <summary> Maximaler Quantisierungsfehler bei gegebener Schrittweite? </summary>
    Halbe Schrittweite
</details>

Natürlich vorkommende Signale sind i.d.R. zeit- und wertkontinuierlich. 

Es ist also eine Zeit- und Wertdiskretisierung notwendig. 

Abtastwerte sind Stützstellen als Gewichte für eine passende Ansatzfunktion (Interpolation).

Aliasing: Überlapping der periodischen Wiederholungen des Spektrums. Verlustfreie Rekonstruktion ist nicht mehr möglich. 

Codierung der unterschiedlichen Signalstufen. Jeder Signalstufe wird dabei ein bestimmtes Codewort zugeordnet. Verteilung der Signalstufen im Quantisierungsintervall. 

## Übertragungskanal
Wir wollen nun erfahren, welchen Einfluss der Übertragungskanal auf das Signal hat und wie hoch die theoretisch maximal erzielbare Übertragungsrate ist. 

### Kanaleinflüsse
* Dämpfung
* Tiefpassfilterung
* Verzögerung
* Rauschen
***
<details>
    <summary> Dämpfung </summary>
    Geringere Signalamplitude beim Ausgang als am Eingang
</details>

<details>
    <summary> Tiefpassfilterung </summary>
    Höhere Frequenzen werden stärker gedämpft als niedrigere
</details>

Unser Modell berücksichtigt **Dämpfung** (geringere Signalamplitude beim Ausgang als am Eingang), Tiefpassfilterung (höhere Frequenzen werden stärker gedämpft, als niedrige), Verzögerung (Übertragung benötigt eine gewisse Zeit), sowie Rauschen in Form von Additive White Gaussian Noise. 

Ein Kanal wirkt wie ein **Tiefpass** (Tiefpasscharakteristik) und zusätzliches Rauschen stört die Übertragung. Niedrige Frequenzen passieren ungehindert (Tiefpass), hohe Frequenzen werden gedämpft, Vernachlässigung von Signalanteilen ab einer bestimmten Frequenz (weil die Dämpfung zu stark ist). 

Durch die Tiefpasscharakteristik kann man vereinfachend auch von einer scharfen Grenzen - der **Kanalbandbreite** sprechen. 

### Kanalkapazität
***
<details>
    <summary> Wovon ist die erzielbaren Datenrate auf einem Kanal mit beigegebener Bandbreite abhängig? </summary>
    Benötigt: Zusammenhang zwischen Bandbreite, Anzahl unterschiedlicher Signalstufen, Verhältnis zwischen Leistung des Nutzsignals und des Rauschens. 
</details>

<details>
    <summary> Was ist die Nyquist-Rate? </summary>
    $f_N = 2B$
    Eine untere Schranke für die minimale Abtastrate (Abtasttheorem), obere Schranke für die Anzahl an Symbolen je Zeiteinheit, die nach der übertragung über den Kanal unterscheidbar sind.
</details>

<details>
    <summary> Was besagt Hartleys Gesetz? </summary>
    Es berechnet die obere Schranke für die Kanalkapazität durch gegebene Bandbreite $B$ mit $M$ unterscheidbaren Signalstufen. 
</details>

Rauschen erschwert die Unterscheidung von Signalstufen mit steigender Feinheit der Signalstufen. 

<details>
    <summary> Gib das Mass für die Stärke des Rauschens an!</summary>
    Signal to Noise Ratio in Dezibel!
</details>

### Nachrichtenübertragung
***
<details>
    <summary>Nachrichtenübertragung</summary>
    <table style="width:100%">
        <tr>
            <td>Quellenkodierung</td>
            <td>Quellendekodierung</td>
            <td>Schicht 6/1</td>
        </tr>
        <tr>
            <td>Kanalkodierung</td>
            <td>Kanaldekodierung</td>
            <td>Physikalische Schicht</td>
        </tr>
        <tr>
            <td>Leitungskodierung</td>
            <td>Detektion</td>
        </tr>
        <tr>
            <td>Modulation</td>
            <td>Demodulation</td>
        </tr>
    </table></br>
</details>

<details>
    <summary>Überprüfung von Datensendung oder Beginn / Ende der Nachricht? </summary>
    <ul>
    <li> Coderegelverletzung: Präambel vor Beginn einer Nachricht (fest definierte Anzahl alternierender Bits) / Start Frame Delimiter zeigt Beginn einer Nachricht an (SFD) </li>
    <li> Steuerzeichen: Blockcode, der Steuerzeichen bereit stellt, Kanalwörter werden in Gruppen von k-Bits unterteilt und auf n>k Bits abgebildet </li>
    </ul>
</details>

**Basisbandsignale**, für das der Übertragungskanal exklusiv zur Verfügung steht. Zeitlich begrenzte Sendeimpulse besitzen ein unendlich ausgedehntes Spektrum. 

1. Quellenkodierung (Source Coding)

    * Entfernung von Redundanz aus den zu übertragenden Daten (Abb. Bitsequenzen auf Codewörter)

    * Verlustlose Datenkompression

    * Huffman-Code, Lempel-Ziv / Lempel-Ziv-Welch (LZW) / Run-Length-Encoding (RLE)

2. Kanalkodierung (Channel Coding)

    * Gezieltes / strukturierte Hinzufügen von Redundanz

    * Ziel: Erkennen / Korrektur von Bitfehlern 

3. Leitungskodierung
    
    * Leitungscode: Abfolge einer best. Art von Grundimpulsen, welche Bits oder Gruppen von Bits repräsentieren (Symbol: physkalisch messbare Veränderung des Zeitsignals)
    * Abfolge von Grundimpulsen: Sendeimpuls
    * Grundimpuls: Rechteckimpuls, cos^2-Impuls
    * Leitungsimpuls: Non-Return-to-Zero, Return-To-Zero, Manchester-Code, Multi-Level-Transmit 3

4. Modulation

    * zeitgleiche Verwendung des Kanals von mehreren Übertragungen
    * Tiefpassfilterung des Basisbandsignals, Aufmodulierung des gefilterten Basisbandsignals auf ein Trägersignal (Verschiebung des Spektrums) 
    * **Frequency Division Multiplex** (FDM): Mehrere Übertragungen teilen sich auf diese Art einen Kanal.

### Übertragungsmedien