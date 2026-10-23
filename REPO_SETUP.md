# Repo setup — cdisc-clinical-sas

Status: LIVE — created 2026-10-23 at https://github.com/gerard415/cdisc-clinical-sas (public).
Local scaffold in this folder mirrors the repo; new work gets committed as it's built.

Local layout:

```
cdisc-clinical-sas/
  README.md            GitHub-facing summary (study, pipeline, validation)
  PROJECT_PLAN.md      phased build order + validation targets
  programs/adam/       adsl.sas, adae.sas, adqsadas.sas (+ config.sas libnames)
  programs/tlf/        demographics, AE overview, efficacy table, figure
  macros/              shared %macros (built as patterns repeat)
  spec-notes/          derivation notes worked out from the pilot define.xml
  output/              RTF/PDF TLF outputs (gitignored except README screenshots)
```

Reference clone (NOT part of the repo, never commit):
`~/workspace/your_files/cdisc-pilot-source/` — SDTM .xpt inputs live under
`updated-pilot-submission-package/900172/m5/datasets/cdiscpilot01/tabulations/sdtm/`,
finished ADaM + define.xml under `.../analysis/adam/datasets/`.

Next: build Phase 1 (ADSL) — see PROJECT_PLAN.md for the build order and
validation targets.
