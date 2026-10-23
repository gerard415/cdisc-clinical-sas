# Project plan — CDISC Clinical SAS, end-to-end (CDISCPILOT01)

Picked 2026-10-23 (option 2 of the portfolio menu). Goal: a GitHub repo that shows
a full clinical-trial analysis pipeline in SAS — SDTM → ADaM → TLFs — on the real
CDISC pilot study, validated against the official finished datasets.

## The study

CDISCPILOT01: Phase 3, randomized, placebo-controlled trial of Xanomeline
(transdermal patches) in mild-to-moderate Alzheimer's disease.
Arms: Placebo (86) / Xanomeline Low Dose (84) / Xanomeline High Dose (84) = 254 randomized.
Primary efficacy endpoint: ADAS-Cog(11) change from baseline at Week 24.
Source (public, free): https://github.com/cdisc-org/sdtm-adam-pilot-project
(cloned locally to `~/workspace/your_files/cdisc-pilot-source/` — reference only,
never committed to this repo; the pilot data is cloned, not redistributed).

## Why this build order

ADSL is the foundation: every other ADaM dataset and every TLF inherits its
treatment variables and population flags. Then the safety dataset (ADAE),
then the efficacy dataset (ADQSADAS), then the outputs (TLFs).

## Phase 1 — ADSL (subject-level) ← START HERE

- Inputs: SDTM DM (306 rows incl. 52 screen failures), EX, DS, VS, MH, QS.
- Build: one row per randomized subject; TRT01P/TRT01A (+ numeric), TRTSDT/TRTEDT
  from EX, TRTDUR, AGEGR1, RACEN, baseline height/weight/BMI from VS, DISONSDT
  (disease onset) from MH, MMSETOT (baseline MMSE total) from QS.
- Population flags: SAFFL, ITTFL, EFFFL; completers COMP8FL / COMP16FL / COMP24FL;
  DISCONFL, DSRAEFL (discontinued due to AE), DTHFL.
- Spec: the pilot's own ADaM define.xml (in the source clone, analysis/adam/datasets/)
  lists all 48 target variables and their derivations. Work from the spec, not by
  peeking at the finished data.
- **Validation targets (from the official finished ADSL): 254 rows; arms 86/84/84;
  SAFFL = ITTFL = Y for all 254; COMP24FL: Y = 118, N = 136.**
  Compare variable-by-variable with PROC COMPARE at the end.

## Phase 2 — ADAE (adverse events, analysis-ready)

- Inputs: SDTM AE + SUPPAE, ADSL.
- Build: treatment-emergent flag (TRTEMFL) from AE start vs. TRTSDT, onset day
  (ASTDY), duration, severity/relatedness as collected, AESER etc.
- Validate against the official adae.xpt with PROC COMPARE.
- Note: the pilot repo ships its own adae.sas — ours is written independently
  from the define spec; the shipped one is a last-resort reference, not a template.

## Phase 3 — ADQSADAS (efficacy)

- Inputs: SDTM QS (ADAS-Cog items), ADSL.
- Build: item scores → total score per visit, BASE, CHG (change from baseline),
  analysis visits (AVISIT), ANL01FL; primary timepoint Week 24.
- Validate against the official adqsadas.xpt.

## Phase 4 — TLFs (the reviewer-visible outputs)

1. Table 1: Demographics & baseline characteristics by treatment arm (from ADSL).
2. Table 2: Adverse events overview — subjects with any TEAE, serious, severe,
   leading to discontinuation (from ADAE).
3. Table 3: Primary efficacy — ADAS-Cog change from baseline at Week 24,
   LS-means style summary by arm (from ADQSADAS).
4. Figure 1: Mean ADAS-Cog change over time by arm.
- Formats: RTF/PDF output via ODS; screenshots of 1–2 outputs go in the README.

## Phase 5 — Repo polish

- README: study table, pipeline diagram (folders), how to reproduce, validation note.
- Macros folder: pull repeated patterns (population counts, % macros) into %macros
  as they emerge — the pharma-gap closer.

## Working rules

- SAS runs the way coursework does: Emeth writes the program, Gerard runs it,
  output comes back, Emeth reviews/validates and writes the next step.
- Comments on their own spaced-out lines, never inline (Gerard's standing format).
- One dataset at a time, validated before moving on.
- Effort estimate: ~2 weekends total.
