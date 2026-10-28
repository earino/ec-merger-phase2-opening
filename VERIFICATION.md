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

# Verification record

What was actually run, on which bytes, and what that does **not** cover. Every figure
below was read from a recorded artifact (gate report, baseline result, worker log),
not restated from memory.

## Bytes identity

The five shipped files are the corrected, re-qualified artifact of worker build
`ec-merger-003`, gate-1.5.0 artifact version
`e96929efbbc4fcb84d2986a7e32f83f9c5fec5395124439fb3404faff2e9e662`
(sha256 over `public/train.csv`, `public/eval.csv`, `private/holdout.csv`,
`meta.json`, `quality.json`). The packaged tree was rebuilt from the private
staging release parts (tag `job-ec-merger-003`) with every part's sha256 verified
on arrival before anything else was done.

**Pre-publication correction.** The build-time artifact was
`e5a98dfc9c05d0b0...`. The independent pre-flip audit (2026-10-27, PUBLISH WITH
CONDITIONS) required descriptor corrections; `meta.json` and `quality.json`
were regenerated (1,342 B / 21,855 B) with the three CSVs byte-identical to the
build. The corrected package was re-qualified by worker job `ecm-requal-002`
(gate 1.5.0, two-phase, over the exact accepted CSV bytes fetched from private
staging release 425292628) with `failed_checks: []`, and its accepted report
ships in this package as `qualification.json`. Both private destinations were
re-verified against the corrected bytes (GitHub release 425556351: 5/5
server-side digests; Hugging Face `main`: corrected files present with a
documented `load_dataset` check) before any public visibility was granted.

## What was run

1. **Construction + qualification + acceptance (worker, 2026-10-27).** Worker job
   `ec-merger-003` (server 172772422, cx23, 40.3 s, exit 0): fetched the
   publisher's daily register snapshot (42,034,960 B, sha256 `420411333e...`,
   last-modified 2026-10-27T01:23:54 GMT), built the extract, then the full
   sequence `construct -> qualify -> --accept` with the gate shipped in
   `code/qualify_dataset.py`: gate 1.5.0 passed 46/46 checks on the **full**
   artifact where the rows are, and exact-byte acceptance printed
   `ACCEPTED: /output/extract matches the qualified artifact e5a98dfc9c05d0b0`
   before any performance figure was computed. The construction stage computes
   no performance metric; the post-acceptance reference scores were computed only
   after acceptance by `code/score.py` (shipped in this release, byte
   digest recorded in `MANIFEST.json`; the paired-bootstrap recipe is
   `code/report.py`, same digest discipline).
2. **Runner-contract baseline (worker, 2026-10-27).** Worker job
   `ec-merger-baseline-001` (server 172773775, 78.5 s, exit 0) fetched the five
   accepted files from the private staging release (digest-verified on arrival),
   re-ACCEPTed them through the gate against the same collected qualification
   report (`ACCEPTED: ... e5a98dfc9c05d0b0`), materialized a runner workdir
   (public train+eval only; the holdout was never copied), and ran the
   benchmark's own `train.py` and `validate.sh` **unmodified** - the three
   runner files' sha256 digests bundled into the job equal the digests recorded
   here in `baseline/` (`train.py` `a3c6bcf1...`, `validate.py` `b597e7f8...`,
   `validate.sh` `3f06ca48...`; the job's report records `verbatim: true`).
   Result: `CONTRACT OK`; train.py and validate.py both print
   **Eval AUC 0.6973**; an independent predict_proba recompute agrees
   (0.69735, Brier 0.025343). All references and paired comparisons in
   `RELEASE_NOTES.md` come from this job's `baseline_result.json`, computed on
   the identical 1,770 eval rows.
3. **Local re-acceptance of the packaged bytes (coordinator, 2026-10-27).** The
   packaged tree re-verified through the same gate's `--accept` against the
   worker's collected qualification report, and `check-package.py` verified
   every document present and every manifest hash matching the bytes on disk.
4. **Corrected-bytes consumer re-verification (coordinator, 2026-10-28T07:22Z).**
   After the descriptor correction, `get_dataset.py` was executed end-to-end
   against the private GitHub release (token download via the API asset path,
   5 files, 860,971 bytes, every digest verified, runner layout restored),
   `sha256sum -c SHA256SUMS` passed on all five restored files, and
   `qualify_dataset.py --accept` against the accepted report returned
   `ACCEPTED: ... matches the qualified artifact e96929efbbc4fcb8`.
   `check-package.py` exit 0 on the regenerated manifest.

## What was NOT covered

- **The agent harness loop.** Nothing here runs a coding agent on the task; the
  baseline is the runner contract only. No claim is made about agent performance;
  the benchmark's own score (holdout AUC) is not evaluated in this package.
- **The holdout.** It was never scored, never copied into any workdir, and is
  excluded from every figure above. Its bytes ship verbatim; evaluating it is the
  benchmark's job, once, on a frozen design.
- **Point-in-time availability of the register.** The publisher retains no
  historical vintages (bulk feed since 2025-03-18; older daily objects expire),
  so a reverse-history probe is not performable. The evaluation contract declares
  retrospective research over the pinned current snapshot; the current-vintage
  sector taxonomy is a stated limitation, not a verified as-of property.
  Exception: one fixture (M.4731 GOOGLE/DOUBLECLICK) cross-checks the register's
  sector coding against the OJ prior notice published at notification.
- **Headroom above the sector prior.** The stock baseline trails the required
  comparator (0.6973 vs 0.7679; paired difference -0.0706, 95% CI [-0.1558,
  +0.0168] - inconclusive, observed gap negative). No claim is made that any
  particular model can beat the prior; equally, one weak stock model does not
  establish that none can. Both numbers are recorded beside each other so no
  reader quotes either alone.
- **Provider billing.** Worker reservations are conservative estimates (EUR 0.04
  for ec-merger-003, EUR 0.06 for the baseline job); actual provider costs are
  estimates, not read back invoices.

## Where the raw evidence lives

The gate's own qualification report ships with this package as `qualification.json`
(gate 1.5.0, the exact checks and evidence the worker executed over the full
artifact). The factory repository (private during review) preserves the full
candidate record with probes at `candidates/ec-merger-phase2-opening/`, the
baseline evidence at `candidates/ec-merger-phase2-opening/baseline/`
(`baseline_result.json`, `expected_artifact.json`), and the worker-written
staging manifests for both jobs in `earino/dataset-factory-staging` (releases
`job-ec-merger-003`, `job-ec-merger-baseline-001`), which is where the
collected gate report, references and baseline logs remain durably downloadable.
