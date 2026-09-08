# Skill Optimizer

A meta-Skill for auditing, trimming, and optimising other agent Skills to reduce unnecessary token usage.

Skill Optimizer reviews `SKILL.md`, `AGENTS.md`, references, prompt/rule files, context-loading behaviour, and workflow definitions. Its main purpose is to slim down Skills by reducing redundant instructions and always-on context without materially weakening their capabilities.

## What it does

- **Audit** — score a Skill from 0–100 for token efficiency
- **Explain** — show exactly where token waste comes from
- **Refactor** — trim and restructure a Skill when explicitly requested
- **Compare** — show before/after context cost, rigidity, and coverage

The scoring model covers:

- Redundancy
- Progressive disclosure
- Context loading
- Workflow rigidity
- Instruction value

## Core principle

> Spend tokens on uncertainty, not ceremony.

Skill Optimizer focuses on the instruction layer of other Skills. It does not optimise papers, datasets, business documents, or unrelated source code.

## Install

Use the repository's `SKILL.md` as a Skill in a compatible agent environment.

## Typical use

```text
Audit this Skill for token efficiency. Do not modify files.
```

```text
Refactor this Skill to reduce always-loaded context while preserving important behaviour, then compare before and after.
```

## Output

Audits return a compact report with:

- Overall score
- High / medium / low waste
- REMOVE / MERGE / MOVE / REWRITE recommendations
- Expected effect on always-loaded context
- Information-loss risk
- Top 3 changes

## Licence

No licence file is included yet.
