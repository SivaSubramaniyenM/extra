Build 1
Updated todo list

Analyze EA detector failure modes

Read 

Read 

Searched for regex `role|mutation|crossover|selection|fitness|population|generation|ea` (`**/tests/test_capability_analyzer.py`), 9 results

Read memory

Read 

Read 

Searched for files matching `**/dataset/non_deap/{pygad,pymoo,mealpy}/**/*`, 20 matches

Searched for files matching `**/*{call,flow,summary,cfg}*.py`, 9 matches

Read , lines 1 to 420

Read 

Read 

Read , lines 1 to 300

Read , lines 1 to 500

Searched for regex `"dataset/non_deap/(pygad|pymoo|mealpy)|non_deap/(pygad|pymoo|mealpy)|role_counts|fitness_evaluation` (`**/evaluation/ea_corpus_results.json`), 401 results

Read , lines 360 to 457

Read 

Read 

Read , lines 1 to 360

Read , lines 1 to 440

Read 

Read 

Searched codebase for "Search repository for interprocedural analysis, call graph construction, function summaries, alias/dataflow tracking, points-to propagation, helper function analysis or summary tables. Include resource analyzer call-depth logic and taint engine.", 13 results

Read 

Read 

Read , lines 1 to 420

Read , lines 1 to 400

Searched for files matching `**/analysis/safety_ir/*.py`, 3 matches

Searched for files matching `**/dataset/non_deap/{pygad,pymoo,mealpy}/**/*.py`, 285 matches

Searched for regex `call_graph|summary|alias|interprocedural|function` (`**/analysis/ea_inference/**`), 14 results

Read , lines 1 to 250

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -c 'import json; d=json.load(open("evaluation/ea_corpus_results.json")); roles=("population","fitness_evaluation","selection","mutation","crossover","replacement","termination"); names=("dataset/deap/ga/onemax.py","Python/genetic_algorithm/basic_string.py");
for r in d["programs"]:
 if r["program_name"] in names: print("CONFIRMED",r["program_name"],"detected",[x for x in roles if r[x]["detected"]],"errors",r["error"])
for fw in ("PyGAD","pymoo","mealpy"):
 rows=[r for r in d["programs"] if r["program_category"].get("framework")==fw and not r.get("error")]; rows.sort(key=lambda r:sum(r[x]["detected"] for x in roles),reverse=True); print("FRAMEWORK",fw,"best:");
 for r in rows[:8]: print(sum(r[x]["detected"] for x in roles),r["program_name"],[x for x in roles if r[x]["detected"]])'

Searched for regex `register\(|toolbox\.|while|max\(fits\)|fitness|offspring|mate|mutate|select` (`**/dataset/deap/ga/onemax.py`), 46 results

Searched for regex `def evaluate|def crossover|def mutate|def select|def basic|while True|population_score|population.clear|population.extend|return` (`**/Python/genetic_algorithm/basic_string.py`), 28 results

Read , lines 1 to 280

Read 

Read 

Read 

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -c 'import json; d=json.load(open("evaluation/ea_corpus_results.json")); names={"dataset/deap/ga/onemax.py","Python/genetic_algorithm/basic_string.py","dataset/non_deap/pygad/example_custom_operators.py","dataset/non_deap/pymoo/algorithms/moo/nsga2/nsga2_custom.py","dataset/non_deap/mealpy/applications/keras/mha-hybrid-mlp-classification.py"}; roles=("population","fitness_evaluation","selection","mutation","crossover","replacement","termination");
for r in d["programs"]:
 if r["program_name"] not in names: continue
 print("\n###",r["program_name"],"error=",r["error"])
 for role in roles:
  x=r[role]; print(role,x["detected"],x["confidence"],"term=",x["termination_kind"])
  for e in x["evidence"][:5]: print(" ",e.get("line"),e.get("ast_node"),e.get("kind"),repr(e.get("snippet")))'

Ran terminal command:  rg -n '^(def _call_name|def _names|class _EAInference|    def (_make_cfg_refs|_detect_population_and_fitness|_detect_selection|_detect_mutation|_detect_crossover|_detect_replacement|_detect_termination|run)|def analyze_ea_roles|class Test(PopulationAndFitness|Selection|MutationAndCrossover|Replacement|Termination)|def _collect_functions|def _build_call_graph|def _expand_user_call|class CFGBuilder|def _process_loop)' analysis/ea_inference/inference.py analysis/ea_inference/_model.py tests/test_ea_inference.py cfg/cfg_builder.py analysis/resource/resource_estimator.py analysis/taint/taint_engine.py; rg -n '^(def evaluate|def crossover|def mutate|def select|def basic)|while True|population_score = \[evaluate|population.clear|population.extend|return \(generation|toolbox.register|pop = toolbox.population|fitnesses = list\(map|while max\(fits\)|offspring = toolbox.select|toolbox.mate|toolbox.mutate|pop\[:\] = offspring' Python/genetic_algorithm/basic_string.py dataset/deap/ga/onemax.py; rg -n 'def fitness_func|def parent_selection_func|def crossover_func|def mutation_func|ga_instance =|ga_instance.run|def _evaluate|class MySampling|class MyCrossover|class MyMutation|def _do|algorithm =|res = minimize|class HybridMlp|def create_problem|def training|def fitness_function|for ws in weight_sizes' dataset/non_deap/pygad/example_custom_operators.py dataset/non_deap/pymoo/algorithms/moo/nsga2/nsga2_custom.py dataset/non_deap/mealpy/applications/keras/mha-hybrid-mlp-classification.py; rg -n 'def _collect_functions|def _build_call_graph|def _call_name|MAX_CALL_DEPTH|def _expand_user_call|def _process_loop|def _make_cfg_refs|def _detect_population_and_fitness|def _detect_selection|def _detect_mutation|def _detect_crossover|def _detect_replacement|def _detect_termination' analysis/resource/resource_estimator.py analysis/taint/taint_engine.py cfg/cfg_builder.py analysis/ea_inference/inference.py

Ran terminal command:  grep -nE '^(def _call_name|def _names|class _EAInference|    def (_make_cfg_refs|_detect_population_and_fitness|_detect_selection|_detect_mutation|_detect_crossover|_detect_replacement|_detect_termination|run)|def analyze_ea_roles|class Test(PopulationAndFitness|Selection|MutationAndCrossover|Replacement|Termination)|def _collect_functions|def _build_call_graph|def _expand_user_call|class CFGBuilder|def _process_loop)' analysis/ea_inference/inference.py analysis/ea_inference/_model.py tests/test_ea_inference.py cfg/cfg_builder.py analysis/resource/resource_estimator.py analysis/taint/taint_engine.py; grep -nE '^(def evaluate|def crossover|def mutate|def select|def basic)|while True|population_score = \[evaluate|population.clear|population.extend|return \(generation|toolbox.register|pop = toolbox.population|fitnesses = list\(map|while max\(fits\)|offspring = toolbox.select|toolbox.mate|toolbox.mutate|pop\[:\] = offspring' Python/genetic_algorithm/basic_string.py dataset/deap/ga/onemax.py; grep -nE 'def fitness_func|def parent_selection_func|def crossover_func|def mutation_func|ga_instance =|ga_instance.run|def _evaluate|class MySampling|class MyCrossover|class MyMutation|def _do|algorithm =|res = minimize|class HybridMlp|def create_problem|def training|def fitness_function|for ws in weight_sizes' dataset/non_deap/pygad/example_custom_operators.py dataset/non_deap/pymoo/algorithms/moo/nsga2/nsga2_custom.py dataset/non_deap/mealpy/applications/keras/mha-hybrid-mlp-classification.py; grep -nE 'def _collect_functions|def _build_call_graph|def _call_name|MAX_CALL_DEPTH|def _expand_user_call|def _process_loop|def _make_cfg_refs|def _detect_population_and_fitness|def _detect_selection|def _detect_mutation|def _detect_crossover|def _detect_replacement|def _detect_termination' analysis/resource/resource_estimator.py analysis/taint/taint_engine.py cfg/cfg_builder.py analysis/ea_inference/inference.py

Read 

Read 

Read 

Read 

Read , lines 80 to 135

Read 

Read , lines 115 to 180

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
import json
p=json.load(open('evaluation/ea_corpus_results.json'))
for name in ['dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py','dataset/non_deap/pygad/example_custom_operators.py','dataset/non_deap/pymoo/algorithms/moo/nsga2/nsga2_custom.py','dataset/non_deap/mealpy/applications/keras/mha-hybrid-mlp-classification.py']:
 x=next((r for r in p['programs'] if r['program_name']==name),None)
 print('\n'+name, x and x['safety_verdict'])
 if x:
  for role in ('population','fitness_evaluation','selection','mutation','crossover','replacement','termination'):
   y=x[role]
   print(role, y['detected'], y['confidence'], [(e.get('line'),e.get('kind'),e.get('snippet','')[:65]) for e in y['evidence'][:2]])
PY

## A. Failure-analysis report

**No code was changed.** This inspection confirms both manually source-audited programs have all seven roles, while the current inference report detects none for either. The generated corpus results are useful for finding review cases, but—except for the specific source audits below—corpus directories do not provide role-by-role ground truth.

### Existing implementation

The seven detector passes are methods in `inference.py:177-418`. The analyzer uses Python AST, but its CFG use is currently for attaching block references to evidence; it does not derive reaching definitions, data flow, or call relationships from CFG edges (`inference.py:98-122`). Several helpers for names, calls, and loop containment also live in the inference module.

The closest existing reusable analysis capabilities are limited:

- `resource_estimator.py:89-117` builds a direct-name call graph for recursion and call depth. It does not resolve attribute calls, aliases, callbacks, or argument/return relationships.
- `taint_engine.py:117-168` tracks a local environment and collects function definitions; `taint_engine.py:533-599` is bounded and specific to taint sources/sinks. It is a useful implementation reference, but not a general-purpose EA fact engine.
- `cfg_builder.py:194-240` represents branches and loops, but functions are treated as opaque statements in a CFG; the builder does not provide data-flow or call-graph semantics.
- EA evidence is created in `inference.py:129-160`. Confidence is a heuristic ordinal: `HIGH` when a detector adds high-strength structural evidence, otherwise `MEDIUM` or the initial `LOW`; it is not calibrated probability (`inference.py:162-170`).
- Current unit tests emphasize compact, same-scope patterns; neither fully audited program is a regression fixture yet (`test_ea_inference.py:25-223`).

### Missed roles in DEAP OneMax

Source: `onemax.py:36-139`

| Role | Source construct | Why the detector misses it | Weakness classification |
|---|---|---|---|
| Population | Individual and population factories are registered, then called as `toolbox.population(n=300)` (`onemax.py:36-45`, `onemax.py:77-78`). | Population detection requires an explicit candidate-using collection loop; the factory call and its returned collection are not modeled. | Framework registration; indirect call; aliasing/data-flow gap; detector rule limitation |
| Fitness evaluation | `evalOneMax(individual)` computes `sum(individual)`, is registered as `evaluate`, then invoked through `map(toolbox.evaluate, ...)` ([function and registration](dataset/deap/ga/onemax.py#L47-L55), `onemax.py:88-90`, `onemax.py:130-134`). | The computation is in a helper; callback registration and `map` do not propagate candidate-to-score evidence. | Helper function; framework registration; indirect call; interprocedural flow; detector rule limitation |
| Selection | `toolbox.select(pop, len(pop))` invokes the registered tournament selector (`onemax.py:64-68`, `onemax.py:106-109`). | Selection recognizes selected built-in ranking/filter/sampling calls on an already inferred population, not registered operator invocations. | Framework registration; indirect call; detector rule limitation |
| Mutation | `toolbox.mutate(mutant)` invokes the registered `mutFlipBit` operator (`onemax.py:58-62`, `onemax.py:123-128`). | The mutation implementation is in DEAP, so the source shows an indirect call but no visible candidate element write. | Framework registration; indirect call; detector rule limitation; framework boundary |
| Crossover | `toolbox.mate(child1, child2)` invokes a registered two-parent operator inside the offspring loop (`onemax.py:58-60`, `onemax.py:111-120`). | The two arguments derive from zip/slice-selected offspring; neither their parent lineage nor the operator binding is resolved. | Framework registration; aliasing; indirect call; interprocedural flow; detector rule limitation |
| Replacement | `pop[:] = offspring` replaces the population (`onemax.py:138-139`). | `pop` was not recognized as a population because its factory result was not tracked. | Framework registration; aliasing; data-flow gap; detector rule limitation |
| Termination | `while max(fits) < 100 and g < 1000` combines convergence and a generation bound (`onemax.py:94-107`). | Termination is considered only for loops already associated with recognized EA findings; there is no link from `fits` to evaluations/population. | Interprocedural/data-flow gap; detector rule limitation; CFG currently provides no such semantic link |

### Missed roles in generic basic-string GA

Source: `basic_string.py`

| Role | Source construct | Why the detector misses it | Weakness classification |
|---|---|---|---|
| Population | `basic()` builds a list with random candidates, then scores it via a list comprehension ([construction and score comprehension](Python/genetic_algorithm/basic_string.py#L127-L163)). | Population inference focuses on `for`/`async for` candidate-using loops, not population construction followed by a candidate-scoring comprehension. | Helper/function scope; detector rule limitation; data-flow gap |
| Fitness evaluation | `evaluate(item, target)` computes a score; `basic()` calls it inside a list comprehension (`basic_string.py:24-32`, `basic_string.py:159-163`). | The analyzer does not treat the comprehension as candidate iteration or summarize `evaluate()` as a candidate-to-score transformer. | Helper function; interprocedural flow; detector rule limitation |
| Selection | `select()` chooses a parent by random indexing and `basic()` sorts scores before calling `select()` (`basic_string.py:62-94`, `basic_string.py:162-188`). | `random.randint` indexing is not recognized as selection; ranking and candidate identities are not linked across helper parameters. | Helper function; interprocedural flow; detector rule limitation |
| Mutation | `mutate()` makes a list copy and changes one indexed gene; it is called by `select()` ([mutation helper](Python/genetic_algorithm/basic_string.py#L48-L58), `basic_string.py:77-93`). | Mutation detection requires a loop over an already inferred population and does not summarize helper effects or offspring returns. | Helper function; aliasing; interprocedural flow; detector rule limitation |
| Crossover | `crossover(parent_1, parent_2)` slices and combines two parents; `select()` invokes it (`basic_string.py:35-45`, `basic_string.py:77-84`). | The function-local parent parameters are not known to represent candidates because the analyzer does not pass role/lineage facts into helper parameters. | Helper function; interprocedural flow; aliasing; detector rule limitation |
| Replacement | `basic()` retains part of the old population, clears it, extends survivors and generated children (`basic_string.py:176-188`). | Replacement depends on recognized population, selection, and offspring identities; those facts do not flow through `select()` or list mutations. | Helper function; aliasing; interprocedural flow; detector rule limitation |
| Termination | The `while True` generation loop exits when the best scored string matches the target (`basic_string.py:141-164`). | The loop is not associated with EA evidence because score/evaluation facts are hidden in the comprehension and helper. The detector therefore never classifies its exit condition. | Helper/function scope; interprocedural flow; detector rule limitation |

For both source-audited files, the misses are **not inherently ambiguous**: the implementations explicitly show all seven roles. The difficulty is connecting those constructs. In contrast, framework-only examples that delegate all operator or loop behavior to a library may not expose the implementation in the file being analyzed; those are source-scope limitations, not necessarily detector misses.

### Additional framework-associated examples inspected

These were selected because their results include several role hits or structurally plausible EA logic. A reported hit is not necessarily correct evidence for the role.

- **PyGAD custom operators:** `example_custom_operators.py:16-74` has callbacks for fitness, parent selection, crossover, and mutation, then registers them in `pygad.GA(...)` and calls `run()`. The current report detects population, fitness, selection, and mutation, but evidence inspection shows likely misattributions: the “fitness” evidence is a parent-array copy; “selection” evidence is choosing a random gene index. Crossover is expressed through indexed array-slice assignment that current rules miss; replacement and process termination are delegated to the framework.
- **pymoo custom NSGA-II:** `nsga2_custom.py:21-112` defines problem evaluation, sampling, two-parent crossover, mutation, and delegates the algorithm to `minimize()`. The report labels several unrelated constructs as population, fitness, replacement, and termination; its evidence comes from sampler/mutation loops and writes, not necessarily the role indicated. Framework-managed selection/replacement/termination are not visible as direct source logic. Also, the mutation detector reads `node.target` for `ast.Assign`, although assignments store targets in `node.targets`; this likely prevents recognizing indexed assignments in this example ([mutation pass](analysis/ea_inference/inference.py#L272-L310)).
- **mealpy hybrid MLP:** `mha-hybrid-mlp-classification.py:39-85` delegates optimization to `optimizer.solve(problem)`. The report labels a `weight_sizes` loop and `np.reshape(solution, ...)` as population and fitness evidence; these are network-weight decoding operations, not EA candidate evaluation. This is a concrete warning that generic candidate-loop/call patterns can produce false positives.

The exact findings and evidence snippets are in `ea_corpus_results.json`; summary counts and reviewed examples are in `ea_corpus_summary.md`.

## B. Proposed shared analysis architecture

The reusable missing capability is a **conservative, shared program-facts layer** that establishes relationships between expressions, variables, calls, and scopes. Adding independent heuristics to each role pass would duplicate alias and call reasoning and likely worsen false positives.

Proposed design, without framework-specific rules:

1. **Function/scope index:** map function and method definitions, parameters, returns, call sites, assignments, and source spans. Build a bounded call graph for direct local calls and simple assigned callables.
2. **Intra-procedural flow facts:** derive assignment/definition-use, collection lineage, and element/attribute writes using AST plus CFG block order. Start with a small, conservative data-flow domain; do not claim full Python semantics.
3. **Alias and lineage relations:** preserve facts such as `population -> offspring list`, `child -> copy(candidate)`, and `score -> objective(candidate)`. Handle simple name aliases, tuple unpacking, slices/subscripts, and mutating collection methods.
4. **Comprehension and built-in summaries:** model `map`, `zip`, `sorted`, comprehensions, and common list operations as generic structural transformers; link input candidates to output elements where justified.
5. **Helper summaries:** infer parameter/return/effect summaries for local functions: candidate-to-score, candidate-to-modified-candidate, pair-of-candidates-to-child, and collection effects. Apply summaries at call sites with a depth/iteration limit; unknown calls remain unknown.
6. **Indirect calls:** resolve only safe, syntactically supported cases, e.g. `alias = local_function`, callback supplied in a keyword argument, or a locally registered callable with a visible binding and call. Do not treat a framework import/name alone as proof of a role.
7. **Role detectors consume facts:** keep the seven role definitions and thresholds separate; make detectors query shared facts instead of reimplementing name and alias scans. Evidence should retain the fact chain and source locations that justified the role.
8. **CFG use:** initially use existing CFG for ordering and loop/branch context, not as a claim of precise path-sensitive semantics. Add richer CFG support only if a defined fact cannot be derived reliably from current blocks/edges.

This should be an internal analysis layer, not a competing IR and not a change to Safety IR or safety verdicting.

## C. Prioritized implementation plan

| Priority | Improvement | Expected role impact | Complexity | False-positive risk | Resource-analysis usefulness |
|---|---|---:|---:|---:|---:|
| 1 | Intraprocedural definitions/uses, simple aliases, collection lineage, comprehension and indexed/slice-write handling | Very high across all roles; particularly population, fitness, mutation, crossover, replacement | Medium–high | Medium; reduce with conservative propagation and targeted negatives | High: population-size, allocations, loop-variable relationships |
| 2 | Bounded helper summaries for parameters, returns, and collection effects | Very high for generic EA and helper-based programs; all roles | High | Medium; summaries must be context-bounded and unknown-preserving | High: call/loop and allocation summaries also aid resource reasoning |
| 3 | Local indirect-call resolution and generic callback/registration patterns | High for DEAP/PyGAD patterns, and partially other frameworks | Medium–high | Medium–high if names/registration alone trigger roles; require visible callable binding and use | Medium: callable/call-depth relationships |
| 4 | Fix AST-shape gaps and improve evidence attribution, including `Assign.targets`; distinguish scores from arbitrary candidate-derived values | Medium–high, especially mutation/selection/crossover and false-positive control | Low–medium | Low–medium if paired with negative tests | Medium |
| 5 | Use CFG facts for loop termination, branch exits, and reaching definitions where necessary | Highest for termination, supporting role context for others | Medium | Medium; runtime bounds still uncertain | Very high |

The sequence is deliberate: local data flow and summaries should precede callback-specific logic. Otherwise framework/operator names risk becoming role assertions without confirming that the callable is used or that its values flow into the relevant operation.

## D. Exact files likely to change in a later implementation

- New shared module such as analysis/ea_inference/program_facts.py for scope indexes, definitions/uses, alias and lineage facts, and generic call summaries.
- `inference.py` to query those shared facts and correct the `Assign` target handling; do not change Safety IR.
- `test_ea_inference.py` for role regressions, evidence, and false-positive controls.
- Potentially `resource_estimator.py` only if a narrowly reusable scope/call-graph utility is factored out; avoid modifying resource behavior in the same change.
- Potentially `cfg_builder.py` only if later requirements need control-flow facts not exposed by existing blocks/edges. Current evidence suggests the primary failures are flow/call-scope gaps, not missing basic CFG structure.

No Safety IR files need to change for this proposed detector improvement.

## E. Tests to add

1. **Confirmed positives:** DEAP OneMax and generic `basic_string.py`, asserting the seven roles and evidence locations; the tests should expose the current misses before any future fix.
2. **Regression per cause:** toolbox/callback registration with visible use; helper candidate-to-score return; helper pair-to-child return; helper mutation effect; collection returned from helper; aliases through assignment; `pop[:] = offspring`; `clear()`/`extend()` survivor replacement; comprehension scoring/filtering; `map`/`zip`; condition-linked termination.
3. **Data-flow boundaries:** one-hop alias and helper calls first; explicit tests showing deeper/dynamic flows remain unknown rather than being overclaimed.
4. **False-positive controls:** unrelated loops over model weights, generic indexed array writes, string/list concatenation, random gene-index selection, arbitrary callback registration, and collection modification without candidate lineage.
5. **Framework-associated cases:** PyGAD callbacks and pymoo custom operators as structural tests, asserting evidence corresponds to the source role and not just any loop/write.
6. **Determinism and safety invariants:** deterministic evidence ordering; the existing SAFE/UNSAFE independence tests remain unchanged and continue passing.

Corpus-level precision/recall/F1 should remain unreported until role-level annotations include both positive and negative examples. The existing two source-audited programs demonstrate concrete recall failures, not population-wide accuracy.

Created 6 todos

Build 2

Updated todo list

Searched for regex `def _detect_population_and_fitness|def _detect_selection|def _detect_mutation|def _detect_crossover|def _detect_replacement|def _detect_termination|def analyze_ea_roles` (`**/analysis/ea_inference/inference.py`), 7 results

Read 

Read 

Read 

Read , lines 104 to 220

The inspection points to a shared gap: role passes keep separate name sets but do not track how an assigned value moves through aliases or call arguments. I’ll add a small AST-backed flow model, use it first for collection aliases and candidate-scoring comprehensions, and leave safety and IR code untouched.

Generating patch (391 lines) in 


```

```


Generating patch (30 lines) in 

Searched for regex `if summary is not None|Call argument is mapped|return _Value\(\)|env\[name\] = value` (`**/analysis/ea_inference/_data_flow.py`), 7 results

Read , lines 280 to 335

Generating patch (19 lines) in 


```

```


Generating patch (68 lines) in 

Created 5 todos


```

```


Generating patch (121 lines) in 


```

```


Generating patch (7 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -v

Checked , , , no problems found

Read 

Generating patch (15 lines) in 


```

```


Starting: *Add unit tests for data-flow model* (3/5)

Generating patch (9 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -v

Generating patch (18 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for path in ['Python/genetic_algorithm/basic_string.py','dataset/deap/ga/onemax.py']:
 result=analyze_ea_roles(Path(path).read_text(),source_name=path)
 print(path,{role.value:(finding.detected,finding.confidence.value,len(finding.evidence)) for role,finding in result.roles.items()})
PY

Generating patch (26 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -v

Checked , , , no problems found

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Generating patch (15 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Checked , , , , no problems found

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check && git -C /home/intel/intel/evosafe status --short -- analysis/ea_inference tests/test_ea_inference.py tests/test_ea_evaluation.py analysis/safety_ir analysis/decision analysis/resource scripts/evaluate_ea_corpus.py evaluation

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('Python/genetic_algorithm/basic_string.py','dataset/deap/ga/onemax.py'):
    report=analyze_ea_roles(Path(file).read_text(encoding='utf-8'), source_name=file)
    print(file)
    print({role.value: (finding.detected, finding.confidence.value) for role, finding in report.roles.items()})
PY

Generating patch (23 lines) in 

Added 1 todo

Read , lines 345 to 400

Generating patch (21 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Checked , , , , no problems found

Generating patch (57 lines) in 


```

```


Generating patch (20 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -q

Checked , , , no problems found

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check && git -C /home/intel/intel/evosafe status --short -- analysis/ea_inference/_data_flow.py analysis/ea_inference/inference.py tests/test_ea_inference.py tests/test_ea_evaluation.py analysis/safety_ir/_model.py analysis/safety_ir/safety_ir_builder.py analysis/decision/engine.py analysis/resource/resource_estimator.py scripts/evaluate_ea_corpus.py

No matches found. Your search pattern might be excluded completely by either the search.exclude settings or .*ignore files.
If you believe that it should have results, you can check into the .*ignore files and the exclude setting (here are some excluded patterns for reference:[**/node_modules,**/bower_components,**/*.code-search]).
Then if you want to include those files you can call the tool again by setting "includeIgnoredFiles" to true.

Generating patch (12 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Checked , , , , no problems found

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check

Searched for regex `class DataFlowFact|class FunctionSummary|class IntraProceduralDataFlow|def _build_summaries|def _analyze_block|def alias_group|def _resolve_summary` (`**/analysis/ea_inference/_data_flow.py`), 7 results

Searched for regex `self.data_flow =|def _detect_population_and_fitness|candidate_comprehension|alias_group\(` (`**/analysis/ea_inference/inference.py`), 5 results

Searched for regex `class TestIntraProceduralDataFlow|test_candidate_scoring_comprehension|test_previously_missed_basic_string|test_alias_of_population_collection` (`**/tests/test_ea_inference.py`), 4 results

## Implemented

Added conservative, AST-backed intra-procedural flow facts. Safety IR, safety verdict logic, resource analysis, and the corpus evaluation utility were not changed.

- The new flow layer records assignment, alias, reassignment, call-site, call-argument, call-result, and return-value facts with source locations. It tracks same-scope aliases at source points and resolves simple local functions that return an argument directly or through a local alias: `_data_flow.py:24-44`, `_data_flow.py:75-119`, `_data_flow.py:202`, `_data_flow.py:419`.
- EA inference now uses those facts to propagate collection aliases and recognize candidate-scoring comprehensions. Evidence retains source locations and the existing confidence meaning: `inference.py:88`, `inference.py:179-273`.
- This recovers **population, fitness evaluation, and termination** in the previously missed generic basic-string GA. The DEAP OneMax example still has no detected roles; its registered operators and their effects remain outside this change’s scope.
- Updated the evaluation-test assertion to derive spot-check counts from current detector results rather than freeze the former all-missed baseline. The evaluation utility itself was not changed.

## Tests and limitations

Added focused tests for assignment and alias flow, function arguments and simple return aliases, unresolved calls, reassignment, call-result flow, and source locations. Detector regressions cover scoring comprehensions, population aliases, and the real basic-string example: `test_ea_inference.py:72`, `test_ea_inference.py:316`.

**All 349 tests passed.**

This is not full CFG-based, path-sensitive, or general interprocedural analysis. Branch and loop joins are conservative; dynamic calls and unresolved call results remain unknown. Helper summaries currently support only simple local return aliases—not helper-mediated mutation, crossover, or framework registrations—so DEAP OneMax remains a known limitation.

Updated todo list

Made changes.

Build 3

Updated todo list

Read 

Read 

Read 

Read , lines 35 to 150

Read , lines 20 to 200

Generating patch (25 lines) in 


```

```


Generating patch (293 lines) in 


```

```


Generating patch (44 lines) in 


```

```


Generating patch (15 lines) in 

Added 3 todos


```

```


Generating patch (19 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -q

Checked , no problems found

Generating patch (55 lines) in 


```

```


Generating patch (144 lines) in 


```

```


Added 1 todo

Read , lines 190 to 390

Generating patch (19 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from analysis.ea_inference import analyze_ea_roles
samples={
'mutation': '''def mutate(individual):\n    individual[0] += 1\n    return individual\npopulation = create()\nfor individual in population:\n    child = mutate(individual)\n''',
'fitness': '''def objective(candidate):\n    return sum(candidate)\ndef evaluate(candidate):\n    return objective(candidate)\npopulation = create()\nfor individual in population:\n    fitness = evaluate(individual)\n''',
'selection': '''def select(population):\n    return sorted(population, key=objective)[:2]\npopulation = create()\nfor individual in population:\n    score = objective(individual)\nselected = select(population)\n''',
'crossover': '''def crossover(parent1, parent2):\n    return parent1[:1] + parent2[1:]\npopulation = create()\nfor parent1, parent2 in pairs(population):\n    child = crossover(parent1, parent2)\n''',
'replacement': '''def replace(population, offspring):\n    population[:] = offspring\n    return population\npopulation = create()\nfor individual in population:\n    score = objective(individual)\nnew_population = replace(population, offspring)\n''',
'ambiguous': '''def maybe(candidate):\n    if flag:\n        return candidate\n    return other\npopulation = create()\nfor individual in population:\n    value = maybe(individual)\n''',
'unresolved': '''def score_one(candidate):\n    return unknown(candidate)\npopulation = create()\nfor individual in population:\n    score = score_one(individual)\n''',
'chain': '''def mutate_one(candidate):\n    candidate[0] += 1\n    return candidate\ndef wrapper(candidate):\n    return mutate_one(candidate)\npopulation = create()\nfor individual in population:\n    child = wrapper(individual)\n''',
'term': '''def should_stop(population):\n    return len(population) == 0\npopulation = create()\nwhile not should_stop(population):\n    for individual in population:\n        score = objective(individual)\n'''
}
for name,src in samples.items():
 try:
  r=analyze_ea_roles(src)
  print(name,{k.value:(v.detected,v.confidence.value,[e.kind for e in v.evidence]) for k,v in r.roles.items()})
 except Exception as ex: print(name,'ERROR',repr(ex))
PY

Generating patch (18 lines) in 


```

```


Generating patch (10 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from analysis.ea_inference import analyze_ea_roles
samples={
'fitness': '''def objective(candidate):\n    return sum(candidate)\ndef evaluate(candidate):\n    return objective(candidate)\npopulation = create()\nfor individual in population:\n    fitness = evaluate(individual)\n''',
'unresolved': '''def score_one(candidate):\n    return unknown(candidate)\npopulation = create()\nfor individual in population:\n    score = score_one(individual)\n''',
'chain': '''def mutate_one(candidate):\n    candidate[0] += 1\n    return candidate\ndef wrapper(candidate):\n    return mutate_one(candidate)\npopulation = create()\nfor individual in population:\n    child = wrapper(individual)\n'''}
for name,src in samples.items():
 r=analyze_ea_roles(src)
 print(name,{k.value:(v.detected,v.confidence.value,[e.kind for e in v.evidence]) for k,v in r.roles.items()})
PY

Generating patch (148 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestHelperFunctionSummaries -v

Checked , , , no problems found

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from analysis.ea_inference import analyze_ea_roles
from textwrap import dedent
s='''
            def objective(candidate):
                return sum(candidate)
            def evaluate(candidate):
                return objective(candidate)
            population = create()
            for individual in population:
                fitness = evaluate(individual)
        '''
r=analyze_ea_roles(dedent(s))
for x in r.for_role('fitness_evaluation').evidence: print(x.kind,x.line,x.snippet,x.description)
PY

Generating patch (10 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestHelperFunctionSummaries -q

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
 report=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
 print(file)
 for role,finding in report.roles.items():
  print(role.value,finding.detected,finding.confidence.value,[(e.kind,e.line) for e in finding.evidence])
PY

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from analysis.ea_inference import analyze_ea_roles
s='''def increment(value):\n    return value + 1\nvalues = [1, 2, 3]\nfor value in values:\n    answer = increment(value)\n'''
r=analyze_ea_roles(s)
print({k.value:(v.detected,v.confidence.value,[e.kind for e in v.evidence]) for k,v in r.roles.items()})
PY

Generating patch (31 lines) in 


```

```


Added 2 todos

Generating patch (41 lines) in 


```

```


Generating patch (26 lines) in 


```

```


Generating patch (18 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestHelperFunctionSummaries -v

Checked , , , no problems found

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
 report=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
 print(file)
 for role,finding in report.roles.items():
  print(role.value,finding.detected,finding.confidence.value,[(e.kind,e.line) for e in finding.evidence])
PY

Generating patch (13 lines) in 


```

```


Generating patch (6 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -q

Checked , , , no problems found

Generating patch (21 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestHelperFunctionSummaries -q

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
    report=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
    print(file, {role.value: (finding.detected, finding.confidence.value) for role,finding in report.roles.items()})
PY

Checked , , , no problems found

Updated todo list

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check && git -C /home/intel/intel/evosafe status --short -- analysis/ea_inference/_data_flow.py analysis/ea_inference/inference.py tests/test_ea_inference.py analysis/safety_ir/_model.py analysis/safety_ir/safety_ir_builder.py analysis/decision/engine.py analysis/resource/resource_estimator.py

Searched for regex `class FunctionRoleEvidence|class FunctionSummary|def _build_summaries|def _summarize_function|def summary_for_call` (`**/analysis/ea_inference/_data_flow.py`), 5 results

Searched for regex `def _apply_helper_role_summaries|def _detect_helper_termination|def _detect_population_and_fitness|self._apply_helper_role_summaries` (`**/analysis/ea_inference/inference.py`), 4 results

Searched for regex `class TestHelperFunctionSummaries|def test_.*helper|def test_ambiguous|def test_unresolved|def test_ordinary_helper` (`**/tests/test_ea_inference.py`), 13 results

Read , lines 205 to 285

Implemented bounded, same-source function summaries without changing Safety IR, safety verdict logic, or resource analysis.

### Changes

- Added helper summaries for positional argument-to-return flow, argument mutation, candidate-dependent returned calls, ranking/filtering, two-input recombination, collection replacement, and returned conditions. A bounded fixed-point pass supports straightforward helper chains in `_data_flow.py:50-64` and `_data_flow.py:171-187`.
- Integrated summaries with the existing role detectors. Findings require relevant call arguments to be structurally linked to inferred candidates or populations; evidence includes both the helper operation and its call site ([inference integration](analysis/ea_inference/inference.py#L364-L439)).
- Ambiguous returns, dynamic behavior, and unresolved calls are not promoted to confident findings. Uncertain summary evidence remains LOW and undetected.
- Added helper positives and ordinary/ambiguous/unresolved negatives in `test_ea_inference.py:418-568`.

### Results

Helper-based tests now recognize **Mutation, Fitness Evaluation, Selection, Crossover, Replacement, and Termination**. Population inference still relies on existing visible collection-iteration/comprehension evidence; collection creation hidden entirely in a helper is not summarized as a population by itself.

On the two audited programs:

- **DEAP OneMax:** still no roles detected. Toolbox registration, delegated operators, and their value relationships remain unresolved.
- **basic_string.py:** Population, Fitness Evaluation, and Termination are detected; Selection, Mutation, Crossover, and Replacement remain undetected. Fitness evidence now also points to its helper implementation. No additional role category became detectable for this file.

No precision or recall is claimed. The full test suite passed: **360 tests**.

Remaining limits include bounded summary depth, simple positional-call resolution, and no general heap/alias or framework-registration analysis. Safety IR and verdict semantics were not modified in this task.

Made changes.

Build 4

Updated todo list

Read 

Read 

Read , lines 400 to 571

Read , lines 25 to 161

Read , lines 20 to 209

Generating patch (42 lines) in 


```

```


Generating patch (177 lines) in 


```

```


Generating patch (15 lines) in 


```

```


Created 5 todos

Generating patch (15 lines) in 


```

```


Generating patch (49 lines) in 


```

```


Generating patch (28 lines) in 


```

```


Generating patch (173 lines) in 


```

```


Generating patch (16 lines) in 


```

```


Starting: *Apply to the seven roles* (4/5)

Generating patch (22 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
 r=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
 print(file,{k.value:(v.detected,v.confidence.value,[(e.kind,e.line) for e in v.evidence]) for k,v in r.roles.items()})
PY

Checked , , no problems found

Generating patch (15 lines) in 


```

```


Generating patch (124 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestIntraProceduralDataFlow tests.test_ea_inference.TestRegistrationDispatch -v

Checked , , , no problems found

Generating patch (9 lines) in 


```

```


Generating patch (38 lines) in 


```

```


Starting: *Optional framework summaries* (1/3)

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestRegistrationDispatch -v

Generating patch (43 lines) in 


```

```


Generating patch (25 lines) in 


```

```


Generating patch (21 lines) in 


```

```


Generating patch (32 lines) in 

Searched for regex `test_slice_replacement_retains|test_copy_then_candidate_element` (`**/tests/test_ea_inference.py`), 2 results

Generating patch (25 lines) in 


```

```


Generating patch (17 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference -q

Checked , , , no problems found

Generating patch (29 lines) in 


```

```


Generating patch (25 lines) in 

Added 3 todos

Read , lines 188 to 240

Read , lines 240 to 290

Generating patch (31 lines) in 


```

```


Generating patch (6 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest tests.test_ea_inference.TestMutationAndCrossover.test_candidate_alias_element_write_is_mutation -v

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
    result=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
    print(file)
    for role, finding in result.roles.items():
        evidence=[(item.kind,item.line,item.snippet) for item in finding.evidence]
        print(role.value, finding.detected, finding.confidence.value, evidence)
PY

Checked , , , no problems found

Generating patch (22 lines) in 


```

```


Generating patch (12 lines) in 


```

```


Generating patch (11 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check

Checked , , , no problems found

Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python - <<'PY'
from pathlib import Path
from analysis.ea_inference import analyze_ea_roles
for file in ('dataset/deap/ga/onemax.py','Python/genetic_algorithm/basic_string.py'):
    report=analyze_ea_roles(Path(file).read_text(encoding='utf-8'),source_name=file)
    print(file)
    for role,finding in report.roles.items():
        print(role.value, finding.detected, finding.confidence.value, [(ev.kind,ev.line) for ev in finding.evidence])
PY

Searched for regex `class RegistrationFact|class DispatchFact|def _collect_registrations_and_dispatches|def dispatch_for_call|def roots_for_name` (`**/analysis/ea_inference/_data_flow.py`), 5 results

Searched for regex `_REGISTERED_ROLE_TAGS|def _expand_population_aliases|def _apply_registered_dispatches|def _detect_slice_replacement|def run` (`**/analysis/ea_inference/inference.py`), 6 results

Searched for regex `class TestRegistrationDispatch|def test_chained_alias_flow|def test_slice_replacement|def test_candidate_alias_element_write` (`**/tests/test_ea_inference.py`), 4 results

Generating patch (19 lines) in 


```

```


Ran terminal command:  PYTHONDONTWRITEBYTECODE=1 /tmp/evosafe-venv/bin/python -m unittest discover -s tests -q

Ran terminal command:  git -C /home/intel/intel/evosafe diff --check

Checked , , , no problems found

Implemented generic alias and registration/dispatch resolution. No framework-specific APIs, Safety IR, safety verdict logic, or resource analysis were changed.

### What changed

- The flow layer now records object aliases, container mutations, slice replacements, and paired registration/dispatch facts, including ambiguous registrations and callback resolution status in `_data_flow.py:73-86` and `_data_flow.py:848-942`.
- EA inference propagates collection aliases, recognizes generic `register(key, callback)` relationships and later attribute or keyed dispatches, and uses callback summaries when available in `inference.py:484-665`.
- Role labels are generic EA taxonomy tags, not framework API names. Ambiguous registrations and unresolved bare callback names are not promoted to detections.

### Audited programs

**DEAP OneMax:** all seven roles are now detected with evidence. Population, fitness evaluation, selection, mutation, crossover, and termination are MEDIUM confidence; replacement is HIGH.

- Population: registration and factory dispatch at `onemax.py:45-77`.
- Fitness evaluation: callback `sum(individual)`, registration, and `map` dispatch at `onemax.py:48-55` and `onemax.py:88`.
- Selection: registration and invocation at `onemax.py:68-107`.
- Mutation: registration and invocation at `onemax.py:62-127`.
- Crossover: registration and two-parent invocation at `onemax.py:58-116`.
- Replacement: slice assignment at `onemax.py:139`.
- Termination: bounded convergence condition at `onemax.py:101`.

**basic_string.py:** Population, Fitness Evaluation, and Termination remain detected. Selection, Mutation, Crossover, and Replacement remain missed. This generic program uses helper calls and data flow without registration dispatch, so the registration change added no new role categories there.

### Tests, risks, and limitations

Added coverage for chained aliases, aliased object mutation, slice replacement, generic registration and dispatch, unresolved and ambiguous registrations, non-EA registrations, and both audited programs in `test_ea_inference.py:351-360` and `test_ea_inference.py:613-690`.

A registration key explicitly labeled `mutation`, `select`, or another EA role could be misleading when its callback is external and its body is unavailable; those cases receive only MEDIUM confidence. Local callbacks with contradictory or unresolved evidence are not treated as confirmed roles. No precision or recall is claimed.

**All 371 tests passed.**

Added 7 todos

Made changes.
