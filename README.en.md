[🇺🇸 English](README.en.md) · [🇨🇳 中文](README.md)

# Trae Skill Creator

> A Trae-tuned writing and review framework for creating, reviewing, and improving AI agent skills on Trae. It aims to reduce false and missed triggers when used on Trae, and is built on an evaluation-driven, failure-first methodology.

一个针对 Trae 调优的写作与体检框架，帮你创建、审查、改进 Trae 上的 AI Agent Skill。目标是减少在 Trae 使用中的误触发与漏触发，基于「评估驱动、失败优先」方法论。

**Tuned for Trae** so your skills trigger and execute more reliably inside the Trae environment. You can also use it as a baseline optimizer when an existing skill underperforms on Trae.

![Source: Trae Docs](https://img.shields.io/badge/source-Trae%20Docs-blue)
![Source: Anthropic Claude](https://img.shields.io/badge/source-Anthropic%20Claude-191919)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

## How It Works

`trae-skill-creator` guides the creation and review of Trae skills through a practical framework:

- Define a single core action for each skill
- Write accurate trigger descriptions (name + description)
- Avoid vague or over-broad skill behavior
- Design structured input and output expectations
- Add failure handling instead of letting the agent improvise
- Use evaluation-driven iteration to improve reliability
- Split large guidance into reference files using progressive disclosure

## Recommended Use Cases

- Create a new Trae skill
- Improve an existing Trae skill
- Review a skill before publishing
- Diagnose wrong or missed triggers
- Split a large skill into smaller, focused skills
- Build more reliable, reusable Trae agent workflows

## Repository Structure

```text
trae-skill-creator/
├── SKILL.md
├── LICENSE
├── README.md
├── README.en.md
├── references/
│   ├── anti-patterns.md
│   ├── evaluation-driven-workflow.md
│   └── examples.md
└── THIRD_PARTY_LICENSES/
    └── README.md
```

## Installation

### Option 1: Install via `npx skills add`

```bash
npx skills add https://github.com/NicksonLin/trae-skill-creator.git
```

### Option 2: Clone via Git

Windows (PowerShell):

```powershell
git clone https://github.com/NicksonLin/trae-skill-creator.git "$env:USERPROFILE\.trae\skills\trae-skill-creator"
```

macOS / Linux:

```bash
git clone https://github.com/NicksonLin/trae-skill-creator.git ~/.trae/skills/trae-skill-creator
```

### Option 3: Manual copy

Copy this folder into your Trae skills directory:

```text
.trae/skills/trae-skill-creator/
```

### Verify installation

After restarting Trae or reloading skills, send a request related to Skill creation, review, or improvement. The skill should trigger automatically. If it does not, check that:

- The folder name is exactly `trae-skill-creator`
- A `SKILL.md` file exists at the folder root
- Trae has reloaded the skills list

## Core Principles

### 1. The Agent Is Already Smart

Only add context the agent actually lacks. Every instruction should earn its place in the context window.

### 2. Evaluation-Driven, Failure-First

Start from the cases where the model fails without a skill. A skill should solve observed failure modes, not encode speculative rules.

### 3. Single Responsibility

One skill should have one core action. If the skill name or description needs multiple unrelated verbs, split it into separate skills.

## Design Standards

Every skill should be checked against five standards:

1. Clear boundaries (positive + negative trigger conditions)
2. Structured input and output
3. Executable steps (imperative, not descriptive)
4. Failure strategy
5. Single responsibility

See `SKILL.md` for the full checklist.

## Recommended Workflow

1. Run the task without relying on a skill. Record the failure points.
2. Write 3–5 evaluation cases (scenario, pass criteria, fail criteria)
3. Create a minimal skill that passes those cases
4. Add boundaries, examples, and structured I/O
5. Re-run all evals after every change
6. Convert real-world failures into new eval cases

## Credits

This skill is built on top of publicly available official documentation and reference implementations from the projects below. The author gratefully acknowledges their work.

- Trae official documentation — `https://docs.trae.ai/`
- Anthropic Claude official documentation — `https://docs.claude.com/`
- Anthropic Claude Skill Creator reference — `https://github.com/anthropics/skills`

This repository reinterprets, restructures, and extends the original concepts with evaluation-driven, failure-first workflows and Trae-specific engineering constraints. Where passages, structural patterns, or examples were reused from the official Anthropic Skill Creator, the original authorship is retained and credited accordingly.

If you believe the attribution is incomplete, please open an issue and we will update the README as soon as possible.

## License

MIT License. See `LICENSE` for details. Third-party notices are collected in `THIRD_PARTY_LICENSES/`.
