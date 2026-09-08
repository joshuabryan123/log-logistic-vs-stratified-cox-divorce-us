# 💍 Marriage Dissolution Survival Analysis: Log-Logistic AFT vs. Stratified Cox Model in the U.S.

This repository contains the complete analytical framework, code, and findings for the study evaluating marriage dissolution in the United States using survival analysis techniques. The study specifically compares the parametric **Log-Logistic Accelerated Failure Time (AFT)** model against the semi-parametric **Stratified Cox Proportional Hazards** model.

---

## 📌 Background

Marriage dissolution is a critical socio-economic issue in the United States, impacting family structures, mental health, and financial stability. According to the National Center for Family and Marriage Research, the U.S. recorded a divorce rate of 14.56 per 1,000 married women in 2022, placing it among the highest globally. 

Understanding the factors influencing marriage longevity is vital:
* **Socio-demographic Variations:** Risk of divorce is unevenly distributed across education levels, race, spousal age differences, and intermarriage types.
* **Economic Consequences:** Divorce can increase poverty risks, reducing family income by 28% to 42% for single mothers.
* **Non-Monotonic Hazard Pattern:** Risk of divorce is non-constant over time—it typically peaks within the first few years of marriage before declining (unimodal pattern). Standard models assuming proportional hazards (PH) across all timeframes often fail to capture this complex dynamic without modification.

This research addresses these modeling challenges by evaluating parametric and semi-parametric techniques tailored for non-proportional hazards and unimodal distribution patterns.

---

## 📁 Dataset & Preprocessing

The research utilizes secondary data from the **Princeton University Marriage Dissolution Dataset** comprising **3,371 married couples**.

### Data Overview
* **Sample Size:** 3,371 observations
* **Event Rate:** 30.6% observed divorce events ($n = 1,032$)
* **Censoring Rate:** 69.4% right-censored ($n = 2,338$)
* **Time Metric:** Duration of marriage in years (`years`)

### Variables & Features
* **`div`** (Target): Divorce indicator ($1 = \text{divorced}, 0 = \text{censored}$)
* **`heduc`**: Husband's education level ($<12 \text{ years}, 12\text{--}15 \text{ years}, >15 \text{ years}$)
* **`heblack`**: Husband's race indicator ($1 = \text{Black}, 0 = \text{Otherwise}$)
* **`mixed`**: Interracial marriage status ($1 = \text{Mixed race}, 0 = \text{Same race}$)
* **`agediff`**: Age difference between spouses ($\text{Older Husband}, \text{Older Wife}, \text{Peer}$)

### Preprocessing Steps
* Fixed data entry issues (e.g., corrected `"15-Dec"` to `"12-15"` years category in `heduc`).
* Structured categorical predictors into appropriate order factor levels.
* Verified zero missing values across duration and target fields.

---

## 🛠️ Methodology

The analytical framework follows a structured survival analysis pipeline:
1. **Non-Parametric Estimation:** Plotted overall Kaplan-Meier survival curves, hazard rates, and cumulative hazards.
2. **Hypothesis Testing:** Conducted Log-Rank tests to check for survival curve differences across sub-groups.
3. **Proportional Hazards Assumption Testing:** Applied scaled Schoenfeld residual tests and $Log(-log(S(t)))$ vs. $Log(t)$ visual plots.
   * **Findings:** The husband's race (`heblack`) violated the PH assumption ($p = 0.031$), necessitating either a stratified approach or an Accelerated Failure Time (AFT) framework.
4. **Model Development:**
   * **Stratified Cox Model:** Stratified by `heblack` to handle the PH violation while estimating partial likelihoods for remaining covariates.
   * **Log-Logistic AFT Model:** Parametric modeling utilizing the log-logistic distribution (shape parameter $\sigma / p = 1.04$) to naturally fit the observed unimodal peak in hazard risk.

---

## 📊 Results & Comparative Analysis

### 1. Model Performance Comparison

Comparing performance metrics shows clear superiority for the parametric Log-Logistic AFT model over the Stratified Cox model:

| Evaluation Metric | Log-Logistic AFT | Stratified Cox | Best Model Indicator |
| :--- | :---: | :---: | :---: |
| **Log-Likelihood** | **-5,179.01** | -7,268.12 | 🏆 Higher Log-Likelihood |
| **AIC** | **10,374.02** | 14,546.24 | 🏆 Significantly Lower AIC |
| **Significant Predictors** | **5 variables** | 3 variables | 🏆 Higher Sensitivity to Predictors |

---

### 2. Best Model Interpretation (Log-Logistic AFT)

The Log-Logistic AFT model provides direct insights through Acceleration Factors ($\gamma = e^\beta$). An acceleration factor $\gamma < 1$ indicates a faster transition toward divorce (shortened time to divorce) relative to the reference group:

* **Peak Risk Timing:** The hazard function reaches its maximum risk peak between **years 3.98 and 7** of marriage before steadily declining.
* **Husband's Education ($12\text{--}15\text{ years}$):** $\gamma = 0.67$ ($p < 0.001$). Time to divorce is **32.9% faster** compared to husbands with $<12$ years of education.
* **Husband's Race ($\text{Black}$):** $\gamma = 0.80$ ($p = 0.010$). Time to divorce is **20.5% faster** compared to non-Black husbands.
* **Marriage Type ($\text{Interracial/Mixed}$):** $\gamma = 0.81$ ($p = 0.030$). Time to divorce is **18.9% shorter** compared to same-race marriages.
* **Age Difference ($\text{Older Wife}$):** $\gamma = 0.64$ ($p = 0.030$). Time to divorce is **36.5% shorter** compared to marriages with an older husband.
* **Age Difference ($\text{Peer}$):** $\gamma = 0.63$ ($p < 0.001$). Time to divorce is **37.3% shorter** compared to marriages with an older husband.

---

## 💡 Conclusions & Recommendations

### 🎯 Key Conclusions
1. **Model Selection:** The **Log-Logistic AFT model** outperforms the Stratified Cox model across statistical parameters (lower AIC: 10,374.02 vs. 14,546.24) and better captures non-monotonic, unimodal marriage failure risks.
2. **Hazard Behavior:** Risk of divorce is non-constant over time; it increases rapidly in early years, peaks around years 4 to 7, and declines as the duration of marriage increases beyond 30 years.
3. **Primary Risk Accelerators:** Peer couples (similar age) and older-wife couples face the largest acceleration towards divorce relative to older-husband couples. Educational differences, interracial dynamics, and racial background also significantly accelerate time to divorce.

### 🔮 Recommendations for Future Research
* **Incorporate Latent Variables:** Integrate additional non-demographic variables such as income/financial distress, household communication metrics, personality compatibility, and social support structures.
* **Advanced Modeling Approaches:** Explore Machine Learning Survival Analysis techniques (e.g., Random Survival Forests, DeepSurv) or models with time-varying covariates.
* **Dataset Expansion:** Apply the methodology to more recent longitudinal cohort datasets to evaluate changing contemporary marriage stability patterns.

---

## 👥 Authors

* **Joshua Bryan Wijaya** (Corresponding Author: joshuabryan@apps.ipb.ac.id)
* **Zifa Aura Rahman**
* **Gemala Aleida Fitri**
* **Muhammad Fauzan Nur Rasendriya**

*Department of Statistics and Data Science, IPB University, Indonesia*
