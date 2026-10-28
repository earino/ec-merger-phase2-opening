<!-- dataset-factory:links -->
**Dataset Factory · v2026.10**

[Dataset website](https://earino.github.io/ec-merger-phase2-opening/versions/v2026.10/) · [GitHub release](https://github.com/earino/ec-merger-phase2-opening/releases/tag/v2026.10) · [Hugging Face](https://huggingface.co/datasets/earino/ec-merger-phase2-opening/tree/v2026.10) · [Catalogue](https://earino.github.io/dataset-factory-site/)

Artifact: e96929efbbc4fcb84d2986a7e32f83f9c5fec5395124439fb3404faff2e9e662

### Suitability and evaluation requirements

Retained for retrospective model-development research. Every shipped feature is a same-row case attribute fixed at notification (an administrative record), and eligibility reads only notification-time fields, but the register publishes a single current snapshot with no historical vintages, so training-label timing and full-row historical availability are unverified and no operational or point-in-time forecasting claim is supported.

**Supported uses:** Retrospective train/validation model-development experiments under the documented timing, vintage and selection limitations; Final evaluation only under a protocol that isolates the labeled holdout from agent development and prevents any later row influencing an earlier prediction

**Not supported:** Claims of verified real-time, operational or point-in-time forecasting validity; Treating development/validation (eval-split) scores as untouched final-test results; Presenting current-vintage register values (notably the NACE Rev 2 sector coding) as the values an analyst at notification time would have seen

**Evaluation access:** isolated rows. The labeled eval split is development/validation data. Keep the final holdout outside agent development; prevent later rows from influencing earlier predictions.

**Historical availability:** partial. **Training-label timing:** unverified. **Vintage:** current snapshot. **Selection:** prediction time.

**Cross-row audit:** bounded no recovery. Gate 1.5.0 executed its two declared probes on the full artifact on the worker across all three splits (unique case_number donor; repeat case_title with a 365-day offset): 0 rows covered, 0 mismatches, 0 ambiguous on both probes and all splits. Rows are unique notified cases and every feature is a same-row attribute, so no outcome-bearing cross-row exposure is constructed; the probes are a bounded mechanism, not a proof, and no broader attack search was run.

Structural qualification and baseline scores do not certify operational forecasting validity.

- The register retains no historical vintages (bulk feed since 2025-03-18; S3 objects expire after 30 days), so a reverse-history probe is not performable: this is retrospective research over the pinned 2026-10-27 snapshot, never a certified historical forecasting setup.
- Feature availability is fixture-verified at three cross-era points (M.877 1997, M.4731 2007 with its OJ prior notice, M.10188 2021) but not verified row-by-row against notification-era vintages; the sector taxonomy labels are current-vintage NACE Rev 2 applied across all eras.
- Training-label timing is unverified: labels are the register's current values, observed as of the fetch snapshot, with no availability ledger.
- Positives cluster by enforcement era and sector acquisition waves beyond the day-level event key; per-notification-year positive counts (1-22, median about 8) are reported beside every score.
- Prevalence falls across splits (train 4.01%, eval 2.60%, holdout 1.58%): the eval and holdout windows are prevalence-shifted relative to training, an out-of-distribution property reported, not hidden.
- Pending cases with neither an opener nor a terminal decision at fetch (33, overwhelmingly live 2020s filings) are censored and counted per split and year; their labels are unobserved, not negative, and censoring never decides another row's membership.
- The ever-adopted label keeps M.10188 positive despite its 2024-09-06 post-ECJ withdrawal of the opener; M.3333 stays positive on its 2007-03-01 opener with the 2004 pre-notification opener excluded by rule.
- The stock runner baseline trails the required sector-division comparator on the identical eval rows (0.6973 vs 0.7679; paired difference -0.0706, 95% CI [-0.1558, +0.0168], inconclusive with the observed gap negative). Headroom above that prior is NOT demonstrated by the packaged evidence; both numbers must be quoted together.

**Corrections to the historical documentation:**

- 2026-10-27/28 descriptor correction per the independent pre-flip audit (PUBLISH WITH CONDITIONS): meta.json and quality.json regenerated (retrospective-use honesty, metadata contradiction fixes, recipes shipped, repeated-transaction dependence disclosed); superseded artifact e5a98dfc9c05d0b0..., CSVs byte-identical; re-qualified as e96929ef by worker job ecm-requal-002 before any public visibility

Review: 2026-10-28; applies to v2026.10 / `e96929efbbc4fcb84d2986a7e32f83f9c5fec5395124439fb3404faff2e9e662`. Frozen data and tags are unchanged.
<!-- /dataset-factory:links -->

# Release notes - v2026.10

First release of `ec-merger-phase2-opening`, one task instance, one artifact.

## What this is

A retrospective binary classification task over the European Commission's
machine-readable merger case register (DG COMP open data): at a case's
notification date, from case attributes fixed that day, predict whether the
Commission will open an in-depth Phase II investigation (Art. 6(1)(c)).
The register publishes one current snapshot with no historical vintages, so the
evaluation contract is retrospective research over the pinned 2026-10-27
vintage; no point-in-time reconstruction is claimed (sector coding is the
register's current-vintage taxonomy).

## Artifact identity

- Artifact version (gate 1.5.0 digest over the five shipped files):
  `e96929efbbc4fcb84d2986a7e32f83f9c5fec5395124439fb3404faff2e9e662`
- Pre-publication correction: the independent pre-flip audit (2026-10-27)
  required descriptor corrections; `meta.json` and `quality.json` were
  regenerated and the package re-qualified (gate 1.5.0, `failed_checks: []`,
  worker job `ecm-requal-002`) before release. The three CSVs are
  byte-identical to the worker build `ec-merger-003`.
- Source snapshot: `case-data-M.json`, 42,034,960 B, sha256 `420411333e9b9fc27e0c2e72cbbbb419f1ba4125e7f40d98a9bc5e0911316702`,
  last-modified Tue 27 Oct 2026 01:23:54 GMT.
- Construction commit: `54428d2` (worker job `ec-merger-003`; the exact
  `code/build.py` / `code/run.sh` / `source/score.py` byte digests it executed
  are recorded in `MANIFEST.json`).
- Qualification: gate 1.5.0, 46/46 checks on the full artifact on the worker,
  exact-byte `--accept` verified before any figure was computed.
- Two prior attempts are preserved as failure records: `ec-merger-001`
  (gate refused two descriptor truths; declaration-only fix) and
  `ec-merger-002` (runner-compatibility defect: its `meta.json` lacked the
  `description`/`split` keys the benchmark's `bench/workdir.py` reads; the CSV
  bytes were unchanged by the fix - train/eval sha256 identical).

## Measured evidence (eval window, 1,770 rows / 46 positives, after acceptance)

| Reference / model | Eval AUC | Brier |
| --- | --- | --- |
| Full model (stock XGBoost baseline, verbatim runner) | 0.6973 | 0.025343 |
| Sector-division prior (train rate per NACE division) - required comparator | 0.7679 | 0.025008 |
| Sector-combination prior (train rate per exact division set) | 0.7051 | 0.026192 |
| Title-token prior | 0.6644 | 0.026111 |
| Calendar/identity prior (regulation x month) | 0.5606 | 0.025295 |
| Recent-era constant (last three train years) | 0.5000 | 0.025314 |
| Global train-rate constant | 0.5 (no ranking information) | 0.025514 |

Majority-class accuracy on this split: 0.9740 (base rate 2.60% - accuracy is
uninformative here; AUC is the metric).

Paired row-bootstrap differences (1000 resamples, identical rows):

- full model - sector prior: **-0.0706**, 95% CI [-0.1558, +0.0168] -
  the interval contains zero (inconclusive difference) and the observed gap is
  **negative**: the stock baseline trails the required comparator. Recorded as
  a finding, not repaired: the runner ships verbatim, and one weak model does
  not establish that no useful prediction is possible.
- full model - calendar prior: +0.1367 [0.0419, 0.2321]
- full model - recent-era constant: +0.1973 [0.1068, 0.2745]
- sector prior - calendar prior: +0.2073 [0.1255, 0.2886] - the sector signal is
  real, and it is the bar a solver must clear.

Per-year (eval window; 7-12 positives per year, so these are noisy):

| Year | rows | positives | model AUC | sector prior AUC |
| --- | --- | --- | --- | --- |
| 2014 | 298 | 9 | 0.6724 | 0.7053 |
| 2015 | 331 | 11 | 0.7486 | 0.6862 |
| 2016 | 358 | 7 | 0.9113 | 0.8386 |
| 2017 | 375 | 7 | 0.6002 | 0.7552 |
| 2018 | 408 | 12 | 0.5921 | 0.8607 |

Prevalence shift across splits (train 4.01% -> eval 2.60% -> holdout 1.58%) is a
property of the falling Phase II rate over the enforcement era; the holdout is
an out-of-distribution test in prevalence as much as in time. The holdout is
sealed: it ships as an asset, is never copied into a runner workdir, and the
benchmark's score is holdout AUC.

## Known limitations

- Current-vintage register coding for sectors; the register retains no
  historical vintages, so an as-of-1995 coding cannot be verified (one OJ
  prior-notice fixture, M.4731 GOOGLE/DOUBLECLICK, documents that the
  notification-time activities correspond to the register's coding).
- 79 cases carry no sectors (`NONE`, flagged by `has_sectors`).
- One pre-notification opener case (M.3333 SONY/BMG re-notification) is handled
  by a recorded rule: label decisions predating the notification do not count;
  the case stays positive on its 2007-03-01 opener.
- 34 pending cases are censored (counted per split in the build summary; 26 of
  them are live 2026 filings - recent-year labels are still maturing; the count
  was corrected from 33 by the independent pre-flip audit, 2026-10-27).
- Eval positives are thin (46 over five years); the per-year AUCs above are
  correspondingly uncertain.
