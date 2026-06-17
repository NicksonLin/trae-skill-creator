# Anti-Patterns Checklist

## Contents
- [Metadata Anti-Patterns](#metadata-anti-patterns)
- [Structural Anti-Patterns](#structural-anti-patterns)
- [Content Anti-Patterns](#content-anti-patterns)
- [Workflow Anti-Patterns](#workflow-anti-patterns)

---

## Metadata Anti-Patterns

### Name: Vague or non-standard

| Anti-Pattern | Why it fails | Fix |
|-------------|-------------|-----|
| `helper` | Model can't determine when to trigger | Use gerund form: `processing-excel` |
| `data-skill-v2` | Version info in name; redundancy | Strip version: `analyzing-data` |
| `deployService` | Mixed case breaks convention | `deploying-services` |
| `anthropic-tools` | Reserved word | Remove reserved words |
| `pdf-tool` | Doesn't communicate action | `processing-pdfs` or `editing-pdfs` |

### Description: Wrong person or too vague

| Anti-Pattern | Why it fails | Fix |
|-------------|-------------|-----|
| "I can help you..." | First person injected into system prompt breaks discovery | "Processes Excel files..." |
| "You can use this to..." | Second person same problem | "Use when the user asks to..." |
| "Helps with documents" | No trigger conditions; model won't know when to use | "Extracts text from PDFs. Use when user mentions PDF extraction or document processing." |
| ≤200 chars when 1024 needed | Too brief to disambiguate from 100+ other skills | Use up to 1024 chars with keywords and trigger scenarios |

---

## Structural Anti-Patterns

### Skill = Prompt

| Symptom | Fix |
|---------|-----|
| Skill is just a long prose paragraph | Structure with headings, steps, conditions |
| No reusable structure; tailored for one conversation | Design for repeated invocation across sessions |
| Reads like documentation, not instructions | Write imperative "do X" not explanatory "X is a thing that..." |

### Multi-responsibility (God Skill)

| Symptom | Fix |
|---------|-----|
| One skill does "run tests + update PR + notify team" | Split into 3 skills, each with one verb |
| Description uses "and" to chain actions | If you can't describe it with one verb, split it |
| Body contains unrelated sections | Each section should trace back to the single core action |

### Deeply Nested References

| Symptom | Fix |
|---------|-----|
| SKILL.md → advanced.md → details.md → actual info | Keep all references one level deep from SKILL.md |
| Claude uses `head -100` on nested files | Direct links ensure full file reads |

### SKILL.md > 500 Lines

| Symptom | Fix |
|---------|-----|
| Everything in one file | Split into reference files, link from SKILL.md |
| No TOC on long reference files | Add table of contents for files ≥100 lines |

---

## Content Anti-Patterns

### Explaining What Claude Already Knows

| Anti-Pattern | Fix |
|-------------|-----|
| "PDF (Portable Document Format) files are a common..." | Just: `Use pdfplumber for text extraction` |
| "Git is a distributed version control system..." | Just: `Run: git checkout {branch}` |
| "JSON stands for JavaScript Object Notation..." | Just: `Output: {"success": true, "data": [...]}` |

**Test:** For every sentence, ask "Does Claude really need this explanation?"

### Vague Boundaries

| Anti-Pattern | Fix |
|-------------|-----|
| "Use when user wants to deploy" | Add negative: "Do NOT use when user is only checking deploy status or reading logs" |
| "Use for code review" | Add: "Use when user asks for code review, PR evaluation, or implementation feedback. Do NOT use for casual code discussion without review intent." |

### Descriptive, Not Imperative

| Anti-Pattern | Fix |
|-------------|-----|
| "The system checks PR validity and then runs tests" | "1. Validate PR. 2. Checkout branch. 3. Run tests." |
| "We should probably handle errors somehow" | "On validation failure: return 400 with field errors" |

### Time-Sensitive Information

| Anti-Pattern | Fix |
|-------------|-----|
| "Before August 2025, use v1. After, use v2." | Use collapsible "Old patterns" section for deprecated approaches. Always show current method first. |
| "Currently we're migrating to..." | Remove. Skills should describe stable state. |

### Inconsistent Terminology

| Anti-Pattern | Fix |
|-------------|-----|
| "document" in one section, "file" in another, "artifact" in a third | Pick one term; use it everywhere |
| Mixing "PR" and "pull request" randomly | Choose one; be consistent |

---

## Workflow Anti-Patterns

### No Feedback Loop

| Symptom | Fix |
|---------|-----|
| Skill runs to completion without any validation step | Add: "Validate → If fail, fix → Validate again → Only then proceed" |
| Assumes everything will work first try | Define failure paths with retry/recovery |

### No Checklist for Complex Tasks

| Symptom | Fix |
|---------|-----|
| >3 sequential steps with no tracking mechanism | Embed a markdown checklist Claude can copy and check off |
| Steps can be skipped without consequence | Number steps; add "Do not skip" to critical ones |

### Wrong Degree of Freedom

| Symptom | Fix |
|---------|-----|
| Database migration described as "look at the schema and figure it out" (too high freedom for fragile task) | Lock down: exact script, exact order, no deviation |
| Code review as a rigid 20-step checklist (too low freedom for context-dependent task) | Give principles: "Check safety first, then readability, then performance" |
