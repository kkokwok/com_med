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
