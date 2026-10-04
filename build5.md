Build 5A — Re-evaluate the full corpus after EA inference improvements

The EA inference implementation has now gone through:
- intra-procedural data-flow improvements
- helper-function summaries
- alias/lineage tracking
- generic registration/dispatch resolution
- slice replacement handling
- improved callback relationships

Current status:
- 371 tests passing.
- DEAP OneMax now detects all 7 EA roles.
- basic_string.py detects Population, Fitness Evaluation, and Termination, but still misses Selection, Mutation, Crossover, and Replacement.
- Safety IR has NOT been changed.
- Safety verdict logic has NOT been changed.
- Resource analysis has NOT been changed.
- No precision/recall claims exist yet.

IMPORTANT:
Do NOT modify detector code during this task.
Do NOT modify Safety IR.
Do NOT modify safety decision logic.
Do NOT implement resource-bound enrichment.

TASK:

1. Re-run the existing EA corpus evaluation using the current detector implementation.

2. Use the same corpus as the previous evaluation so the results are comparable.

3. Preserve the previous evaluation methodology.

For every successfully analyzed program report:
- program name
- safety verdict
- population detected/confidence/evidence
- fitness detected/confidence/evidence
- selection detected/confidence/evidence
- mutation detected/confidence/evidence
- crossover detected/confidence/evidence
- replacement detected/confidence/evidence
- termination detected/confidence/evidence

4. Compare old vs new results.

Produce a comparison table:

role
old detections
new detections
delta
old confidence distribution
new confidence distribution

5. Specifically compare:
- DEAP
- PyGAD
- pymoo
- mealpy
- generic EA examples
- generic educational Python
- safety-oriented corpus

6. Re-audit:
- DEAP OneMax
- basic_string.py
- the previously inspected PyGAD example
- the previously inspected pymoo example
- the previously inspected mealpy example

For each, manually inspect whether the reported evidence actually corresponds to the claimed EA role.

This is important:
A higher detection count is NOT automatically an improvement.
Look for false-positive patterns introduced by the new shared analysis.

7. Check for regressions:
- ordinary Python loops incorrectly classified as EA roles
- model-weight loops classified as population
- arbitrary indexed writes classified as mutation
- random indexing classified as selection
- generic callback registration classified as EA
- ordinary collection replacement classified as EA replacement

8. Re-run all existing tests.

9. Produce:
- updated JSON
- updated CSV
- updated human-readable summary
- old-vs-new comparison report

10. Report:
A. total programs
B. successful analyses
C. parse failures
D. role detection counts before/after
E. confidence distributions
F. reviewed false-positive candidates
G. reviewed remaining false negatives
H. safety verdict distribution before/after
I. confirmation that EA inference still does not alter safety verdicts

Do NOT calculate precision, recall, or F1 for the whole corpus unless ground-truth role labels exist.

Do NOT create a new benchmark yet.

At the end, recommend whether the next step should be:
1. fix false positives,
2. improve basic_string.py helper/data-flow handling,
3. build a labeled benchmark,
or
4. proceed to resource-bound analysis.

Base that recommendation on the new evaluation evidence.

Build 5B

Build 5B — Create a small labeled EA-role benchmark

Do NOT modify detector implementation in this task.

Based on the latest corpus re-evaluation and the existing audited programs, create a small controlled benchmark for the seven EA roles.

The benchmark must contain ground-truth labels for:

Population
Fitness Evaluation
Selection
Mutation
Crossover
Replacement
Termination

Create examples for:

1. Direct EA implementation
2. Helper-function EA
3. Alias-based EA
4. Generic registration/dispatch
5. DEAP-style operator registration
6. Comprehension-based fitness evaluation
7. Two-parent crossover through helper functions
8. Mutation through helper functions
9. Population slice replacement
10. Termination through helper-returned conditions
11. Non-EA loops that resemble EA patterns
12. Non-EA indexed writes
13. Non-EA random indexing
14. Ordinary callback registration
15. Safe EA
16. Unsafe EA
17. Incomplete/ambiguous EA

Keep the benchmark small and readable.

Every program must have explicit role-level ground truth.

For every example, document why each positive role is a role and why each negative role is not.

Integrate the benchmark with the existing evaluation runner.

Calculate per-role:
- TP
- FP
- FN
- TN
- precision
- recall
- F1

Also calculate macro-average metrics.

Do not mix these benchmark metrics with the unlabeled corpus results.

Keep safety evaluation separate from EA-role evaluation.

Verify that EA inference does not change:
SAFE
UNSAFE
UNKNOWN

Run the complete 371+ test suite.

Do not modify:
- Safety IR
- safety decision logic
- resource analysis
- resource-bound enrichment

Report the benchmark and its results clearly.

Build 5C

Build 5C — Candidate lineage
population
   ↓
parent candidates
   ↓
selection
   ↓
parent1 + parent2
   ↓
crossover
   ↓
offspring
   ↓
mutation
   ↓
replacement
   ↓
new population

That lineage is also extremely useful for your later resource-bound work.
For example, once EvoSafe can establish:
population size
×
number of generations
×
fitness evaluations per generation

you can start deriving resource bounds from EA structure.

