We have now implemented and tested the initial seven framework-independent EA-role detectors in the existing EvoSafe repository.

The next task is to integrate their results into the project's intermediate representation.

IMPORTANT:
- Do NOT redesign the existing safety-analysis architecture.
- Do NOT remove or modify the semantics of the existing safety verdict.
- Do NOT make EA inference a prerequisite for safety certification.
- Do NOT implement resource-bound enrichment yet.
- Do NOT add new datasets yet.
- First inspect the current implementation and determine the smallest compatible change.

GOAL

Extend the existing analysis representation so that one analyzed Python program can contain:

1. Safety analysis results
   - taint
   - capability
   - resource
   - existing safety findings

2. EA inference results
   - population
   - fitness evaluation
   - selection
   - mutation
   - crossover
   - replacement
   - termination

3. Evidence
   - source location
   - AST/CFG/program-graph evidence where available
   - explanation/reason for the finding

4. Confidence
   - confidence associated with EA-role inference
   - clearly distinguish structural confidence from statistical probability

The representation should eventually support the EvoSafe research goal:

    Python Program
          |
       AST → CFG
          |
    Static Analysis
          |
    +-----+------+
    |            |
 Safety       EA Inference
 Facts          Facts
    |            |
    +-----+------+
          |
        EvoIR
          |
    Safety Certificate
    + Resource Information
    + EA Information
    + Evidence

ARCHITECTURAL REQUIREMENT

EA information is optional enrichment.

The following must remain true:

    Safety analysis does NOT depend on EA recognition.

For example:

    Non-EA + SAFE       → SAFE
    Non-EA + UNSAFE     → UNSAFE
    EA + SAFE           → SAFE
    EA + UNSAFE         → UNSAFE
    EA recognition = UNKNOWN → safety analysis still proceeds

Do not change SAFE / UNKNOWN / UNSAFE semantics.

FIRST: INSPECT THE REPOSITORY

Before modifying code, inspect:

1. Existing SafetyIR / IR definitions.
2. Existing analysis result classes.
3. Existing safety certificate/manifest structures.
4. Existing evidence/provenance representation.
5. Existing EA detector result structures.
6. Existing serialization/deserialization code.
7. Existing tests.
8. Any code that consumes the current IR.

Then report:

- Which existing representation should be extended.
- Which files should be changed.
- Whether an EvoIR structure already exists.
- What fields are already available.
- What minimum new fields are required.

Do not create a duplicate IR if an existing structure can be extended.

EVOR REPRESENTATION

The resulting representation should conceptually support:

    Program
      ├── program metadata
      ├── safety facts
      │     ├── taint findings
      │     ├── capability findings
      │     └── resource findings
      │
      ├── EA facts
      │     ├── population
      │     ├── fitness
      │     ├── selection
      │     ├── mutation
      │     ├── crossover
      │     ├── replacement
      │     └── termination
      │
      ├── evidence
      └── verdict

Each EA role should retain:

    role
    detection status
    confidence
    evidence
    source location
    explanation/reason

Use the existing project's naming conventions instead of blindly creating these exact field names.

EVIDENCE

EA inference must be explainable.

For example, for:

    for individual in population:
        score = objective(individual)

the representation should be capable of recording evidence such as:

    Population:
        evidence = iteration over population

    Fitness:
        evidence = objective(individual)

The exact implementation should follow the repository's existing evidence model.

CONFIDENCE

If the repository already has a confidence model, reuse it.

Otherwise introduce the smallest suitable representation.

Do NOT describe confidence as a statistically calibrated probability unless the implementation actually supports calibration.

SERIALIZATION

Inspect how the existing analysis result / SafetyIR is serialized.

If the project uses JSON, YAML, dataclasses, Pydantic, or another format, extend the existing mechanism.

The EvoIR output must be machine-readable.

BACKWARD COMPATIBILITY

Existing safety-analysis tests and outputs should continue to work unless a change is strictly required.

Existing programs that are not EAs must still be representable.

A program with no detected EA roles must not cause an error.

A program with incomplete EA evidence must still be representable.

TESTING

Add tests for:

1. EA program with several detected roles.
2. Non-EA Python program.
3. Program with partial EA structure.
4. Program where EA inference returns low/unknown evidence.
5. SAFE EA program.
6. UNSAFE EA program.
7. SAFE non-EA program.
8. UNSAFE non-EA program.

Verify that:

    EA inference does not alter safety verdicts.

Also test serialization/deserialization if the project has this functionality.

IMPORTANT: DO NOT implement resource enrichment yet.

We will use EvoIR later as the common representation for resource-bound enrichment.

DELIVERABLE

After implementation, report:

1. Files changed.
2. Existing IR structure reused.
3. New EvoIR fields.
4. How EA evidence is represented.
5. How confidence is represented.
6. How safety and EA results remain independent.
7. Tests added.
8. Test results.
9. Example EvoIR output for one EA program.
10. Remaining limitations.

Do not make unrelated refactoring.
