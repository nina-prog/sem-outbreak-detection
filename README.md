# sem-outbreak-detection
---
## Motivation:
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
  
## Description:
Variante 1:
"Compare outbreak detection methods in presence of reporting delays in a simulation study and maybe suggest some simple new procedure."
  * überzugendes Simulations-Setting ausdenken (z.B. teilweise mit realen Daten): 
    * ohne reporting delays (hier evtl. reale Daten)
    * mit reporting delays (hier simuliert)
  * Teste 2-3 Outbreak detection Verfahren (für mit und ohne reproting delays) --> Quantifiziere: Wie viel schlechter ist die Ausbruchserkennung, wenn man Meldeverzüge hat?
Variante 2:
* "Neues Modell entwickeln welches beides koppelt."
  * 1. Nowcasting (for handling reporting delays) --> 2. Outbreak detection

## Outbreak Detection approaches:
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
* Monitoring Seasonal Diseases:
  * Setting: has been monitored **for several years**, surveillance practices have remained comparable
  * Goal: detecting aberrations relative to **past seasonal patterns**

* The Early Aberration Reporting System (EARS) (see slides)
* **The Farrington Method** (see slides)
* Cumulative sum (CUSUM) methods
* Time series (ARIMA) methods

## Sources:
* [Kapitel Allevius und Höhle](https://arxiv.org/abs/1711.08960)
* [Unkel et al](https://doi.org/10.1111/j.1467-985X.2011.00714.x)
* [Paper Noufaily et al](https://onlinelibrary.wiley.com/doi/10.1002/sim.5595) (no reporting delays) -> Vorbild für das Setup einer Simulationsstudie (vielleicht etwas abschauen)
* Specific Outbreak Detection + Reporting Delays (Ausbruchserkennung und Meldeverzüge):
   * [Salmon et al](https://onlinelibrary.wiley.com/doi/10.1002/bimj.201400159) -> R-package, evtl. "surveillance"
   * [Noufaily et al](https://academic.oup.com/jrsssa/article/178/1/205/7058463)
   * [Noufaily et al](https://www.tandfonline.com/doi/full/10.1080/01621459.2015.1119047)
* Nowcasting: [Wolffram et al](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011394) -> Outbreak detection (Ausbruchserkennung) mit der "frozen time series" (einfache Variante)

## Information
- Statistische Methoden und Ökonometrie, Institut für Statistik (STAT)
- Supervisor: Dr. Johannes Bracher
Presentation:
* Intermediate presentations: presenting and motivating the topic, showing some preliminary work.
Paper:
* 15 – 20 pages (including figures and references, excluding title page, table of
contents, declarations)
* standard 12pt, 2cm margin format
