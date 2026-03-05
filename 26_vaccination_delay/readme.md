# COVID-19 Vaccination Latency Study 

This repository contains the processed datasets used in the article:

[Institutional Trust and Vaccination Delay as Key Metrics for Vaccination Rollout Success] 

[Communications Medicine]

The data support analyses of vaccination latency for first and third COVID-19 doses using longitudinal survey responses, survival analyses and structural equation modeling (SEM).

## Files in this repository
## 1. CS_LONG_GITHUB.xlsx

### Purpose
Primary analysis dataset used in all latency models, including the latency SEM.

### Structure

* Long format (one row = one participant × one survey round)

* Key identifiers:

  * PID: participant ID

  * ROUND: survey wave (1–13)

### Contents

* Repeated measures:

  * Vaccine attitudes (5C components, trust, intentions)

  * Mental health indicators

  * Demographics (time-invariant, repeated across rounds)

* Vaccination information:

  * Vaccination dates (DateCD1, DateCD3)

  * Computed latency variables (CD1_DAY, CD3_DAY, etc.)

* Policy timing indicators (e.g., vaccine rollout, vaccine pass periods)

### Notes

* All variables required for the published latency SEM are contained in this file.

* No additional merging with other datasets is required for model estimation.

## 2. CS_MATCH_GITHUB.xlsx

### Purpose
Participant-level reference dataset used to construct vaccination timelines and eligibility logic.

### Structure

* One row per participant (PID)

### Contents

* Consolidated vaccination dates across survey waves

* COVID-19 infection history

* Final vaccination dose counts

* Auxiliary indicators used during latency construction

### Notes

* This dataset is not used directly in model estimation

* It supports transparency and traceability of vaccination timing variables in CS_LONG

## Relationship between datasets

* CS_LONG and CS_MATCH are linked by PID

* All SEM analyses are conducted only on CS_LONG

* CS_MATCH documents how vaccination dates and latency measures were derived

## Reproducing the analyses

To reproduce the published results:

* Load CS_LONG_GITHUB.xlsx

* Apply the case selection criteria described in the Methods section

* Fit the SEM models and survival analyses models as specified in the article

No additional raw data files are required.

## Data processing and scope

* These datasets are processed research data, not raw survey exports

* Cleaning steps include:

  * Harmonization across survey waves

  * Resolution of inconsistent vaccination dates

  * Construction of policy-adjusted latency measures

* Survey waves beyond Round 13 were retained for internal consistency but are excluded from analyses, as stated in the article

## Ethical approval and data access

* Data were collected with institutional ethical approval

* All data are anonymized prior to sharing

* Participant IDs are non-identifiable study codes

## Code used to generate manuscript figures

The Kaplan–Meier survival curves reported in **Figure 1** and **Figure 2** are generated in the survival analysis section of the R script.

### Figure 1(a): Kaplan–Meier, First Dose (overall)

This panel is produced by the block under:

```
############################### SURVIVAL ANALYSIS ##################################
######## FIRST DOSE ########
```

Key code lines:

```r
CS_NEARVACall$surv_obj <- with(CS_NEARVACall, Surv(CD1_DAY, rep(1, nrow(CS_NEARVACall))))
km_fit <- survfit(surv_obj ~ 1, data = CS_NEARVACall)

plot(km_fit,
     main="Kaplan-Meier Estimate of Vaccination Delay",
     xlab="Days",
     ylab="Probability of not being vaccinated of the 1st dose")

abline(v = meanDaysToVR, col="blue", lty=2)
abline(v = meanDaysToVP, col="red", lty=2)
abline(v = meanDaysToAnnouncement, col="green", lty=2)
```

---

### Figure 1(b): Kaplan–Meier, Third Dose (overall)

This panel is produced by the block under:

```
############################### SURVIVAL ANALYSIS ##################################
######### THIRD DOSE ##########
####### ADD ANNOUNCEMENT DATE (USE IN MANUSCRIPT)
```

Key code lines:

```r
CS_NEAR3VACall$surv_obj <- with(CS_NEAR3VACall, Surv(CD3_DAY, rep(1, nrow(CS_NEAR3VACall))))
km_fit <- survfit(surv_obj ~ 1, data = CS_NEAR3VACall)

plot(km_fit,
     main="Kaplan-Meier Estimate of Vaccination Delay",
     xlab="Days",
     ylab="Probability of not being vaccinated of the booster")

abline(v = meanDaysToVP, col="red", lty=2)
abline(v = meanDaysToAnnouncement, col="green", lty=2)
```

---

### Figure 2(a): Kaplan–Meier, First Dose by Age Group

This panel is produced by the block under:

```
######## AGE SPECIFIC SURVIVAL CURVE ##########
####### FIRST DOSE #######
```

Key code lines:

```r
CS_NEARVACall$surv_obj <- with(CS_NEARVACall, Surv(CD1_DAY, rep(1, nrow(CS_NEARVACall))))
km_fit_age <- survfit(surv_obj ~ AGE, data = CS_NEARVACall)

plot(km_fit_age,
     col = 1:6,
     lty = 1:6,
     xlab = "Days",
     ylab = "Probability of not being vaccinated of the 1st dose",
     main = "Kaplan-Meier Estimate by Age Group")
```

---

### Figure 2(b): Kaplan–Meier, Third Dose by Age Group

This panel is produced by the block under:

```
######## AGE SPECIFIC SURVIVAL CURVE ##########
####### THIRD DOSE #######
```

Key code lines:

```r
CS_NEAR3VACall$surv_obj <- with(CS_NEAR3VACall, Surv(CD3_DAY, rep(1, nrow(CS_NEAR3VACall))))
km_fit_age <- survfit(surv_obj ~ AGE, data = CS_NEAR3VACall)

plot(km_fit_age,
     col = 1:6,
     lty = 1:6,
     xlab = "Days",
     ylab = "Probability of not being vaccinated of the booster",
     main = "Kaplan-Meier Estimate by Age Group")
```

---

## Key variables used in survival analyses

| Variable | Description |
|---------|-------------|
| `PID` | Unique participant identifier |
| `ROUND` | Survey wave number |
| `DateCD1` | Date of first COVID-19 vaccine dose |
| `DateCD3` | Date of third (booster) dose |
| `CD1_DAY` | Vaccination latency in days for the first dose |
| `CD3_DAY` | Vaccination latency in days for the third dose |
| `AGE` | Age group used for stratified survival curves |
| `meanDaysToVR` | Time point of vaccination requirement implementation |
| `meanDaysToVP` | Time point of vaccine pass implementation |
| `meanDaysToAnnouncement` | Time point of vaccine pass announcement |
