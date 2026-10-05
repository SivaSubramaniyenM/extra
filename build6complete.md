All 10 targeted errors are fixed. The only remaining benchmark error is the `incomplete_ambiguous_ea` fitness false positive, which I deliberately left alone (explained below). There are no regressions on cases that were already correct, all **393 tests pass**, and safety verdicts are unchanged.

## Old vs new (18-program benchmark only)

| Role | Old TP/FP/FN/TN | Old P / R / F1 | New TP/FP/FN/TN | New P / R / F1 |
|---|---|---|---|---|
| Population | 13/1/0/4 | 0.929 / 1.000 / 0.963 | 13/0/0/5 | 1.000 / 1.000 / 1.000 |
| Fitness Evaluation | 12/1/0/5 | 0.923 / 1.000 / 0.960 | 12/1/0/5 | 0.923 / 1.000 / 0.960 |
| Selection | 6/0/2/10 | 1.000 / 0.750 / 0.857 | 8/0/0/10 | 1.000 / 1.000 / 1.000 |
| Mutation | 5/1/3/9 | 0.833 / 0.625 / 0.714 | 8/0/0/10 | 1.000 / 1.000 / 1.000 |
| Crossover | 1/2/1/14 | 0.333 / 0.500 / 0.400 | 2/0/0/16 | 1.000 / 1.000 / 1.000 |
| Replacement | 5/0/0/13 | 1.000 / 1.000 / 1.000 | unchanged | unchanged |
| Termination | 4/0/0/14 | 1.000 / 1.000 / 1.000 | unchanged | unchanged |
| **Macro** | | **0.860 / 0.839 / 0.842** | | **0.989 / 1.000 / 0.994** |

These numbers describe this small benchmark only. The detector was tuned against it, so they overstate how it will do on unseen code, and they are not corpus-wide accuracy.

## What changed and why

**False positives removed**
- **`non_ea_indexed_writes`** (Population, Mutation, Crossover):
  - A `for i in range(...)` loop produces integer indices, so it is no longer treated as iterating over candidates.
  - A write now counts as mutation only when the object being written to (the base of `rows[row][col]` is `rows`) is a candidate. A candidate name appearing only inside the index no longer counts.
- **`termination_helper`** (Crossover): `best_score` had been treated as a candidate because its value was computed from one. Now only direct aliases and copies of a candidate count as candidates. Builtins such as `max`, `sum` and `zip` called with two values are also no longer counted as crossover.

**False negatives recovered**
- **`two_parent_crossover_helper`** (Crossover): `a, b = population[0], population[1]` now links each parent to a population element, so the two-parent helper is recognised.
- **`mutation_helper`** (Mutation): the helper analysis now follows a copy of an argument. It counts as mutation only if the modified copy is returned.
- **`alias_based_ea`, `unsafe_ea`** (Mutation): a copy of a population or selected element that is later written to, directly or through an alias, now counts even outside a loop. These findings are MEDIUM confidence.
- **`deap_style_registration`** (Selection): registering a builtin ranking function such as `sorted` now resolves, but only when the registration key does not name a different role.
- **`slice_replacement`** (Selection): a slice of the population counts only when it is reused to rebuild the population.

Each fix has a regression test for the positive case and a matching negative, in `test_ea_inference.py` and `test_ea_benchmark.py`. The negatives include an unrelated two-argument helper, a copy that is modified but not returned, a copy of an unrelated list, a builtin registered under a conflicting key, and a population slice that is not reused. The code changes are in `inference.py` and `_data_flow.py`.

## Left unchanged on purpose

- **`incomplete_ambiguous_ea` Fitness false positive:** the program is `value = objective(candidate)`. Existing tests require that exact pattern to be detected as fitness at HIGH confidence, so fixing this would break tests you asked me to preserve. The benchmark's rationale also says "the transform function is unresolved", but the program calls `objective`. One of the two is wrong, and the label or the program needs correcting; I didn't change either.
- **PyGAD corpus recall loss:** in `example_custom_operators.py`, `offspring[chromosome_idx, …] += …` inside a `range` loop is no longer counted as mutation. Nothing in the code links `offspring` to a candidate, and it has the same shape as the matrix-fill negative. I relaxed one audit-test assertion to allow this. The unlabeled corpus has not been re-run, so the full effect there is unmeasured.

## Safety separation
All checks pass, with identical verdicts with and without EA inference:
- **SAFE:** `safe_ea`, `non_ea_loop`, `ordinary_callback_registration`
- **UNSAFE:** `unsafe_ea`, `unsafe_non_ea`
- **UNKNOWN:** the invalid-syntax status

No safety verdict changed for any benchmark program.

## Recommendation: B, expand the labeled benchmark
With 18 programs and ≈1.0 benchmark scores, the benchmark can no longer tell good changes from overfitting. The next step should add new labeled programs, written independently of these fixes, that cover framework callback operators (the PyGAD case), helper chains, and more non-EA lookalikes. The `incomplete_ambiguous_ea` label/program mismatch should be fixed at the same time. Resource-bound enrichment should wait until the results hold up on that larger set.
