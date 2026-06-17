[🇨🇳 中文](README.md) · [🇺🇸 English](README.en.md)

# Trae Skill Creator

> 一个针对 Trae 调优的写作与体检框架，帮你创建、审查、改进 Trae 上的 AI Agent Skill。目标是减少在 Trae 使用中的误触发与漏触发，基于「评估驱动、失败优先」方法论。

A reusable Trae skill for creating, reviewing, and improving AI agent skills with evaluation-driven best practices.

**面向 Trae 做了针对性优化**——让你的 Skill 在 Trae 上能更稳定地触发和执行。如果现有的某个 Skill 在 Trae 上表现不佳，也可以用它来做基础优化。

![Source: Trae Docs](https://img.shields.io/badge/source-Trae%20Docs-blue)
![Source: Anthropic Claude](https://img.shields.io/badge/source-Anthropic%20Claude-191919)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

## 本 Skill 的运作原理与步骤

`trae-skill-creator` 通过一套实用框架，引导 Skill 的创建与审查：

- 为每个 Skill 定义单一的核心动作
- 写出更准确的触发描述（name + description）
- 避免含糊或过于宽泛的行为边界
- 设计结构化的输入与输出
- 添加失败策略，避免模型临场发挥
- 用评估驱动（evaluation-driven）的方式迭代改进
- 用渐进披露（progressive disclosure）拆分大型说明

## 建议使用场景

- 创建一个新的 Trae Skill
- 改进一个已有的 Trae Skill
- 发布前审查一个 Skill
- 诊断 Skill 误触发、漏触发问题
- 把一个大 Skill 拆成更聚焦的小 Skill
- 搭建更稳健、可复用的 Trae Agent 工作流

## 仓库结构

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

## 安装

把本目录复制到你的 Trae skills 目录下：

```text
.trae/skills/trae-skill-creator/
```

安装完成后，在涉及 Skill 创建、改进或审查的场景中即可自动触发。

## 核心原则

### 1. Agent 已经足够聪明

只补充模型真正缺失的上下文。每一句指令都要为上下文窗口「挣得位置」，否则不要写入。

### 2. 评估驱动，失败优先

从一个无 Trae Skill 的基线开始，记录模型在哪些场景会出错。Skill 是为了解决**已观察到的失败**，而不是为了把假设性规则全部塞进去。

### 3. 单一职责

一个 Skill 只做一件事。如果 Skill 的名字或描述需要多个互不相关的动词，就把它拆成多个 Skill。

## 五项设计标准

每个 Skill 都应通过以下五项检查：

1. 清晰的触发边界（positive + negative）
2. 结构化的输入与输出
3. 可执行的步骤（imperative，而非描述性）
4. 明确的失败策略
5. 单一职责

完整检查清单见 `SKILL.md`。

## 推荐工作流

1. 不依赖 Skill，先执行目标任务，记录失败点
2. 写 3–5 个评估用例（每个用例包含场景、通过条件、失败条件）
3. 写一个最小可用的 Skill，让所有评估用例通过
4. 增加触发边界、示例与结构化 I/O
5. 每次改动后，重新跑全部评估
6. 把真实环境中出现的新失败转成新的评估用例

## Credits（致谢）

本项目基于以下公开的官方文档与参考实现进行二次创作，作者对其工作深表感谢：

- Trae 官方文档 — `https://docs.trae.ai/`
- Anthropic Claude 官方文档 — `https://docs.claude.com/`
- Anthropic Claude Skill Creator 参考实现 — `https://github.com/anthropics/skills`

本仓库对原始概念进行重新组织与扩展，加入「评估驱动、失败优先」工作流与面向 Trae 的工程约束。对于复用自官方 Skill Creator 的段落、结构与示例，已保留原作者署名并在此致谢。

如果你认为署名不充分，请提交 Issue，本人将第一时间更新 README。

## License

MIT License。详见 `LICENSE`。第三方声明汇总在 `THIRD_PARTY_LICENSES/`。