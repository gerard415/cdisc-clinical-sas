# Clinical SAS End-to-End — SDTM → ADaM → TLFs on the CDISC Pilot Study

An end-to-end clinical trial analysis in **SAS**, built on the public CDISC pilot
study (CDISCPILOT01): raw tabulation data (SDTM) → analysis datasets (ADaM) →
submission-style tables and a figure (TLFs), with every derived dataset validated
against the official CDISC finished datasets using PROC COMPARE.

## The study

| | |
|---|---|
| Design | Phase 3, randomized, double-blind, placebo-controlled |
| Treatment | Xanomeline transdermal patches — Placebo (n=86), Low Dose (n=84), High Dose (n=84) |
| Population | 254 randomized subjects with mild-to-moderate Alzheimer's disease |
| Primary endpoint | ADAS-Cog(11) change from baseline at Week 24 |
| Source data | CDISC SDTM/ADaM pilot package (public): github.com/cdisc-org/sdtm-adam-pilot-project |

## Pipeline

```
sdtm/ (DM, EX, DS, AE, QS, VS, MH + supplements)
  → programs/adam/adsl.sas      subject-level: treatments, dates, population flags
  → programs/adam/adae.sas      adverse events: treatment-emergent flags, onset days
  → programs/adam/adqsadas.sas  efficacy: ADAS-Cog totals, baseline, change from baseline
  → programs/tlf/               demographics table · AE overview · primary efficacy table · figure
```

Derivations follow the pilot's ADaM define.xml specifications (variable-level
metadata: origin and derivation method for all 48 ADSL variables).

## Validation

Each ADaM dataset is compared record-by-record against the official CDISC
finished dataset (PROC COMPARE). ADSL checkpoints: 254 subjects, arms 86/84/84,
Week-24 completers 118.

## Reproduce

1. Clone the CDISC pilot package (link above); point the `sdtm`/`adam` libnames in
   `programs/config.sas` at the tabulations folder.
2. Run the ADaM programs in order (adsl → adae → adqsadas), then the TLF programs.

## Skills demonstrated

SAS (data step, PROC SQL/MEANS/FREQ/REPORT, ODS RTF), CDISC SDTM and ADaM
standards, define.xml-driven derivation specs, clinical trial populations and
flags (ITT / safety / completers), treatment-emergent AE logic, and
submission-style TLF production.
