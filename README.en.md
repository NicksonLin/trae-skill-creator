[🇺🇸 English](README.en.md) · [🇨🇳 中文](README.md)

# Trae Skill Creator

> A reusable Trae skill for creating, reviewing, and improving AI agent skills with evaluation-driven best practices.

一个用于创建、审查与改进 AI Agent Skill 的可复用 Trae Skill，基于「评估驱动、失败优先」的最佳实践。

![Source: Trae Docs](https://img.shields.io/badge/source-Trae%20Docs-blue)
![Source: Anthropic Claude](https://img.shields.io/badge/source-Anthropic%20Claude-191919)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

## What This Skill Does

`trae-skill-creator` guides the creation and review of Trae skills through a practical framework:

- Define a single core action for each skill
- Write accurate trigger descriptions (name + description)
- Avoid vague or over-broad skill behavior
- Design structured input and output expectations
- Add failure handling instead of letting the agent improvise
- Use evaluation-driven iteration to improve reliability
- Split large guidance into reference files using progressive disclosure

## When To Use It

Use this skill when you want to:

- Create a new Trae skill
- Improve an existing Trae skill
- Review a skill before publishing
- Diagnose why a skill triggers incorrectly or fails to trigger
- Refactor a large skill into clearer, smaller skills
- Build reusable agent workflows with stronger operational boundaries

## Repository Structure

```text
trae-skill-creator/
├── SKILL.md
├── LICENSE
├── README.md
├── README.en.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── references/
│   ├── anti-patterns.md
│   ├── evaluation-driven-workflow.md
│   └── examples.md
└── THIRD_PARTY_LICENSES/
    └── README.md
```

## Installation

Copy this folder into your Trae skills directory:

```text
.trae/skills/trae-skill-creator/
```

After installation, the skill can be triggered when working on skill creation, skill improvement, or skill review tasks.

## Core Principles

### 1. Claude Is Already Smart

Only add context the model actually lacks. Every instruction should earn its place in the context window.

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

1. Run the task without a skill and record failure points
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

If you believe proper credit has not been given, please open an issue and the README will be updated.

## License

MIT License. See `LICENSE` for details. Third-party notices are collected in `THIRD_PARTY_LICENSES/`.