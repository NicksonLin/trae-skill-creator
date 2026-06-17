---
name: "trae-skill-creator"
description: "Guides creation, iteration, and review of Trae skills following Claude official best practices plus Trae-specific engineering rigor. Use when user wants to create a new skill, improve an existing skill, audit a skill against standards, or asks how to write effective skills."
---

# Trae Skill Creator

Synthesized from **Claude official Skill Best Practices** + **Trae Skill Engineering Guide** + lessons from real-world skill failures.

## Core Philosophy

Three principles govern all design decisions:

1. **Claude is already smart.** Only add context Claude lacks. Every token must earn its place in the context window.
2. **Evaluation-driven, failure-first.** Start from what fails without a skill, not from assumptions. A skill without eval is amplifying uncertainty.
3. **Single responsibility.** One skill = one action verb. Complexity kills trigger accuracy.

---

## Quick Creation Flow

When user asks to create a skill, follow this decision tree:

```
User request
  ├─ "Create a new skill for X" → Go to [Direct Creation](#direct-creation)
  ├─ "Improve/iterate my skill Y" → Go to [Iteration Flow](references/evaluation-driven-workflow.md)
  └─ "Review/audit my skill Y" → Go to [Review Checklist](#review-checklist)
```

### Direct Creation

1. Clarify the skill's single core action (one verb).
2. Draft the `description` — third person, covering **what** + **when**. Max 1024 chars.
3. Choose a `name` — gerund form, lowercase + hyphens, ≤64 chars.
4. Determine **degree of freedom** (high / medium / low) based on task fragility.
5. Write SKILL.md body — apply [5 Core Standards](#5-core-standards).
6. Run the [Review Checklist](#review-checklist) before finalizing.
7. If≥3 reference files needed, use [Progressive Disclosure](#progressive-disclosure) pattern.

---

## Metadata Rules

### name

- Gerund form preferred: `processing-pdfs`, `analyzing-data`, `running-tests`
- Alternative acceptable: `pdf-processing`, `data-analysis`
- Lowercase letters, digits, hyphens only
- ≤64 characters
- No reserved words: `anthropic`, `claude`
- No XML tags

| Good | Bad |
|------|-----|
| `processing-pdfs` | `helper`, `utils`, `pdf-tool-v2` |
| `analyzing-spreadsheets` | `data`, `files`, `xlsxHandler` |
| `running-unit-tests` | `runTests`, `claude-pdf-helper` |

### description

- **Third person only.** Claude injects descriptions into system prompts. First/second person breaks discovery.
- Must answer two questions: **(1) What does it do?** + **(2) When to invoke it?**
- Include trigger keywords users naturally say.
- ≤1024 characters (Claude official limit; Trae's 200-char suggestion is too restrictive).

```
Good:
"Extracts text and tables from PDF files, fills forms, merges documents. 
Use when working with PDF files or when the user mentions PDFs, forms, 
or document extraction."

Bad:
"I can help you process PDF files"
"Helps with documents"
"Does stuff with data"
```

---

## Degree of Freedom

Match constraint level to task fragility.

| Level | When to use | How to write |
|-------|-------------|--------------|
| **High** | Multiple valid approaches; context dictates decision | Give principles/heuristics |
| **Medium** | Preferred patterns exist; some variation allowed | Give templates/pseudocode |
| **Low** | Fragile operations; strict sequence required | Give exact executable steps |

> Think of Claude as exploring terrain. A narrow bridge over a cliff needs guardrails (low freedom). An open field needs only a direction (high freedom).

---

## 5 Core Standards

Every skill must pass these. See [Anti-Patterns](references/anti-patterns.md) for what violates each.

### 1. Clear Boundaries

Define positive AND negative trigger conditions explicitly. The #1 skill failure mode is wrong-trigger, not wrong-execution.

```markdown
**Use this skill when:**
- User asks to deploy to staging or production
- PR is labeled "ready-for-deploy"

**Do NOT use this skill when:**
- User is only checking deploy status or logs
- PR contains only documentation changes
```

### 2. Structured I/O

Define input/output like a function signature — no vague hand-waving.

```markdown
**Input:**
- `prId`: string — Pull request identifier
- `branch`: string — Target branch name
- `runTests`: boolean — Whether to execute test suite

**Output:**
- `success`: boolean
- `testReport?`: object[] — Present only when runTests=true
- `errorMessage?`: string — Present only on failure
```

### 3. Executable Steps

Imperative, concrete actions — not descriptive summaries.

```markdown
Good:
1. Validate PR: check prId and branch validity.
2. Checkout branch: git checkout {branch}
3. Run tests: execute test suite per runTests flag.

Bad:
"Check the PR and run some tests, then update things."
```

### 4. Failure Strategy

Define what happens at each failure point. Never let Claude improvise error handling.

```markdown
**On Failure:**
- Validation fails → Return 400 with field-specific error messages
- Test execution fails → Retry once; if still failing, return report
- Service unavailable → Retry 3 times with exponential backoff; log and notify
```

### 5. Single Responsibility

One skill = one verb. If you need "and" in the skill name, split it.

```markdown
Good architecture:
  running-unit-tests → executes tests only
  updating-pr-status → updates PR status only
  sending-notifications → sends notifications only

Bad architecture:
  One mega-skill that runs tests + updates PR + sends notifications
```

---

## Progressive Disclosure

SKILL.md is an entry point, not an encyclopedia. Claude loads SKILL.md on trigger; it loads referenced files only as needed.

- **SKILL.md body ≤ 500 lines.** When approaching this, split into reference files.
- **One level deep.** All reference files linked directly from SKILL.md. No A→B→C chains.
- **TOC for long files.** Files ≥100 lines need a table of contents at the top.

```
my-skill/
├── SKILL.md              # Entry point: quick start + navigation
├── REFERENCE.md          # API, config, detailed params
├── EXAMPLES.md           # Annotated usage examples
└── scripts/
    └── validate.py       # Executed, not loaded into context
```

For domain-organized skills, split references by domain so Claude loads only relevant context:

```
analytics-skill/
├── SKILL.md              # Overview + navigation
└── reference/
    ├── finance.md        # Revenue, billing metrics
    ├── sales.md          # Pipeline, opportunities
    └── marketing.md      # Campaigns, attribution
```

---

## Workflow + Feedback Loop

For tasks with >3 steps or where intermediate results affect quality, embed a checklist.

```markdown
Copy this checklist and track progress:
- [ ] Step 1: Analyze inputs
- [ ] Step 2: Generate draft
- [ ] Step 3: Validate against constraints → If fail, return to Step 2
- [ ] Step 4: Finalize output
```

Core pattern: **validate → fix → repeat** until passing.

---

## Content Hygiene

- **No time-sensitive info.** Use "Old patterns" collapsible sections for deprecated approaches instead of "before/after date X" language.
- **Consistent terminology.** Pick one term and use it throughout.
- **Assume Claude knows.** Never explain what a PDF is, what JSON is, or how git works.

---

## Review Checklist

Before finalizing any skill, verify:

- [ ] `name`: gerund form, lowercase+hyphens, ≤64 chars, no reserved words
- [ ] `description`: third person, what+when, ≤1024 chars
- [ ] **Clear Boundaries**: positive + negative trigger conditions explicit
- [ ] **Structured I/O**: input/output defined like function signatures
- [ ] **Executable Steps**: imperative, not descriptive
- [ ] **Failure Strategy**: error paths defined, no room for improvisation
- [ ] **Single Responsibility**: one verb, no "and"
- [ ] **Degree of Freedom**: appropriate level chosen
- [ ] **Progressive Disclosure**: ≤500 lines, one-level-deep references
- [ ] **No time-sensitive info**: no "before/after date X" patterns
- [ ] **Consistent terminology**: one term throughout
- [ ] **Workflow checklist**: present for complex (>3 steps) tasks

For full anti-pattern details, see [Anti-Patterns](references/anti-patterns.md).

---

## References

- [Anti-Patterns Checklist](references/anti-patterns.md) — Common failure modes with fixes
- [Evaluation-Driven Workflow](references/evaluation-driven-workflow.md) — Full 6-step iteration process
- [Examples Library](references/examples.md) — Good vs bad examples for each principle
