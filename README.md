# sem-outbreak-detection
---
## TODO
- [ ] Seminar Anmeldung Campus System
- [ ] Literature Review
- [x] Define Research Question
- [x] Source Data
- [x] Data Preparation
- [ ] Simulation Setup --> dataset with outbreaks + delays, dataset with outbreaks + no delays
  - [x] Simulate outbreaks
  - [ ] Simulate reporting delays
- [ ] Apply Detection Methods on simulated data
  - [ ] No delays: Serves as a baseline for evaluating performance
    - [ ] Farrington Method
    - [ ] CUSUM
    - [ ] ARIMA
  - [ ] With delays
    - [ ] Farrington Method
    - [ ] CUSUM
    - [ ] ARIMA
- [ ] Compare Methods/Results
- [ ] Optional: Nowcasting Correction Methods (if time allows)

## Motivation
* A main purpose of routine surveillance of infectious disease is to spot unusual events early on.
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
"How do reporting delays impact the performance of outbreak detection methods? Optional: And can optional corrective procedures like nowcasting mitigate these effects?"
Objectives:
* Quantify the impact of reporting delays on outbreak detection accuracy and timeliness. How much worse is outbreak detection when you have reporting delays?
* Optionally, explore and evaluate new correction procedures (e.g., nowcasting) to address the challenges posed by delays.

## Simulation Design
* Data Source:
  * Use **real-world** epidemiological data as a baseline to generate realistic outbreak scenarios.
  * Epidemiological(/infectionous) data is seasonal (e.g., Norovirus & Rotavirus --> gastrointestinal infections)
* Outbreak Simulation:
  * Outbreak Placement:
    * Perform seasonal decomposition on the time series to identify and classify time points as peak, mid-season, or low-activity based on the seasonal component
    * Define a fixed number n of outbreaks to simulate.
    * Distribute these outbreaks equally and periodically (e.g., n outbreak per seasonal year or equivalent cycle). Ensure variation in seasonal intensity and periodical alignment to account for different seasonal phases in different periods.
  * Outbreak Modeling:
    * Simulate outbreaks using added excess cases:
      * Poisson process: For random outbreak events with a fixed average rate.
      * Exponential process: For outbreaks with time-varying intensity (exponential growth).
    * Define corresponding target variable indicating the presence or absence of outbreaks.
* Reporting Delay Simulation:
  * Discretized log-normal distribution: For a continuous delay distribution.
  * Gamma distribution: For a continuous delay distribution.

## Data
### Data Sources to investigate:
  * RKI SurvStat (DE): National data reports on notifiable diseases in Germany (e.g. influenza or measles) ([RKI SurvStat](https://survstat.rki.de/Content/Query/Create.aspx))
  * CDC FluView (USA): Influenza Surveillance Reports (weekly) ([CDC FluView](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html))
  * WHO FluNet (International): International influenza incidence data (weekly incidence per country, available in CSV format). ([WHO FluNet Data](https://www.who.int/influenza/gisrs_laboratory/flunet/en/))
  * ECDC (EU): Data on notifiable diseases in Europe ([ECDC Surveillance Atlas](https://atlas.ecdc.europa.eu/public/index.aspx))

### Used Data:
* **Data source**: Notifications in accordance with the Infection Protection Act (§7.1 IfSG and §7.3 IfSG) from [RKI SurvStat](https://survstat.rki.de/Content/Query/Create.aspx). All data is collected on the basis of the most recently published data. Reporting is carried out in accordance with the specifications of the RKI and health authorities.
* **Reporting period**:
  * §7.1 IfSG: From 01.01.2025 to the end of the 52nd calendar week 2024.
  * §7.3 IfSG: From 01.01.2025 to October 2024.
* **Reporting channel**: Data was collected and forwarded via health authorities and state offices.
* **Reference definition**: Only cases that fulfil the RKI reference definition are taken into account.
* **Diseases recorded**: **Norovirus** gastroenteritis & **Rotavirus** gastroenteritis.

* **Data Structure**: Data is organised according to the **seasonal calendar** (CW27 to CW26 of the following calendar year). Reason: The seasonal classification (start CW27) is adapted to the epidemiological season of the reporting obligation, as diseases such as rotavirus gastroenteritis typically show seasonal patterns.
* Row: Seasonal year (e.g. 2024/2025), starting with CW27. Corresponds to seasonal calendar in which reporting takes place from summer to summer. Indicates when the health authority first became officially aware of a case through reporting or its own investigation.
* Column: Seasonal week (SW1 corresponds to week 27 of the seasonal calendar). Indicates the calendar week in which the case first became known.

## Outbreak Detection Methods
### Introduction
Most methods follow the same principle:
* Some model is set up to obtain a distribution of plausible values if nothing unusual is going on (“in control”).
* Some high quantile of this distribution (e.g., at 1 − α = 0.99) is chosen as a threshold value.
* -> If a newly incoming observation exceeds this threshold, an alarm is flagged.
Diffrence between methods: Mostly differ in the construction of the reference distribution.

Differentiate:
* Drop-in surveillance
  * Setting: a new threat / specific occasion requires a specific surveillance effort over a **short time**, very limited baseline data (≤ 7 days) are available
  * Goal: detecting aberrations relative to the **immediate past**
* Monitoring Seasonal Diseases:
  * Setting: has been monitored **for several years**, surveillance practices have remained comparable
  * Goal: detecting aberrations relative to **past seasonal patterns**

### Methods to compare
* Farrington Method: Suitable for seasonal disease data.
* CUSUM (Cumulative Sum Control Chart): Detects small shifts in data.
* ARIMA (Auto-Regressive Integrated Moving Average): Captures complex patterns in time series.

### Evaluation Metrics used
* Sensitivity (True Positive Rate): How effectively outbreaks are detected.
* Specificity (True Negative Rate): Avoiding false alarms.
* Detection Timeliness: Days required to identify an outbreak.
* Overall Accuracy: Combination of precision and recall.

## Optional: Nowcasting for Delay Correction
This step is optional and aims to explore new approaches for handling delays:
* Nowcasting:
  * Predict unreported cases using statistical models (e.g., regression or Bayesian methods).
  * Incorporate predictions into the outbreak detection process to improve accuracy.
* Evaluate if nowcasting can:
  * Mitigate the negative effects of delays.
  * Enhance detection sensitivity and timeliness in delayed datasets.

## Sources
* 1 [Kapitel Allevius und Höhle](https://arxiv.org/abs/1711.08960)
* 2 [Unkel et al](https://doi.org/10.1111/j.1467-985X.2011.00714.x)
* 3 [Paper Noufaily et al](https://onlinelibrary.wiley.com/doi/10.1002/sim.5595) (no reporting delays) --> Role model for own simulation setup ?
* Specific Outbreak Detection + Reporting Delays:
   * 4 [Salmon et al](https://onlinelibrary.wiley.com/doi/10.1002/bimj.201400159) --> R-package, evtl. "surveillance"
   * 5 [Noufaily et al](https://academic.oup.com/jrsssa/article/178/1/205/7058463)
   * 6 [Noufaily et al](https://www.tandfonline.com/doi/full/10.1080/01621459.2015.1119047)
* Nowcasting:
   * 7 [Wolffram et al](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1011394) --> Outbreak detection with a "frozen time series" (simple approach)

## Note: Paper
- Statistische Methoden und Ökonometrie, Institut für Statistik (STAT)
- Supervisor: Dr. Johannes Bracher
Presentation:
Paper:
* 15 – 20 pages (including figures and references, excluding title page, table of
contents, declarations)
* standard 12pt, 2cm margin format

## Note: Presentation

* Time: 15-20 minutes
* Not all points will apply equally to all topics, so don't worry too much if you focus more on some aspects than others.

Questions to address:
* What will the seminar thesis be about?
* Why is it interesting? Motivation?
* What's the current plan moving forward.
In general, you should cover the following points:
* the general idea / question of the topic
* brief descriptions of the used or planned to use methods (can be somewhat high-level, won't be able to give all details)
* the data which is planned to be use and the simulation setting one considers
* what one has done so far and what the next steps will be
* potentially some intermediate results or findings

## Note: Table of Contents Outline
**1. Introduction & Motivation**
*1.1 Aim of the Thesis*
- Assess the impact of reporting delays on outbreak detection.
- Compare outbreak detection methods with/without delays.
- Propose a model addressing reporting delays.
*1.2 Motivation*
- Importance of early outbreak detection for public health.
- Challenges posed by reporting delays.
- Relevance in bioterrorism and routine disease surveillance.

**2. Literature Review/Related Work**
- Overview of existing outbreak detection methods.
- Studies on reporting delays and mitigation techniques.

**3. Theoretical Foundation**
*3.1 Outbreak Detection Principles*
- Thresholds and reference distributions.
- Differences: drop-in surveillance vs. seasonal monitoring.
*3.2 Detection Methods*
- Farrington Method: For seasonal patterns.
- CUSUM: Detecting cumulative deviations.
- Time Series (ARIMA): Capturing complex trends.
*3.3 Reporting Delays*
- Definition, causes (e.g., testing, administrative lag).
- Impact on detection.
- Methods to manage delays.

**4. Data**
- Real-world datasets without reporting delays as benchmarks.
- Open sources: COVID-19, influenza surveillance.
- Selection of delay-free datasets for validation.

**5. Methodology**
*5.1 Simulation Setup*
- Simulated outbreak scenarios.
- Simulated reporting delays.

**6. Results**
- Comparison: Performance without delays vs. with uncorrected delays (diagrams, tables). Quantitative --> How much worse is detection with delays?
- Evaluation of nowcasting correction methods (diagrams, tables). Quantitative --> Which method is most effective?
- Sensitivity analysis: Variation in simulation parameters. Robustness --> How do results change with different parameters?

**7. Discussion**
- Key insights and practical relevance for epidemiological surveillance.
- Limitations: Assumptions, data availability, limitations of simulation.
- Future directions: Real-time data, machine learning approaches, new methods.

**8. Conclusion**
- Summary of findings.
- Implications for public health and future research.
