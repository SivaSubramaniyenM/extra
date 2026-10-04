The goal is:

**Current detectors → data-flow support → helper-function support → alias/indirect-call support → targeted benchmark → re-evaluation**

---

# Build 1 — Failure analysis + architecture inspection

Give GHCP this first.

```text
We have completed the first evaluation of the existing EvoSafe EA-role inference system.

Current baseline:
- 338 tests passing.
- 2,562 Python programs discovered.
- 2,545 analyzed successfully.
- 17 failed parsing because of legacy Python 2 syntax.
- Seven EA roles:
  1. Population
  2. Fitness Evaluation
  3. Selection
  4. Mutation
  5. Crossover
  6. Replacement
  7. Termination
- Safety IR version 1.1 is already integrated.
- EA inference is optional.
- SAFE/UNKNOWN/UNSAFE safety verdicts are independent of EA inference.
- Do not modify the Safety IR architecture.
- Do not implement resource-bound enrichment yet.

Important evaluation finding:
Two source-audited positive EA programs:
1. DEAP OneMax
2. generic basic_string.py

Both explicitly implement all seven EA roles, but the current detectors detected none of the seven roles.

Likely weaknesses include:
- helper functions
- toolbox/operator registration
- indirect calls
- variable aliases
- population replacement through slice assignment
- interprocedural relationships
- loop conditions whose meaning is not directly connected to EA variables

TASK:

Before changing any detector code, inspect the existing implementation and perform a detailed failure analysis.

1. Locate:
   - all seven detector implementations
   - shared AST/CFG utilities
   - existing data-flow utilities
   - call graph/interprocedural utilities, if any
   - evidence generation
   - confidence calculation
   - existing tests

2. Inspect:
   - DEAP OneMax
   - basic_string.py
   - at least 3 additional framework-associated examples from the evaluation corpus where multiple EA roles were detected or suspected.

3. For each missed role in DEAP OneMax and basic_string.py:
   - identify the exact source construct implementing the role
   - explain why the current detector misses it
   - classify the weakness as:
       a. helper function
       b. aliasing
       c. indirect call
       d. framework registration
       e. interprocedural data flow
       f. CFG limitation
       g. detector rule limitation
       h. genuinely ambiguous
   - do not change code yet.

4. Determine what reusable static-analysis capability is missing.
   Prefer shared infrastructure over adding ad-hoc rules to each detector.

5. Propose a minimal architecture for improving:
   - intra-procedural data flow
   - helper-function summaries
   - simple alias tracking
   - indirect-call resolution

6. Rank these improvements by:
   - expected impact on the seven EA roles
   - implementation complexity
   - risk of false positives
   - usefulness for future resource-bound analysis

IMPORTANT:
- Do not modify detector logic yet.
- Do not modify Safety IR.
- Do not add framework-specific logic yet.
- Do not implement resource enrichment.
- Do not invent ground-truth metrics.
- Preserve all existing tests.

Deliver:
A. failure-analysis report
B. proposed shared analysis architecture
C. prioritized implementation plan
D. exact files that would need modification
E. new tests that should be added
```

---

# Build 2 — Add shared data-flow infrastructure

Once GHCP gives you the analysis, use this.

```text
Implement the highest-priority shared static-analysis improvement identified in the previous failure analysis.

Goal:
Improve EA-role inference for realistic Python programs without rewriting the seven detectors as ad-hoc pattern matchers.

Baseline:
- 338+ tests currently pass.
- Safety IR 1.1 must remain unchanged.
- Safety verdict logic must remain unchanged.
- EA inference remains optional.
- Resource-bound enrichment is NOT part of this task.

Focus ONLY on:
INTRA-PROCEDURAL DATA FLOW.

Implement a lightweight, conservative data-flow layer that can track relationships such as:

variable
  -> assignment
  -> reassignment
  -> use
  -> function argument
  -> return value
  -> call site

The analysis should operate on the existing AST/CFG infrastructure wherever possible.

Requirements:

1. Inspect the existing code before implementation.

2. Reuse existing AST/CFG structures rather than creating a competing representation.

3. Track source locations for data-flow facts.

4. Support simple cases such as:

   population = create_population()
   pop = population
   offspring = mutate(pop)

5. Track:
   - assignments
   - simple aliases
   - function arguments
   - return values where statically resolvable
   - call-site relationships

6. Be conservative:
   - unresolved relationships must remain unknown
   - never infer a role solely because a variable has a name such as "population", "fitness", "mutant", etc.
   - do not claim certainty when the relationship cannot be established.

7. Do not make the analysis framework-specific.

8. Integrate this capability only where it clearly improves the existing detectors.

9. Preserve the existing evidence model:
   every new detection should include useful source evidence.

10. Preserve confidence semantics:
   HIGH/MEDIUM/LOW represent structural evidence strength, not probability.

Tests:
Add focused unit tests for:
- simple assignment flow
- alias flow
- function argument flow
- return-value flow
- unresolved call
- reassignment
- source-location preservation

Then add detector-level tests demonstrating that at least some previously missed EA patterns can now be recognized.

Run:
- new tests
- complete test suite

Do not modify:
- Safety IR schema
- safety decision logic
- resource analysis
- resource estimator
- corpus evaluation utility

Report:
- files changed
- new analysis capability
- detectors benefiting from it
- tests added
- full test count
- remaining limitations
```

---

# Build 3 — Helper functions + interprocedural summaries

After Build 2 passes, use this.

```text
Now improve EA-role inference using the shared data-flow infrastructure implemented in the previous build.

Goal:
Recover EA roles that are implemented through small helper functions.

Do NOT redesign the system.

Do NOT modify Safety IR.
Do NOT modify safety verdict semantics.
Do NOT implement resource-bound enrichment.
Do NOT add framework-specific rules yet.

Problem examples:

def mutate(individual):
    ...
    return individual

child = mutate(parent)

or:

def evaluate(candidate):
    return objective(candidate)

fitness = evaluate(individual)

or:

def select(population):
    ...
    return survivors

selected = select(population)

TASK:

1. Inspect the current data-flow implementation.

2. Add conservative interprocedural function summaries for simple Python functions.

A summary may capture relationships such as:

INPUT argument
    -> transformed
    -> RETURN VALUE

and relevant role evidence.

3. Support:
   - positional arguments
   - simple return values
   - direct function calls
   - functions defined in the same analyzed source/module
   - straightforward helper chains where safely resolvable

4. Do NOT attempt full Python interprocedural analysis.

5. If a function:
   - contains dynamic behavior
   - uses unresolved calls
   - has complex aliasing
   - has multiple ambiguous return paths

then preserve UNKNOWN/LOW-confidence behavior rather than guessing.

6. Use the summaries to improve the existing seven EA detectors.

Examples:

Mutation:
argument candidate -> modified candidate -> returned candidate

Fitness:
candidate argument -> candidate-dependent computation -> returned score

Selection:
population argument -> subset/ranking/filtering -> returned candidates

Crossover:
two candidate inputs -> combined output

Replacement:
population input -> updated population output

Termination:
helper-returned condition may be used only when the relationship is statically justified.

7. Evidence must point back to actual source locations.

8. Do not use variable names alone as evidence.

9. Do not introduce framework-specific assumptions.

Testing:

Create small positive tests for each applicable role using helper functions.

Also create negative tests where a helper function performs ordinary non-EA computation and must NOT be classified as an EA role.

Add tests for:
- direct helper
- helper returning a candidate
- helper returning a score
- helper with ambiguous return
- unresolved helper
- helper chain

Run the complete test suite.

Compare against the two previously audited programs:
- DEAP OneMax
- basic_string.py

Report which roles become detectable and which remain missed.

Do not claim recall/precision unless ground-truth labels exist.
```

---

# Build 4 — Alias + indirect-call/framework registration

This is the **important one for DEAP**.

```text
Continue improving EA-role inference after the helper-function/interprocedural build.

Current problem:
DEAP OneMax and similar framework-style programs often register operators indirectly:

toolbox.register("evaluate", ...)
toolbox.register("select", ...)
toolbox.register("mate", ...)
toolbox.register("mutate", ...)

Later the program invokes the registered operations indirectly.

Goal:
Conservatively resolve simple indirect relationships so the seven EA roles can be recognized.

IMPORTANT:
Do NOT make the detector dependent on DEAP.

Instead, implement a generic mechanism for resolving simple registration/dispatch patterns.

TASK 1 — Simple alias tracking

Support straightforward relationships such as:

a = population
b = a
c = b

and:

population[:] = offspring

when the relationship can be established statically.

Track:
- assignment aliases
- simple container/object aliases
- slice assignment
- simple mutation of aliased objects

Do not attempt full Python points-to analysis.

TASK 2 — Simple registration/dispatch abstraction

Create a generic representation for patterns like:

register("role", function)

followed later by:

invoke_registered("role", ...)

The representation should not hard-code DEAP names.

If the existing source pattern allows a reliable relationship between registration and later invocation, preserve it as evidence.

TASK 3 — Optional framework summaries

Only if necessary, introduce a lightweight framework-summary mechanism.

Framework summaries may describe:
- what an API call means structurally
- what arguments/returns it connects
- what EA role it may represent

Do NOT embed large DEAP/PyGAD/pymoo/mealpy-specific detectors.

The generic static-analysis path must continue to work without framework summaries.

TASK 4 — Apply to the seven roles

Improve:
- Population
- Fitness Evaluation
- Selection
- Mutation
- Crossover
- Replacement
- Termination

Use structural evidence and data-flow relationships.

Variable names alone must never establish a role.

TASK 5 — Tests

Add tests for:
- simple aliases
- chained aliases
- slice replacement
- registered function
- indirect function invocation
- unresolved registration
- ambiguous registration
- non-EA registration

Regression-test:
- DEAP OneMax
- basic_string.py

Run the complete test suite.

Report:
1. roles newly detected
2. evidence for each detection
3. remaining missed roles
4. false-positive risks
5. whether framework summaries were required
6. full test count

Do not modify:
- Safety IR schema
- safety decision logic
- resource-bound analysis
- resource enrichment
```

---

# Then Build 5 — Targeted benchmark

Once those improvements are stable, **don't immediately jump into resource bounds**.

Give GHCP this:

```text
The EA-role inference improvements are now implemented.

Next, build a small targeted labeled benchmark specifically for evaluating the seven EA-role detectors.

Do not modify the detector implementation during this task.

Goal:
Create a controlled benchmark that isolates the failure modes found during the corpus evaluation.

Benchmark categories:

1. Direct EA implementation
2. Helper-function EA
3. Alias-heavy EA
4. Indirect-call EA
5. DEAP-style registration
6. PyGAD-style framework usage, if representative examples are available
7. pymoo-style framework abstraction, if representative examples are available
8. mealpy-style framework abstraction, if representative examples are available
9. Non-EA programs that resemble EA patterns
10. Safe EA
11. Unsafe EA
12. EA with incomplete/ambiguous role implementation

For each benchmark program define ground-truth labels for all seven roles:

population
fitness_evaluation
selection
mutation
crossover
replacement
termination

Labels must be based on the actual source implementation.

Do not invent labels for external corpus programs.

Produce:
- benchmark source files
- ground-truth JSON
- evaluation runner integration
- per-role confusion matrix
- precision
- recall
- F1
- support/count

Also report:
- UNKNOWN/ambiguous cases
- confidence distribution
- evidence quality

Keep safety evaluation separate from EA-role evaluation.

Verify:
SAFE EA
UNSAFE EA
SAFE non-EA
UNSAFE non-EA
incomplete EA

Confirm EA recognition does not modify the safety verdict.

Run the full existing test suite plus benchmark tests.

Do not modify:
- Safety IR architecture
- safety decision logic
- resource-bound analysis
```

---

# Finally — Build 6: Resource-bound analysis

**Only after the above is working reasonably well.**

Then we move to your second research contribution:

> **Can EA structure improve resource bounds without weakening safety?**

The next prompt should compare:

```text
Generic static bound
        vs
EA-aware bound
```

For example:

```python
for generation in range(10):
    for individual in population:
        evaluate(individual)
```

Generic analysis may see two loops.

EA-aware analysis can potentially derive:

```text
evaluation_count ≤ generations × population_size
```

and if both are statically bounded:

```text
evaluation_count ≤ 10 × |population|
```

That is where your **EA → resource enrichment** becomes scientifically meaningful.

---

## So your build order should be

```text
BUILD 1
Failure analysis
       ↓
BUILD 2
Data-flow infrastructure
       ↓
BUILD 3
Helper/interprocedural analysis
       ↓
BUILD 4
Aliases + indirect calls + registration
       ↓
BUILD 5
Small labeled benchmark
       ↓
BUILD 6
Resource-bound analysis
       ↓
BUILD 7
EA-aware resource enrichment
       ↓
BUILD 8
End-to-end evaluation + VolPE/EvoLab integration
```

**Do not give GHCP all 8 prompts at once.** Run them one at a time, run the full test suite after each, and keep **338 tests as your regression baseline**. This will also give you a much cleaner story for your paper: *evaluation exposed limitations → shared static-analysis improvements → controlled benchmark → resource-bound enrichment*.
