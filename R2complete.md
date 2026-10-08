**Review 2 Freeze: Ready**, with duplicate findings recorded. The lock covers only reviewed, labeled programs; the 178 unreviewed programs are excluded.

- **Programs / reviewed:** 936 of 1,114 total.
- **By category:** `benign` 600/602 reviewed; `deap` 51/54; `generic_unsafe` 40/40; `llm_ea` 40/40; `llm_non_ea` 40/40; `non_deap` 85/258; `unsafe_ea` 40/40; `unsafe_non_ea` 40/40.
- **Safety labels:** 35 `SAFE`, 67 `UNSAFE`, 827 `UNKNOWN`, 7 `CONDITIONALLY_SAFE`.
- **EA-role positives:** population 207, fitness 209, selection 209, mutation 194, crossover 115, replacement 203, termination 172.
- **Validation:** zero syntax failures among reviewed programs; zero metadata, source-reference, unreadable-file, or duplicate-ID failures. Two syntax-invalid files remain outside the reviewed set: `catalan_numbers.py` and `egg_dropping.py` in the benign corpus.
- **Duplicates:** 3 exact-source groups (9 extra files) and 116 near-duplicate pairs. They were retained and recorded.

The SHA-256 dataset hash is `c40f92f8c0b49732a045469c1091bccf32d5d14360234456edc8fb7218680082`. Verification confirmed all 936 program hashes and 936 metadata hashes match the lock.

Created or refreshed: `dataset_manifest.json`, `dataset_validation_report.md`, and `dataset_lock.json`. No programs or metadata were modified during this validation and freeze; EvoSafe was not run.

Completed the three requested diagnostics from frozen R2 results. The implementation fingerprint matched the R2 run; no labels or code were changed.

- **Role errors:** 1,328 total: 361 FP and 967 FN. The largest groups were replacement/offspring flow (184 errors), selection/ranking (176), mutation (140), candidate fitness lineage (130), and population recognition (113). Comprehension-driven ordinary transforms caused 203 FPs; other non-EA lookalikes caused 148.
- **Safety independence:** Tested all 936 programs with EA inference off and on. Verdicts matched for **936/936**, with zero mismatches or experiment errors. This included 17 SAFE EA, 24 UNSAFE EA, 18 SAFE non-EA, and 43 UNSAFE non-EA cases.
- **Performance:** 936 successful analyses, zero failures; mean 0.014234 s, median 0.008158 s, p95 0.044081 s, maximum 0.261039 s. Timeouts were not separately instrumented. Mean source size was 67.8 lines and 283.4 AST nodes; CFG size was not persisted or recomputed.
- **Limitations:** Safety ground truth is UNKNOWN for 827 programs, so safety assessment is limited. Role recall was weakest for replacement, crossover, and selection. The frozen dataset also has 178 unreviewed programs excluded, plus recorded duplicate findings.

Reports: error analysis, safety independence, and performance. Diagnostic plots are in `plots`.

**R3 improvements to consider:** strengthen EA-context gating against non-EA lookalikes; improve selection and replacement flow recognition; extend candidate lineage through helpers and aliases; improve two-distinct-parent crossover detection; link termination conditions to the evolutionary process while handling callbacks and dynamic dispatch conservatively. These are recommendations only; no changes were implemented.

Frozen R2 Evaluation Complete

All 936 programs in dataset_lock.json were analyzed successfully. No source programs were executed, and no EvoSafe code or ground-truth labels were changed.

Dataset hash: c40f92f8c0b49732a045469c1091bccf32d5d14360234456edc8fb7218680082
EvoSafe revision: 0891f932119b21a0258f12a41441022e515e9aca
Implementation fingerprint: 72e0dd15c716bd8162203c34e66e61b86c90f4fed119c86ce26e42bf15fa21df
Environment: Python 3.12.3, Linux
Analysis: 936 successful, 0 failed
Raw safety predictions: 482 SAFE, 260 UNSAFE, 0 UNKNOWN, 194 CONDITIONALLY_SAFE
Safety mismatches: 829
Per-program EA predictions include confidence and evidence. Aggregate EA-role metrics are deferred to the independent metrics task.

Results are in r2_results.json and r2_results.csv; the run summary is r2_run_report.md.

Created the preliminary metrics, publication tables, and six plots:

Metrics JSON
Metrics CSV
Metrics report
Publication tables
Plots, safety by category, role F1, role precision/recall, category EA F1, version comparison
Results: On the 109 programs with determinate safety ground truth, accuracy is 98.2%. SAFE precision/recall/F1 are 0.971/0.971/0.971; UNSAFE are 1.000/0.985/0.992. There was 1 false SAFE, 0 false UNSAFE, and 0 analysis failures. The 827 UNKNOWN ground truths remain explicit; EvoSafe predicted UNKNOWN for none. Exact four-label accuracy across all 936 programs is 11.4%, so the determinate-only score should not be read without that coverage caveat.

EA-role macro precision/recall/F1 are 0.572/0.252/0.316. Replacement (F1 0.165), crossover (0.200), and selection (0.249) are weakest.

The saved comparison snapshots are clearly separated: v1 development 1.000, v1 historical evaluation 0.640, v2 development candidate 1.000, and current R2 0.316. These use different splits and are not paired performance deltas; the saved v2 candidate fingerprint also differs from the R2 run fingerprint.

The results support a preliminary PoC claim: the current pipeline completed all 936 analyses and emits usable safety and role predictions. They do not establish broad safety assurance or production readiness. No EvoSafe code, labels, or predictions were changed or rerun for these metrics.

Created and validated the requested reports:

- EA-role error analysis
- Safety independence
- Performance
- Error-category plot
- Performance histogram

The analysis covers all 1,328 EA-role error instances: 361 false positives and 967 false negatives. The main groups are comprehension/collection-transform lookalikes (203), replacement/offspring flow (184), selection/ranking (176), and non-EA lookalikes (148).

Safety verdicts matched with EA inference disabled and enabled for **936/936 programs**, with zero mismatches. Performance from the frozen run: 936 successes, no failures; mean 0.014234 s, median 0.008158 s, p95 0.044081 s, maximum 0.261039 s. CFG size was not persisted, so it is reported as unavailable rather than recomputed.

No EvoSafe code or dataset labels were changed.
