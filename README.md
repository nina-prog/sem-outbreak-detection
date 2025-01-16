# sem-outbreak-detection
---
## Motivation
* A main purpose of routine surveillance of infectious disases is to spot unusual events early on.
* Challenges:
  * Many diseases and subgroups (e.g., regions) need to be monitored in parallel.
  * Outbreak detection needs to be prospective, i.e., deal with the difficulties of real-time data.
  * Incidence is low for many diseases and signals can be subtle.
  * The patterns one will want to detect are context-specific.
* Bioterrorism (for example In the aftermath of the 9/11 terrorist attacks (2001), a series
of anthrax-laced letters were sent to journalists and
politicians) --> [Hutwagner et al (2003): The Bioterrorism Preparedness and Response Early Aberration Reporting System
(EARS)](https://pmc.ncbi.nlm.nih.gov/articles/PMC3456557/)

## Description
"Compare outbreak detection methods in presence of reporting delays in a simulation study and maybe suggest some simple new procedure.(Nowcasting (for handling reporting delays) --> 2. Outbreak detection)"
  * überzeugendes Simulations-Setting auf Basis realer zeitreihen
    * 1. ohne reporting delays
    * 2. mit simulierten reporting delays
      * real reporting delays (only real data)
      * simuliert: periodische delay (z.B. wöchentlicher Bericht)
      * simuliert: konstante Verzögerung: Ein fixer Verzug für alle Tage (z.B., 3 Tage).
      * simuliert: variierende Verzögerung: Log-normal verteilte Verzögerungen (Mittelwert=3, Streuung=1)
      * simuliert: episodische Verzögerungen: Zufällige Tage mit Verzögerungen (z.B., durch Feiertage wie Weihnachten oder Wochenenden)
  * Teste 2-3 Outbreak detection Verfahren (für mit und ohne reproting delays) --> Quantifiziere: Wie viel schlechter ist die Ausbruchserkennung, wenn man Meldeverzüge hat?

## Data
### Assumptions:
  * **real** data
  * seasonal infections (e.g., norovirus, rotavirus -> gastrointestinal infections)
  * **simulated reporting delays** (discretized log-normal, gamma)
  * **simulated outbreaks** (exponential increase)
    * Randomly placed in a week in the time series
    * With added excess cases (e.g., 10x the baseline incidence)
    * Test outbreaks in different time points (peak, middle or low seasonal activity)

### Data Sources:
  * RKI SurvStat (DE): Nationale Daten zu meldepflichtigen Krankheiten in Deutschland (z. B. Influenza oder Masern) or weekly infulenza report ([RKI SurvStat](https://survstat.rki.de/Content/Query/Create.aspx))
  * CDC FluView (USA): Influenza Surveillance Reports (weekly Inzidenz Bericht) ([CDC FluView](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html))
  * WHO FluNet (International): Internationale Influenza-Inzidenzdaten (wöchentliche Inzidenz pro Land, verfügbar im CSV-Format). ([WHO FluNet Data](https://www.who.int/influenza/gisrs_laboratory/flunet/en/))
  * ECDC (EU): Daten zu meldepflichtigen Krankheiten in Europa ([ECDC Surveillance Atlas](https://atlas.ecdc.europa.eu/public/index.aspx))

### Used Data:
* **Datenquelle**: Meldungen gemäß Infektionsschutzgesetz (§7.1 IfSG und §7.3 IfSG) von [RKI SurvStat](https://survstat.rki.de/Content/Query/Create.aspx). Alle Daten sind auf Basis der jüngst veröffentlichten Datenstände erhoben. Die Berichterstattung erfolgt gemäß den Vorgaben des RKI und Gesundheitsämtern.
* **Berichtszeitraum**:
  * §7.1 IfSG: Vom 01.01.2025 bis Ende der 52. Kalenderwoche 2024.
  * §7.3 IfSG: Vom 01.01.2025 bis Oktober 2024.
* **Meldeweg**: Daten wurden über Gesundheitsämter und Landesstellen erhoben und weitergeleitet.
* **Referenzdefinition**: Es werden ausschließlich Fälle berücksichtigt, die die Referenzdefinition des RKI erfüllen.
* Erfasste **Krankheiten**: **Norovirus**-Gastroenteritis & **Rotavirus**-Gastroenteritis

Daten sind nach saisonalem Kalender geordnet (KW27 bis KW26 des Folgejahres). Grund: Die saisonale Einteilung (Start KW27) ist an die epidemiologische Saison der Meldepflicht angepasst, da Krankheiten wie Rotavirus-Gastroenteritis typischerweise saisonale Muster aufweisen.

Zeile: Saisonjahr (z.B. 2024/2025), beginnend mit KW27. Entspricht saisonalem Kalender, indem die Berichterstattung von Sommer bis Sommer erfolgt. Gibt an, wann das Gesundheitsamt erstmalig durch Meldung oder eigene Ermittlung von einem Fall offiziell Kenntniss erlangte.
Spalte: Saisonwoche (SW1 entspricht KW27 des saisonales Kalenders). Gibt an, in welcher Kalenderwoche der Fall erstmalig bekannt wurde.

## Outbreak Detection approaches
Most methods follow the same principle:
* Some model is set up to obtain a distribution of plausible values if nothing unusual is going on (“in control”).
* Some high quantile of this distribution (e.g., at 1 − α = 0.99) is chosen as a threshold value.
* -> If a newly incoming observation exceeds this threshold, an alarm is flagged.
Diffrence between methods: Mostly differ in the construction of the reference distribution.

Possible to test (+named in lectures):
Differentiate:
* Drop-in surveillance
  * Setting: a new threat / specific occasion requires a specific surveillance effort over a **short time**, very limited baseline data (≤ 7 days) are available
  * Goal: detecting aberrations relative to the **immediate past**
* **Monitoring Seasonal Diseases:
  * Setting: has been monitored **for several years**, surveillance practices have remained comparable
  * Goal: detecting aberrations relative to **past seasonal patterns**

* The Early Aberration Reporting System (EARS) (see slides)
* **The Farrington Method** (see slides)
* Cumulative sum (CUSUM) methods
* Time series (ARIMA) methods

* nowcasting + farrington

## Sources
* 1 [Kapitel Allevius und Höhle](https://arxiv.org/abs/1711.08960)
* 2 [Unkel et al](https://doi.org/10.1111/j.1467-985X.2011.00714.x)
* 3 [Paper Noufaily et al](https://onlinelibrary.wiley.com/doi/10.1002/sim.5595) (no reporting delays) -> Vorbild für das Setup einer Simulationsstudie (vielleicht etwas abschauen)
  * Notes:
* Specific Outbreak Detection + Reporting Delays (Ausbruchserkennung und Meldeverzüge):
   * 4 [Salmon et al](https://onlinelibrary.wiley.com/doi/10.1002/bimj.201400159) -> R-package, evtl. "surveillance"
   * 5 [Noufaily et al](https://academic.oup.com/jrsssa/article/178/1/205/7058463)
   * 6 [Noufaily et al](https://www.tandfonline.com/doi/full/10.1080/01621459.2015.1119047)
* Nowcasting: 7 [Wolffram et al](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011394) -> Outbreak detection (Ausbruchserkennung) mit der "frozen time series" (einfache Variante)

## Information
- Statistische Methoden und Ökonometrie, Institut für Statistik (STAT)
- Supervisor: Dr. Johannes Bracher
Presentation:
* Intermediate presentations: presenting and motivating the topic, showing some preliminary work.
Paper:
* 15 – 20 pages (including figures and references, excluding title page, table of
contents, declarations)
* standard 12pt, 2cm margin format

## Table of Contents Outline
1. Introduction & Motivation
  * 1.1 Aim of the Thesis
    * Untersuchung des Einflusses von Reporting Delays auf die Ausbruchserkennung.
    * Vergleich verschiedener Methoden zur Ausbruchserkennung mit und ohne Meldeverzüge.
    * Entwicklung eines neuen Modells, das Reporting Delays berücksichtigt.
  * 1.2 Motivation
    * Bedeutung der frühzeitigen Erkennung von Ausbrüchen für die öffentliche Gesundheit.
    * Herausforderungen bei der Ausbruchserkennung, insbesondere bei Meldeverzügen.
    * Relevanz in spezifischen Kontexten: Bioterrorismus, Routineüberwachung von Infektionskrankheiten
2. Literature Review
3. Theoretical Foundation
    * 3.1 Foundation of Outbreak Detection
        * 3.1.1 Prinzipien der Referenzverteilung und Schwellenwerte.
        * 3.1.2 Unterschied zwischen Drop-in Surveillance und Monitoring Seasonal Diseases.
    * 3.2 Outbreak Detection Methods
        * 3.1.2 The Farrington Method: für saisonale Daten
        * 3.1.3 Cumulative sum (CUSUM) methods: kumulative Abweichungen zur Alarmerkennung
        * 3.1.4 Time series (ARIMA) methods: zeitserienbasierte Modellierung für komplexe Muster
    * 3.3 Reporting Delays
        * 3.2.1 Definition, Cause (z.B. administrative Prozesse, Testlaufzeiten) and Types of Reporting Delays
        * 3.2.2 Impact of Reporting Delays on Outbreak Detection
        * 3.2.3 Methods to Handle Reporting Delays
4. Data
    * 4.1 Real Data
      * Suche nach offenen epidemiologischen Datensätzen (z.B. COVID-19-Daten, Influenza-Überwachung).
      * Verwendung von Datensätzen ohne und mit Meldeverzüge als Basis.
    * 4.2 Simulation Data
      * 3.2.1. Baseline Data Generation
        * Generiere Daten mit bekannten saisonalen Mustern oder Poisson-Verteilung.
        * Füge kontrolliert Ausbrüche hinzu (z.B. exponentielle Anstiege).
      * 3.2.2. Simulation von Reporting Delays ** auf realen Daten auch zum Vergleich?
        * Einführung von Meldeverzügen: Konstante Verzögerungen (z.B. 2–3 Tage), Variierende Verzögerungen (z.B. log-normal verteilt).
    * 4.3 Hybrid Data ?
      * Verwenden von realen Daten für Grundrauschen und synthetische Ausbrüche.
5. Methodology
    * 5.1 Simulation Setup................
7. Results
   * Leistung ohne Reporting Delays. (Diagramm + Tabelle)
   * Leistung mit Reporting Delays (ohne Korrektur). (Diagramm + Tabelle)
   * Leistung mit Nowcasting (Korrektur). (Diagramm + Tabelle)
   * Quantitativ: Wie stark verschlechtert sich die Erkennung durch Reporting Delays?
   * Quantitativ: Welcher Nowcasting-Ansatz ist am effektivsten?
   * Robustheit: Sensitivitätsanalyse: Wie verändern sich die Ergebnisse bei Variation der Simulationsparameter?
8. Discussion
    * Erkenntnisse: Wesentliche Ergebnisse des Vergleichs.
    * Praktische Relevanz der Methoden für die epidemiologische Überwachung.
    * Limitationen: Begrenzungen der Simulation
    * Datenverfügbarkeit und Annahmen.
    * Zukunftsperspektiven: Potenziale für die Entwicklung neuer Verfahren.
    * Integration von Echtzeitdaten und Machine Learning.
9. Conclusion
        * Impact on Outbreak Detection
