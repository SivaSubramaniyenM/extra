Updated todo list

Agreed. I'll do this in three strictly ordered stages so the held-out result is credible:

1. Resolve the `incomplete_ambiguous_ea` contradiction in the development benchmark, then freeze detector v1 (source fingerprints + dev metrics).
2. Write and **label-lock** the new held-out benchmark (hashes recorded) *before* the detector ever sees it.
3. Run the frozen detector once against it and report, without touching detector code.

**Stage 1 — the inconsistency.** The program's own docstring says *"candidate scoring"*, the original spec explicitly lists `score = objective(individual)` as fitness evidence, and existing tests assert that pattern is fitness. The contradiction came from my earlier label edit, which changed the label and rationale but not the program. So the correct fix is to restore the positive label; the genuinely-unresolved case (`transform(candidate)`) will be tested in the held-out set instead. 

Replacing 4 lines with 4 lines in 


```

```


Read , lines 1 to 120

Read , lines 120 to 200

Replacing 35 lines with 63 lines in 


```

```


Replacing 3 lines with 8 lines in 


```

```


Created 11 todos

Now regenerate the development results and write the v1 freeze record (detector fingerprints plus both the original 0.994 and the corrected-label dev result).  

Ran terminal command:  cd /home/intel/intel/evosafe && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python scripts/evaluate_ea_benchmark.py && /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q 2>&1 | grep -E '^(Ran|OK|FAILED)' && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
import json
r=json.load(open('evaluation/ea_role_benchmark/results.json'))
print(r['detector_fingerprint']); print(r['manifest_sha256']); print(r['metrics']['macro_average'])
print(r['safety_separation']['all_programs_verdict_unchanged'], r['safety_separation']['all_checks_pass'])
PY

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Compacted conversation

Completed: *Add regression tests for fixed errors* (1/8)

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Created 

Next I need the manifest format and the validation rules.  

Searched for regex `def _validate_manifest|def _negative_rationale|safety_expectation|positive_rationale|"path"|program\[|def evaluate_manifest` (`**/scripts/evaluate_ea_benchmark.py`), 19 results

Read , lines 1 to 60

Read , lines 40 to 120

Writing the held-out manifest. Each program gets explicit labels for all seven roles, plus a positive or negative rationale for every role. 

Created 

Labels are written. Before creating the lock, I'll validate the manifest without running the detector, hash every file, and check that the detector fingerprint still matches the frozen v1 values. 

Ran terminal command:  cd /home/intel/intel/evosafe && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'EOF'
import json, hashlib, sys
from pathlib import Path
sys.path.insert(0, "scripts")
import evaluate_ea_benchmark as e
root = Path("evaluation/ea_role_benchmark_holdout")
m = json.loads((root/"benchmark.json").read_text())
e._validate_manifest(m)
files = sorted(p.name for p in (root/"programs").glob("*.py"))
listed = sorted(Path(p["path"]).name for p in m["programs"])
print("programs:", len(m["programs"]), "files:", len(files), "match:", files == listed)
print("positives per role:", {r: sum(p["ground_truth"][r] for p in m["programs"]) for r in e.ROLE_NAMES})
lock = {"manifest_sha256": hashlib.sha256((root/"benchmark.json").read_bytes()).hexdigest(),
        "programs": {f"programs/{f}": hashlib.sha256((root/"programs"/f).read_bytes()).hexdigest() for f in files}}
print(json.dumps(lock, indent=2))
frozen = json.loads(Path("evaluation/ea_role_benchmark/frozen_v1.json").read_text())
print("detector:", e.detector_fingerprint())
print("frozen keys:", list(frozen))
EOF

There are 51 programs, and the detector fingerprint matches frozen v1 exactly. Writing the label lock before the first detector run.  

Created 

Labels are locked. Now the single run of the frozen v1 detector on the held-out set: 

Ran terminal command:  cd /home/intel/intel/evosafe && H=evaluation/ea_role_benchmark_holdout && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python scripts/evaluate_ea_benchmark.py $H/benchmark.json --json-output $H/results.json --csv-output $H/results.csv --summary-output $H/results.md > /dev/null && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'EOF'
import json
r = json.load(open("evaluation/ea_role_benchmark_holdout/results.json"))
print([k for k in r])
m = r["metrics"]
for role, v in m["per_role"].items():
    print(f"{role:20} TP={v['tp']:2} FP={v['fp']:2} FN={v['fn']:2} TN={v['tn']:2} P={v['precision']} R={v['recall']} F1={v['f1']}")
print("macro", m["macro_average"])
print("safety", json.dumps(r.get("safety_separation"), default=str)[:1500])
print("fingerprint", r["detector_fingerprint"])
for rec in r["programs"]:
    errs = [f"{role}:{'FP' if p['detected'] else 'FN'}" for role, p in rec["predictions"].items() if p["detected"] != rec["ground_truth"][role]]
    if errs: print(rec["program_name"], errs)
EOF

Ran terminal command:  cd /home/intel/intel/evosafe && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'EOF'
import json
r = json.load(open("evaluation/ea_role_benchmark_holdout/results.json"))
recs = {x["program_name"]: x for x in r["programs"]}
for name, roles in [("c01_uniform_crossover",["crossover"]),("d01_direct_while_convergence",["crossover"]),("d03_direct_numpy_es",["population","fitness_evaluation"]),("sf01_safe_ea",["population","fitness_evaluation"]),("h03_helper_chain_crossover_replace",["population"]),("n05_record_processing",["mutation","selection"])]:
    for role in roles:
        p = recs[name]["predictions"][role]
        print(name, role, {k: p[k] for k in p if k != "evidence"}, str(p.get("evidence"))[:200])
EOF

Searched for regex `^import|^from|^class|^ROOT|MANIFEST|def test_manifest` (`**/tests/test_ea_benchmark.py`), 15 results

Read , lines 1 to 20

Replacing 4 lines with 30 lines in 


```

```


Ran terminal command:  cd /home/intel/intel/evosafe && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q 2>&1 | tail -4 && sha256sum analysis/ea_inference/inference.py analysis/ea_inference/_data_flow.py analysis/ea_inference/_model.py && git status --short analysis/ | head

Ran terminal command:  cd /home/intel/intel/evosafe && PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q 2>&1 | grep -E "^(Ran|OK|FAILED)"

The frozen v1 detector scores much lower on the new held-out set: **macro F1 0.640**, compared with 0.994 on the development set (1.000 after the label correction). Detector files are unchanged (fingerprints match `frozen_v1.json`), all 395 tests pass, and safety verdicts are unaffected.

## Held-out benchmark
- **Programs:** 51 new programs in `ea_role_benchmark_holdout`, covering all the categories you listed.
- **Labels:** every role is labeled explicitly, with a reason for each positive and each negative label.
- **Lock:** I locked the labels in `labels.lock.json` (hashes of the manifest, all 51 programs and the detector) before running the detector once. Two new tests check the manifest and the lock.

## Results

| Role | TP | FP | FN | TN | P | R | F1 |
|---|---|---|---|---|---|---|---|
| population | 28 | 5 | 7 | 11 | 0.848 | 0.800 | 0.824 |
| fitness_evaluation | 22 | 5 | 14 | 10 | 0.815 | 0.611 | 0.698 |
| selection | 6 | 2 | 10 | 33 | 0.750 | 0.375 | 0.500 |
| mutation | 9 | 3 | 10 | 29 | 0.750 | 0.474 | 0.581 |
| crossover | 0 | 0 | 8 | 43 | — | 0.000 | — |
| replacement | 4 | 1 | 7 | 39 | 0.800 | 0.364 | 0.500 |
| termination | 7 | 1 | 4 | 39 | 0.875 | 0.636 | 0.737 |

The evaluator's macro average leaves out crossover because its precision is undefined (it predicted nothing), which flatters the result:

| | Precision | Recall | F1 |
|---|---|---|---|
| 6 roles (evaluator default) | 0.806 | 0.543 | 0.640 |
| All 7 roles, crossover F1 counted as 0 | — | 0.466 | 0.548 |

Precision holds up reasonably well; recall is the main problem.

**On the development benchmark the detector reached 0.994 macro F1; on the independently built held-out benchmark it reached 0.640 (0.548 counting crossover as 0), mainly because recall dropped.**

## What went wrong
1. **Population not recognised:** in d03, h03, sf01, c03, m04, s04 and r03 the detector never identified the population. That happens when candidates are scored by NumPy calls, sort keys, `heapq` or index lookups instead of a plain `for x in coll:` loop. Every other role in those programs then goes undetected too.
2. **Crossover is never detected (0/8):** it was missed in uniform crossover in a list comprehension, blend crossover in a helper, `np.concatenate` of two parent slices, a `Genome.cross(other)` method, a splice inside an inline loop, and the two engine callbacks. Crossover on the development set only worked for the patterns it was tuned on.
3. **Several mutation styles are missed:**
   - swaps (`a[i], a[j] = ...`);
   - helpers that return a new perturbed list;
   - operators called through `ops"mut"`, `getattr`, or one built with `eval`;
   - writes made through a chain of helpers;
   - `parents + noise` in NumPy.
4. **Framework callbacks:** fitness, mutation, crossover and selection functions passed to an `Engine` as keyword arguments are not recognised. Only the registration style seen in the development set is handled.
5. **Selection and replacement:**
   - **Missed:** `heapq.nlargest`, `scores.index(min(...))`, `population[-1] = ...`, `remove` + `append`, and reassignment from a list comprehension (sf02).
   - **False positive:** `min` used only to track the best value (t02).
6. **False positives on non-EA code:**
   - record processing (n05: a dict field write plus `sorted[:1]`);
   - cache eviction with a slice assignment (n09);
   - SGD training (n03);
   - an unresolved `transform(candidate)` call (x01);
   - a plain f-string comprehension (sf04).

   The detector treats any per-item loop or comprehension as candidate scoring.

## Safety
All 51 programs got the same verdict with and without EA inference. All 7 programs that have an expected verdict matched it: SAFE for n01, sf01 and sf04; UNSAFE for sf02, sf03, sf05 and x03.

## Caveats
- I wrote both benchmarks, so author bias is still possible, although the labels were locked before the run.
- 51 programs is a small set, and crossover has only 8 positive cases.
- Some labels are judgment calls: t01 (counting a deterministic in-place update as mutation) and t03 (no fitness, because only a population-wide total is recorded).
- These are benchmark scores, not corpus-wide precision or recall.

## Suggested next steps (detector not changed)
1. Recognise a population from more than plain loops: index access, NumPy row operations, and being used as a sort, `heapq` or `min`/`max` key target. This is the biggest single gain because the other roles depend on it.
2. Change crossover detection to ask whether a single output depends on two different elements of the population, whatever the syntax.
3. Recognise callables passed to constructors as keyword arguments, and dispatch through dicts and `getattr`.
4. Make "any per-item loop counts as scoring" stricter.

Any v2 should be scored on a further unseen set, because this held-out set has now been seen.

Results are in `results.md`, `results.json` and `results.csv`.
