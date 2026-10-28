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

# Data dictionary

One row per merger case notified to the European Commission under the EU Merger
Regulation (caseType `Merger` with a recorded `caseNotificationDate`), as carried by
the DG COMP machine-readable case register (open-data bulk feed `case-data-M.json`,
vintage of 2026-10-27). 10,091 shipped rows of 10,125 eligible; 33 pending cases
(no terminal decision yet: 29 with no decisions at all, 3 Art. 9(3) partial referrals,
1 Art. 4(4) partial referral) are censored out and counted, never labelled.

## Columns

| Column | Role | Definition |
| --- | --- | --- |
| `case_number` | id | The register's case number (e.g. `M.10188`), one row per case. |
| `phase2_opened` | **target** | 1 if the case carries at least one `Art. 6(1)(c)` decision adopted on or after the notification date (ever-adopted; a later withdrawal does not undo the opener; `'Withdrawal of Art. 6(1)(c)'` decisions never match), else 0. Positive rate: train 4.01%, eval 2.60%, holdout 1.58%. |
| `notification_date` | time column | The case's notification date (`caseNotificationDate`, date-only precision, Brussels local). The prediction instant: every feature is fixed when the file is registered. |
| `notification_year` | feature | Calendar year of the notification date. |
| `notification_month` | feature | Calendar month (1-12) of the notification date. |
| `regulation` | feature | The applicable merger regulation: `4064/89` or `139/2004` (`caseRegulation`). |
| `sector_divisions` | feature | Sorted set of NACE division labels for the parties' activities, `'\|'`-joined, or `NONE` when the register carries no sectors (79 cases measured). Derived from `caseSectors`; division = first two components of the label's code part (e.g. `C.20.5 - ...` -> `C.20`). Register coding is the current-vintage NACE Rev 2 taxonomy (see the vintage note in the README). |
| `sector_count` | feature | Number of `caseSectors` entries on the case. |
| `has_sectors` | feature | 1 when the case carries sectors, 0 for the `NONE` cases. |
| `party_count` | feature | Number of segments of the case title split on `/` with optional surrounding whitespace. |
| `title_has_jv` | feature | 1 when the title carries a `JV` token (joint ventures). |
| `case_title` | feature | The case title (`caseTitle`): the merging parties as named by the register, an immutable historical fact (the OJ prior notice for M.4731 GOOGLE/DOUBLECLICK documents the parties and their activities at notification). High-cardinality free text. |

## Excluded by construction (never shipped)

`caseSimplified` (upgrade leak: a simplified filing upgraded to Normal during review
would make the current value post-hoc), and the post-hoc / mutable / non-pinned fields
`caseInvestigationPhase`, `caseDeadlineDate`, `caseLastDecisionDate`, `caseLinks`,
`caseCourtCases`, `caseRenotificationDate`, `caseInitiationDate`, `caseAttachments`.
The label source - the case's `decisions[]` block (decision types, adoption dates) -
is excluded wholesale from the shipped columns.

## Splits

Temporal on `notification_year`: train 1990-2013 (5,406 rows / 217 positives),
eval 2014-2018 (1,770 / 46), holdout 2019-snapshot (2,915 / 46). Distinct positive
notification dates: 45 in eval, 44 in holdout. The register's coverage starts
November 1990.
