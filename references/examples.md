# Examples Library

## Contents
- [Metadata Examples](#metadata-examples)
- [Degree of Freedom Examples](#degree-of-freedom-examples)
- [5 Core Standards Examples](#5-core-standards-examples)
- [Progressive Disclosure Examples](#progressive-disclosure-examples)
- [Workflow Examples](#workflow-examples)
- [Full Skill Examples](#full-skill-examples)

---

## Metadata Examples

### name

```
Good:
  processing-pdfs           (gerund, lowercase, descriptive)
  analyzing-spreadsheets    (gerund, clear action)
  running-unit-tests        (gerund, specific)

Acceptable:
  pdf-processing            (noun phrase)
  spreadsheet-analysis      (noun phrase)

Bad:
  helper                    (vague — when would model trigger this?)
  data-skill-v2             (version in name)
  deployService             (camelCase)
  anthropic-helper          (reserved word)
```

### description

```
Good:
"Extracts text and tables from PDF files, fills forms, merges documents. 
Use when working with PDF files or when the user mentions PDFs, forms, 
or document extraction."

Bad:
"I can help you with PDF files"     (first person)
"Helps with documents"              (no trigger conditions)
"Does PDF stuff"                    (vague)
```

---

## Degree of Freedom Examples

### High Freedom: Code Review

Task: Review code for quality. Many valid approaches; context dictates what matters.

```markdown
## Code Review Process

1. Analyze code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions
```

### Medium Freedom: Report Generation

Task: Generate a report. Preferred structure exists but allows adaptation.

```markdown
## Report Generation

Use this template and customize as needed:

## Summary
- Brief overview of key findings

## Analysis
- Detailed breakdown of data, trends, root causes

## Recommendations
- Actionable next steps with priority indicators
```

### Low Freedom: Database Migration

Task: Run database migration. Order is critical; deviation breaks things.

```markdown
## Database Migration

Run exactly this script:
  python scripts/migrate.py --verify --backup

Do NOT modify the command or add additional flags.
Migration order is fixed. Do not skip or reorder steps.
```

---

## 5 Core Standards Examples

### Standard 1: Clear Boundaries

```markdown
Good:

**Use this skill when:**
- User requests CI/CD pipeline execution for unit tests
- PR is labeled "ready-for-merge" and needs automated checks

**Do NOT use this skill when:**
- User is only viewing test reports or CI/CD status
- PR has no code changes (documentation-only)


Bad:

**Use this skill when:**
- User wants to run tests on a pipeline
- PR has code or doc changes

**Do NOT use this skill when:**
- User is just looking at a PR
```

### Standard 2: Structured I/O

```markdown
Good:

Input:
  - prId: string          — Pull request ID
  - branch: string        — Target branch
  - runTests: boolean     — Execute tests flag

Output:
  - success: boolean
  - testReport?: object[] — Present if runTests=true
  - errorMessage?: string — Present on failure only


Bad:

"Help user run tests and return results."
```

### Standard 3: Executable Steps

```markdown
Good:

1. Validate PR: confirm prId and branch are valid
2. Checkout branch: git checkout {branch}
3. Run tests: execute suite per runTests flag
4. Collect results: aggregate pass/fail data
5. Update PR status: post results to PR
6. Notify: return error info if tests failed


Bad:

"Check the PR, run some tests, then update status."
```

### Standard 4: Failure Strategy

```markdown
Good:

On Failure:
  - Validation fails → Return 400 with validation errors
  - Test execution fails → Retry once; still failing? Return report with logs
  - CI/CD service unavailable → Retry 3x with backoff; log and notify admin


Bad:

(no failure handling defined — model improvises)
```

### Standard 5: Single Responsibility

```markdown
Good architecture (separate skills):

  running-unit-tests       → executes tests
  updating-pr-status       → updates PR status
  sending-notifications    → sends notifications
  running-lint-check       → runs linting


Bad architecture (god skill):

  ci-cd-orchestrator       → runs tests + updates PR + sends notifications + runs lint
                           → model confused about when to trigger
                           → description has too many keywords, pollutes discovery
```

---

## Progressive Disclosure Examples

### Good: Entry Point Pattern

```markdown
# PDF Processing

## Quick Start
Extract text with pdfplumber:
  import pdfplumber
  with pdfplumber.open("file.pdf") as pdf:
      text = pdf.pages[0].extract_text()

## Advanced Features
- **Form filling**: See FORMS.md
- **API reference**: See REFERENCE.md
- **Examples**: See EXAMPLES.md
```

### Good: Domain-Organized

```
analytics-skill/
├── SKILL.md              # Navigation only
└── reference/
    ├── finance.md        # Revenue metrics
    ├── sales.md          # Pipeline data
    └── marketing.md      # Campaign metrics
```

When user asks about sales → Claude reads only `reference/sales.md`.

### Bad: Deeply Nested

```
SKILL.md → advanced.md → details.md → actual_info.md
```
Claude may `head -100` intermediate files and miss critical info.

---

## Workflow Examples

### Good: Analysis Workflow (no-code)

```markdown
## Research Synthesis Workflow

Copy this checklist and track progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary → If citations incomplete, return to Step 3
- [ ] Step 5: Verify citations → If fails, return to Step 3
```

### Good: Code Workflow (with feedback loop)

```markdown
## PDF Form Filling Workflow

- [ ] Step 1: Analyze form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py) → If fail, return to Step 2
- [ ] Step 4: Fill form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py) → If fail, return to Step 2
```

---

## Full Skill Examples

### Example: `processing-pdfs` (Good)

```markdown
---
name: "processing-pdfs"
description: "Extracts text and tables from PDF files, fills forms, merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction."
---

# PDF Processing

## Quick Start
Extract text:
  import pdfplumber
  with pdfplumber.open("file.pdf") as pdf:
      text = pdf.pages[0].extract_text()

## Use this skill when:
- User mentions PDF, form filling, or document extraction
- User needs to merge or split PDF files

## Do NOT use when:
- User is viewing/reading PDF content in browser
- User asks about PDF format specifications (refer to docs instead)

## Workflow
- [ ] Step 1: Identify operation (extract / fill / merge)
- [ ] Step 2: Validate input file exists
- [ ] Step 3: Execute operation
- [ ] Step 4: Verify output → If incomplete, return to Step 2

## On Failure:
- File not found → Report path and suggest checking filename
- Corrupt PDF → Report and suggest repair tools
- Extraction fails → Try alternative library

## Reference
- Form filling: See FORMS.md
- API details: See REFERENCE.md
```
