---
name: skill-optimizer
description: Audit other agent Skills for token inefficiency, score instruction quality, identify waste, and optionally refactor Skills for lower context cost without materially reducing capability.；中文触发：技能精简、指令去冗余、渐进式加载；English triggers: skill optimisation, token-efficient skills
---

# Skill Optimizer

## Purpose

Audit and optimise **other Skills** for token efficiency.

This is a meta-Skill. It does not perform the target Skill's domain task. It evaluates how efficiently the target Skill uses instructions, references, context, and workflow rules.

Primary targets:

- `SKILL.md`
- Skill-specific `AGENTS.md`
- `references/`
- prompt and rule files
- context-loading instructions
- workflow definitions
- duplicated instructions across Skill files

Core principle:

> Spend tokens on uncertainty, not ceremony.

## Scope

This Skill may:

- inspect Skill instruction files;
- score token efficiency;
- identify redundant or low-value instructions;
- detect unnecessary context loading;
- detect duplicated rules;
- detect rigid workflows;
- recommend progressive disclosure;
- estimate relative before/after instruction cost;
- refactor Skill files when explicitly requested;
- compare the original and refactored Skill.

Do not modify unrelated task-domain content unless explicitly requested. Papers, reports, datasets, business documents, application source code, and domain-specific analysis are out of scope.

## Default Mode

Default to **Audit Only**.

Do not modify files unless the user explicitly asks to optimise, refactor, rewrite, or apply changes. Prefer targeted inspection. Do not read an entire repository when the Skill structure can be understood from relevant instruction files. Do not load every reference file unless needed to verify duplication, routing, or hidden dependencies.

## Modes

### 1. Audit

Use when the user asks to check a Skill, review token efficiency, find wasted context, score a Skill, inspect `SKILL.md`, or inspect a Skill package.

Actions:

1. identify the Skill entry file;
2. inspect only the files needed to understand instruction structure;
3. score the Skill;
4. report waste and recommendations;
5. do not modify files.

### 2. Explain

Explain what is wasting tokens, why it matters, whether it affects every invocation or only some tasks, whether fixing it could increase information-loss risk, and the safest improvement. Prefer concrete file-level findings over generic advice.

### 3. Refactor

Use only when the user explicitly asks to change the Skill.

Goals:

- reduce unnecessary always-loaded instructions;
- keep the main `SKILL.md` compact;
- preserve essential behaviour;
- move specialised detail to on-demand references;
- remove duplication;
- replace broad "read everything" rules with targeted discovery;
- reduce unnecessary planning, recap, and confirmation steps;
- preserve safety and correctness constraints.

Do not edit unrelated files.

### 4. Compare

Use after a refactor or when the user provides two versions.

Compare:

- estimated instruction size;
- always-loaded context;
- reference-loading behaviour;
- duplication;
- workflow rigidity;
- likely information-loss risk;
- capability coverage.

Do not claim exact token savings unless measured.

## Scoring Framework

Score the target Skill from **0 to 100** using five dimensions.

### A. Redundancy — 25 points

Evaluate repeated rules, near-duplicates, duplicated instructions across `SKILL.md`, `AGENTS.md`, and references, and verbose explanations that do not change behaviour.

### B. Progressive Disclosure — 20 points

Evaluate whether specialised instructions are loaded only when relevant. High-scoring Skills keep the main file compact and route to targeted references only when needed.

### C. Context Loading — 20 points

Evaluate how the Skill acquires supporting information. Prefer identifying or searching likely files first, reading relevant sections, and expanding context only when needed.

### D. Workflow Rigidity — 15 points

Evaluate whether the Skill unnecessarily forces fixed sequences such as mandatory planning, recap, confirmation, or rereading for trivial tasks.

### E. Instruction Value — 20 points

Evaluate whether each always-loaded instruction materially changes behaviour. Flag low-information instructions that merely restate default model behaviour, such as "think carefully", "be accurate", or "use best practices", unless they encode a specific measurable requirement.

## Waste Severity

### HIGH WASTE

Issues affecting most or all invocations, such as:

- every task loads all references;
- `SKILL.md` is extremely large;
- repeated rules appear in several always-loaded files;
- full-repository reading is mandatory;
- long fixed workflows run for trivial tasks.

### MEDIUM WASTE

Conditional but meaningful cost, such as repeated planning, unnecessary recap, duplicated secondary rules, or references broader than necessary.

### LOW WASTE

Small inefficiencies with limited practical impact, such as minor duplicated phrasing or short low-value explanations.

## Audit Checks

### 1. Duplicate Rules

Look for the same rule in `SKILL.md`, `AGENTS.md`, references, prompt templates, and workflow files. Recommend one canonical location.

### 2. Main File Bloat

Flag specialised detail that could be loaded conditionally. Prefer a compact main Skill that contains purpose, scope, routing, core constraints, and links to targeted references.

### 3. Forced Full Reads

Flag rules such as:

```text
Read all files before starting.
Read every reference before every task.
Inspect the whole repository first.
Load all documentation into context.
```

Prefer:

```text
Identify likely relevant files first.
Search or inspect targeted sections.
Load additional material only when needed.
```

### 4. Weak Progressive Disclosure

A `references/` folder does not reduce context if every file is always loaded. Routing must make reference loading conditional.

### 5. Rigid Step Sequences

Flag mandatory planning, confirmation, recap, or rereading unless required for safety, correctness, irreversible actions, contractual output structure, or critical validation.

### 6. Repeated Planning and Recap

Flag workflows that repeatedly restate the request, create multiple plans, recap unchanged context, or summarise minor actions.

### 7. Excessive Confirmation

Flag approval gates for read-only inspection, reversible edits already requested, or low-risk operations. Keep confirmation for meaningful irreversible or high-risk actions.

### 8. Default-Capability Rules

Flag instructions such as:

```text
Think carefully.
Be accurate.
Use best practices.
Check your answer.
Do not make mistakes.
```

Do not remove a generic-sounding rule if it actually encodes a measurable requirement.

### 9. Conflicting Instructions

Flag incompatible output rules, contradictory approval requirements, conflicting file-reading rules, or duplicated rules with different scope. Recommend a single precedence rule or canonical source.

### 10. Hidden Always-On Cost

Identify specialised content loaded for every invocation, including long examples, edge-case catalogues, large schemas, extensive domain references, and full templates. Move them behind conditional routing when safe.

## Estimating Token Cost

If exact tokenizer measurements are unavailable:

- do not invent exact token counts;
- use words or characters only as a rough proxy;
- label estimates clearly;
- prioritise relative before/after comparison.

If measured token counts are available, report them separately from estimates.

## Audit Output

Use this compact format:

```text
SKILL TOKEN AUDIT

Overall Score: <0-100>/100

SCORES
- Redundancy: <0-25>/25
- Progressive Disclosure: <0-20>/20
- Context Loading: <0-20>/20
- Workflow Rigidity: <0-15>/15
- Instruction Value: <0-20>/20

HIGH WASTE
- <finding>

MEDIUM WASTE
- <finding>

LOW WASTE
- <finding>

REMOVE
- <specific instruction or section>

MERGE
- <duplicated rule groups>

MOVE
- <section> -> <suggested reference file>

REWRITE
- <current rule>
  -> <recommended rule>

EXPECTED EFFECT
- Always-loaded context: <High / Medium / Low>
- Behavioural rigidity: <High / Medium / Low>
- Information-loss risk: <High / Medium / Low>

TOP 3 CHANGES
1. <highest-impact change>
2. <next change>
3. <next change>
```

Keep the report concise unless the user asks for a deep audit.

## Refactor Strategy

When the user requests a refactor:

1. **Preserve intent.** Do not optimise away safety constraints, domain-critical rules, required output contracts, validation requirements, or essential routing logic.
2. **Remove duplication.** Keep one canonical form of each rule.
3. **Thin the main Skill.** Keep only purpose, trigger/scope, routing, essential constraints, and compact completion rules always loaded.
4. **Move conditional detail.** Put specialised content into targeted references that are loaded only when relevant.
5. **Replace broad reads.** Change global "read everything" rules into targeted discovery.
6. **Reduce ceremony.** Remove unnecessary planning, recap, repeated confirmation, and repeated self-check instructions.
7. **Validate coverage.** Confirm the refactored Skill still covers the original important behaviours.

## Before / After Comparison

After refactoring, provide:

```text
BEFORE / AFTER

Instruction footprint
Before: <estimate or measured tokens>
After:  <estimate or measured tokens>

Always-loaded files
Before: <count or description>
After:  <count or description>

Forced full reads
Before: <yes/no>
After:  <yes/no>

Progressive disclosure
Before: <weak/medium/strong>
After:  <weak/medium/strong>

Workflow rigidity
Before: <high/medium/low>
After:  <high/medium/low>

Capability coverage
Preserved: <yes/no/partial>

Information-loss risk
<low/medium/high>
```

If capability coverage is uncertain, say so.

## Example

Before:

```text
Before any task:
1. Read AGENTS.md.
2. Read all Skill files.
3. Read all references.
4. Inspect the full repository.
5. Produce a detailed plan.
6. Ask the user to confirm.
7. Execute.
8. Summarise each step.
9. Review all files again.
```

After:

```text
Read the applicable Skill instructions.
Identify the files relevant to the requested task.
Load specialised references only when needed.
Plan when task complexity warrants it.
Proceed with requested reversible work without unnecessary confirmation.
```

## Guardrails

Never reduce token usage by:

- hiding uncertainty;
- skipping necessary evidence;
- removing required safety checks;
- deleting important validation;
- weakening essential domain constraints;
- modifying unrelated user content;
- reducing reasoning below what the task reasonably needs.

When efficiency conflicts with correctness, prioritise correctness.

## Completion Rules

For **Audit**, return the score and findings and do not modify files.

For **Explain**, explain the most important causes of waste and prioritise concrete findings over generic advice.

For **Refactor**, modify only the relevant Skill instruction files, preserve important behaviour, and provide a short summary of changes.

For **Compare**, show before/after structure and relative token-efficiency impact without inventing exact savings.

Do not execute the target Skill's unrelated domain task.

## Optional v3 research-group interface

This Skill works independently. Only for a genuine cross-Skill task, read `references/research-group-interop-v3.md` on demand (relative to the Skill folder). Own **Skill instruction efficiency and progressive loading without lowering safety or domain coverage**; do not take over **Deleting safety constraints, refusal boundaries or verification rules just to reduce token count**. A suggested next specialist is not a completed tool call; preserve evidence and version in a small handoff.
