“Do only the repository inspection and proposed design. Do not modify files yet.”

I am extending an existing Python static-analysis project called EvoSafe.

IMPORTANT:
- Do NOT rewrite or replace the existing safety-analysis pipeline.
- Do NOT remove or weaken existing taint, capability, resource, AST/CFG, verdict, or certificate functionality.
- First inspect the repository and understand the existing architecture, modules, IR/data structures, analysis passes, tests, and naming conventions.
- Reuse existing AST/CFG/program-graph infrastructure wherever possible.
- Follow the existing coding style and architecture.
- Make small, reviewable changes.
- Do not introduce ML/LLM-based inference at this stage.
- The EA inference layer must be structural, explainable, deterministic, and framework-independent.

PROJECT GOAL

Add a framework-independent EA (Evolutionary Algorithm) inference layer that detects seven possible EA roles from Python program structure and data/control-flow evidence.

The seven roles are:

1. Population
2. Fitness Evaluation
3. Selection
4. Mutation
5. Crossover
6. Replacement
7. Termination

IMPORTANT ARCHITECTURAL RULE

EA inference is OPTIONAL enrichment.

Safety analysis must remain independent of EA recognition.

The system must NOT behave like:

    EA detected -> SAFE

Instead:

    Python program
        |
        +--> existing safety analysis
        |       +--> taint
        |       +--> capability
        |       +--> resource
        |
        +--> optional EA-role inference
                +--> population
                +--> fitness
                +--> selection
                +--> mutation
                +--> crossover
                +--> replacement
                +--> termination

EA inference may later provide information for resource-bound enrichment, but EA recognition must never override a safety violation.

REQUIREMENTS

For each of the seven roles, implement a detector that:

1. Uses Python AST and existing CFG/program-graph information.
2. Does not depend on DEAP, PyGAD, pymoo, mealpy, or any specific EA framework API.
3. Detects structural patterns rather than only matching library names.
4. Produces evidence explaining why a role was detected.
5. Produces a confidence value or confidence category consistent with the existing project architecture.
6. Handles uncertainty conservatively.
7. Does not claim a role solely from a variable name if stronger structural evidence is unavailable.
8. Records source locations / AST nodes / CFG information wherever the existing infrastructure supports this.
9. Is independently testable.
10. Does not change the existing SAFE / UNKNOWN / UNSAFE semantics.

ROLE DEFINITIONS

### 1. Population

Detect structures representing a collection of candidate solutions.

Look for structural evidence such as:
- collection/list/set/dict construction or assignment
- repeated iteration over candidate objects
- collection being passed to evolutionary operators
- collection being updated between iterations
- population-size related operations
- candidate creation followed by collection insertion

Do NOT rely only on names such as `population`, `pop`, or `individuals`.

Example:

    population = initialize_candidates()
    for individual in population:
        evaluate(individual)

should provide strong population evidence.

---

### 2. Fitness Evaluation

Detect code that evaluates or scores an individual/candidate.

Look for patterns such as:
- function calls that consume an individual/candidate
- computation producing a score/objective/value
- repeated evaluation inside a population/generation loop
- comparison/ranking based on computed scores
- fitness/objective values associated with candidates

Do NOT depend only on names such as `fitness`, `score`, or `evaluate`.

Example:

    score = objective(individual)

or

    fitness_value = loss(candidate)

should be considered candidate evidence.

---

### 3. Selection

Detect code that chooses candidates from a population for further processing.

Look for:
- sorting/ranking candidates
- top-k/best selection
- filtering candidates
- sampling candidates
- choosing candidates based on score/fitness
- passing selected candidates to mutation/crossover/reproduction

Example:

    selected = sorted(population, key=fitness)[-10:]

should provide selection evidence.

Do not require a function named `select`.

---

### 4. Mutation

Detect code that modifies an existing candidate to create a changed candidate.

Look for:
- modification of candidate values
- random perturbation
- element replacement/update
- transformation of an individual
- candidate -> modified candidate relationship
- mutation-like operations occurring inside an evolutionary loop

Example:

    child = individual.copy()
    child[i] += random_change

should provide mutation evidence.

Do not rely only on a function named `mutate`.

---

### 5. Crossover

Detect combination of information from two or more candidate solutions to produce offspring.

Look for:
- two or more candidate inputs
- slicing/combining candidate representations
- concatenation or recombination
- constructing a child from multiple parents
- pairwise candidate operations
- crossover-like behavior inside an evolutionary loop

Example:

    child = parent1[:k] + parent2[k:]

should provide crossover evidence.

Do not require a function named `crossover`.

---

### 6. Replacement

Detect population update/replacement after candidate evaluation or reproduction.

Look for:
- adding offspring to population
- removing old candidates
- replacing worst/best candidates
- assigning a new population
- population update after selection/mutation/crossover
- survivor selection

Example:

    population = selected + offspring

or:

    population[worst_index] = child

should provide replacement evidence.

---

### 7. Termination

Detect the condition controlling the end of an evolutionary process.

Look for:
- bounded generation loops
- iteration limits
- convergence checks
- fitness thresholds
- no-improvement conditions
- population/process stopping conditions
- explicit termination conditions

Example:

    for generation in range(100):
        ...

or:

    while best_score < threshold:
        ...

The detector must distinguish:
- statically bounded termination
- condition-based termination
- unknown/unbounded termination

This distinction is important for later resource-bound analysis.

---

EVIDENCE MODEL

Before implementing, inspect the existing project to determine how analysis evidence is currently represented.

Reuse the existing evidence/provenance representation if one exists.

Each EA-role finding should conceptually contain:

    role
    confidence
    evidence
    source location
    supporting AST/CFG/data-flow information
    optional reason

Example conceptual result:

    Role: FITNESS_EVALUATION
    Confidence: HIGH
    Evidence:
        objective(individual)
    Location:
        line 24
    Reason:
        Candidate-dependent value is computed inside population iteration.

Do not invent a completely separate evidence architecture if the existing project already provides one.

CONFIDENCE

Use the existing project's confidence representation if available.

If it does not exist, introduce a minimal and explainable mechanism such as:

    HIGH
    MEDIUM
    LOW

or a bounded numeric confidence.

Do NOT pretend that confidence is statistically calibrated.

It represents strength of structural evidence, not probability of correctness.

FRAMEWORK INDEPENDENCE

The following must NOT be required for detection:

    DEAP
    PyGAD
    pymoo
    mealpy
    specific EA class names
    specific framework decorators
    framework imports

Framework-specific examples can be used in tests, but detectors must rely on program structure.

FALSE POSITIVES

Pay particular attention to false positives.

For example:

    population = [1, 2, 3]

alone does NOT prove that this is an EA population.

Similarly:

    score = x * 2

alone does NOT prove fitness evaluation.

A role should receive stronger confidence when multiple structural clues agree.

UNKNOWN / INSUFFICIENT EVIDENCE

If evidence is insufficient, do not force a role.

Represent the role as:
- not detected, or
- unknown/low-confidence

according to the existing project conventions.

Do not classify an entire program as unsafe merely because EA inference cannot identify an EA role.

TESTING

Before modifying code:

1. Inspect existing tests.
2. Identify the appropriate test structure.
3. Add unit tests for each detector.
4. Add positive and negative examples.
5. Add framework-independent examples.
6. Add examples where variable names are misleading.
7. Add examples where only partial EA structure is present.
8. Verify that all existing safety tests still pass.

Create tests approximately covering:

Population:
- clear population collection
- ordinary unrelated collection
- population-like collection with no EA behavior

Fitness:
- candidate scoring
- ordinary arithmetic
- score independent of candidate

Selection:
- ranking/filtering candidates
- ordinary list filtering

Mutation:
- candidate modification
- ordinary variable modification

Crossover:
- two-parent recombination
- ordinary string/list concatenation

Replacement:
- offspring inserted/replacing population members
- ordinary collection update

Termination:
- bounded generation loop
- convergence-based loop
- unbounded loop
- unrelated loop

INTEGRATION

Integrate the EA inference layer into the existing pipeline without changing the existing safety verdict.

The conceptual output should become:

    AnalysisResult
        safety_facts
        resource_facts
        ea_roles
        evidence

If EvoIR already exists, extend it rather than creating another competing representation.

If EvoIR does not yet exist in the current implementation, first report the current IR structure and propose the smallest compatible extension.

IMPORTANT:

Do NOT implement resource enrichment yet unless the repository architecture already requires it for integration.

First make the seven EA detectors work and test them independently.

DELIVERABLES

Before making major changes, report:

1. Existing repository architecture relevant to this task.
2. Existing AST/CFG infrastructure.
3. Existing analysis result/evidence structures.
4. Existing IR/SafetyIR/EvoIR structures.
5. Existing test structure.
6. Proposed files/modules to modify.
7. Proposed design for the seven detectors.

Then implement the detectors incrementally.

After implementation, report:

1. Files changed.
2. What each detector does.
3. Evidence representation.
4. Confidence representation.
5. Tests added.
6. Test results.
7. Any limitations or unresolved cases.
8. Suggested next step for resource-bound enrichment.

Do not silently make architectural changes outside this scope.
