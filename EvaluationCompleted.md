## Evaluation completed

The evaluation utility analyzes Python source without executing it. It supports directories or explicit files, preserves per-role evidence, emits CSV and JSON, and accepts optional role labels for future precision/recall/F1 calculations. Detector and Safety IR code were not modified.

### Corpus discovered

The default run analyzed **2,562 programs**: 2,545 analyzed successfully and 17 failed parsing under the active Python version. The 17 failures include legacy Python 2-style exception syntax; they are analysis errors, not detector misses.

The corpus contains:

- **340 framework-associated Python files:** DEAP 55, PyGAD 72, pymoo 146, and mealpy 67.
- **2 generic EA programs** in the educational genetic-algorithm folder.
- **622 files** in the safety-oriented benign corpus. “Benign” is not treated as proof that a file is non-EA.
- **1,580** other educational Python files and **18** unannotated example, test, or root programs.
- No Python files in the `unsafe_ea`, `unsafe_non_ea`, `generic_unsafe`, `llm_ea`, or `llm_non_ea` folders.
- Eight notebooks were found but not analyzed. Exact inventory is in the report.
- No exact EvoLab or EvoC examples were found.

Repository directory labels do not provide role-level ground truth, so corpus-wide false-positive/false-negative rates and precision/F1 are unavailable.

### Role results

Counts below are across successfully analyzed programs; “not detected” does not imply a false negative without role labels.

| Role | Detected | Not detected | Confidence among detections |
|---|---:|---:|---|
| Population | 413 | 2,132 | 411 HIGH, 2 MEDIUM |
| Fitness evaluation | 335 | 2,210 | 331 HIGH, 4 MEDIUM |
| Selection | 63 | 2,482 | 9 HIGH, 54 MEDIUM |
| Mutation | 106 | 2,439 | 98 HIGH, 8 MEDIUM |
| Crossover | 81 | 2,464 | 3 HIGH, 78 MEDIUM |
| Replacement | 31 | 2,514 | 31 HIGH |
| Termination | 106 | 2,439 | 46 HIGH, 60 MEDIUM |

The framework-specific breakdowns and per-program evidence are in the generated reports.

### Manually reviewed misses

I source-audited two representative EA programs: DEAP OneMax and the generic `basic_string.py`. Both explicitly implement all seven roles; the current detectors detected none in either. This is a **two-program positive-only spot check**: it yields two false negatives per role and recall of 0 on those examples. There are no labeled negative examples, so precision and F1 cannot be estimated. This is not a corpus-wide accuracy estimate.

The DEAP example registers the population and operators through the toolbox, uses a convergence/generation `while` condition, and replaces the population by slice assignment ([OneMax source](dataset/deap/ga/onemax.py#L45-L139)). Likely causes vary by role: registered operator calls and indirect candidate relationships obscure population, evaluation, selection, mutation, and crossover; the population alias is not linked to replacement; and the termination condition is not linked to the population/fitness flow.

The generic example distributes its algorithm across helper functions, making helper/call-site and data-flow linkage a concrete weakness. Other missed roles in framework-associated corpus files are **review candidates, not confirmed misses**; their file-level category does not specify which roles are implemented. In particular, some PyGAD population management and termination behavior, and pymoo/mealpy algorithm loops, are delegated to framework APIs rather than represented in the source file. The report distinguishes these abstraction boundaries from proven detector errors.

### Safety independence

All five checks passed with verdicts unchanged when EA inference was enabled: SAFE EA, UNSAFE EA, SAFE non-EA, UNSAFE non-EA, and an EA program with incomplete role recognition. The broad corpus also produced static verdict counts, but those verdicts are not ground-truth safety labels.

### Deliverables and tests

- Evaluation runner: `evaluate_ea_corpus.py:27-136`
- JSON results: `ea_corpus_results.json`
- CSV results: `ea_corpus_results.csv`
- Human-readable report: `ea_corpus_summary.md`
- Runner tests: `test_ea_evaluation.py:22-100`

The full suite passed: **338 tests**. No new corpus dataset, resource enrichment, detector changes, or IR changes were made.

Created 7 todos

Made changes.
