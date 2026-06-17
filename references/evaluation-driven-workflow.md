# Evaluation-Driven Workflow

## Contents
- [Overview](#overview)
- [Step 1: Baseline Without Skill](#step-1-baseline-without-skill)
- [Step 2: Define Eval Cases, Failure-First](#step-2-define-eval-cases-failure-first)
- [Step 3: Minimal Viable Skill](#step-3-minimal-viable-skill)
- [Step 4: Expand Boundaries & Examples](#step-4-expand-boundaries--examples)
- [Step 5: Regression & Iteration](#step-5-regression--iteration)
- [Step 6: Real-World Calibration](#step-6-real-world-calibration)

---

## Overview

The 6-step "evaluation-driven, failure-first" loop. This turns skill creation from assumption-based into evidence-based.

```
Baseline → Eval → Minimal Skill → Expand → Regress → Calibrate → (loop)
```

Every addition to a skill must trace back to a specific eval case. No eval = no change.

---

## Step 1: Baseline Without Skill

Run the target task with NO skill loaded. Observe and record:

- Where does the model go wrong?
- Which inputs cause ambiguity or drift?
- Does the model "help" at the wrong time?

These failure points ARE your skill's requirements. If you can't name at least 3 failures, you may not need a skill.

---

## Step 2: Define Eval Cases, Failure-First

Before writing any skill code, write 3-5 eval cases. Each must have:

- **Scenario**: concrete input description
- **Pass criteria**: what correct behavior looks like
- **Fail criteria**: what wrong behavior looks like (equally important!)

Prioritize cases where the model is most likely to misuse or misfire the skill.

```
Example eval case:
Scenario: User says "show me the test results from last night"
Pass: Skill is NOT triggered (user is reading, not running tests)
Fail: Skill triggers and tries to run tests
```

---

## Step 3: Minimal Viable Skill

Write only enough to pass the eval cases from Step 2. Focus on:

1. **Negative conditions first** — explicitly encode failure scenarios from evals
2. **Shortest successful path** — the simplest valid input → stable output
3. **Single responsibility** — one problem, one verb

This is NOT the time for completeness. It's the time for correctness on known cases.

---

## Step 4: Expand Boundaries & Examples

Once the minimal skill passes all evals, add:

- More boundary scenarios with explicit behavior
- Structured Input/Output definitions
- Key input/output examples (good and bad)

**Rule:** Every new rule added must correspond to a new (or existing) eval case. Never add rules "just in case."

---

## Step 5: Regression & Iteration

Maintain the eval suite as a regression gate:

- New eval → incremental skill change
- Any skill change → re-run ALL existing evals
- If an eval fails → simplify the skill before adding more rules

Track: "no-skill baseline" vs "current skill + evals" success rate. If the skill doesn't measurably improve outcomes, it's dead weight.

---

## Step 6: Real-World Calibration

Evals only cover known cases. In live use, watch for:

- **False triggers**: Model using skill in unexpected scenarios
- **Missing context**: Model skipping reference files it should read
- **Repetitive reads**: Model re-reading the same section (sign of unclear structure)

Every such signal becomes a new eval case → feeds back to Step 2.
