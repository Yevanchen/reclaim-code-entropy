# Reclaim Code Entropy

> An evidence-first Agent Skill for safely simplifying any codebase.

Inspired by the unusually deletion-heavy development practice visible in [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness), this skill turns “delete code” from taste into a repeatable engineering workflow.

English | [简体中文](./README.zh.md)

## The Problem

Codebases accumulate more than dead symbols. They accumulate duplicate truths, abandoned extension points, compatibility paths with no remaining consumer, lifecycle flags that describe the same transition, and wrappers that only move complexity around.

Static analysis can produce candidates. It cannot prove that a string-dispatched route, public export, migration, persisted format, plugin entrypoint, or asynchronous cleanup path is safe to remove.

## The Solution

`reclaim-code-entropy` makes simplification evidence-driven:

| Stage | Question |
| --- | --- |
| **Establish the contract** | What is public, persisted, generated, dynamic, or compatibility-sensitive? |
| **Survey entropy** | Where are the extra representations, states, APIs, layers, and policies? |
| **Prove the cut** | Who consumes it, why was it added, and what capability would disappear? |
| **Simplify one boundary** | Can the obsolete contract be removed end to end without replacement glue? |
| **Verify behavior** | What is the smallest check that would fail if the deletion were wrong? |

The skill prefers a few proved cuts over a long speculative cleanup list. Finding nothing safe to remove is a valid result.

## What It Finds

- unconsumed APIs, exports, config, events, and packages
- mirrored state and duplicate representations of one fact
- speculative abstractions, fixed knobs, and abandoned fallbacks
- forwarding-only wrappers and multiple routes to one behavior
- duplicated lifecycle state, readiness, cancellation, or cleanup
- defensive code on non-boundary, same-process typed handoffs
- hand-rolled infrastructure already covered by the platform
- test- or documentation-only residue from removed implementations
- code added for an idea that was later abandoned incompletely

## Modes

**Audit mode** ranks candidates by confidence, risk, tradeoff, and estimated net reduction. It does not edit the repository.

**Apply mode** implements the safest proved cut, preserves observable behavior, and runs proportional verification.

## Install

### Option A: Codex skill installer (recommended)

Ask Codex:

```text
Install the reclaim-code-entropy skill from https://github.com/Yevanchen/reclaim-code-entropy/tree/main/skills/reclaim-code-entropy
```

Start a new Codex task after installation.

### Option B: Manual install

```bash
git clone https://github.com/Yevanchen/reclaim-code-entropy.git
mkdir -p ~/.codex/skills
cp -R reclaim-code-entropy/skills/reclaim-code-entropy ~/.codex/skills/
```

## Use

Audit a repository without changing it:

```text
Use $reclaim-code-entropy to audit this repository and rank the safest simplification candidates.
```

Apply the strongest safe cut:

```text
Use $reclaim-code-entropy to simplify this repository. Preserve public and persisted behavior, implement one high-confidence cut, and verify it.
```

Investigate a specific area:

```text
Use $reclaim-code-entropy to check whether these lifecycle flags represent distinct states or duplicated ownership.
```

## Key Insight

Deletion volume is not the goal. The goal is reducing the number of facts, contracts, states, and concepts the team must keep coherent.

A large deletion can be wrong. A small deletion can remove an entire maintenance obligation. The decisive evidence is the consumer graph, ownership model, history, compatibility surface, and verification boundary.

## How to Know It Is Working

- cleanup proposals name exact consumers and compatibility risks
- diffs remove concepts instead of replacing them with new abstractions
- public, persisted, and dynamic entrypoints remain deliberate
- surviving tests describe observable behavior rather than deleted internals
- each applied batch is reviewable, reversible, and net simpler
- “keep it” is accepted when the evidence does not justify deletion

## Safety Boundaries

The skill does not simplify away authorization, trust-boundary validation, accessibility basics, data-loss prevention, durable-data compatibility, or cleanup required for resource quiescence.

It is not a performance audit unless simplification is the stated goal.

## Inspiration

The initial idea came from studying [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness/commits/master/) and its emphasis on simplification during agent-driven development. This project generalizes that lesson for repositories in any language or stack. It is independent and not affiliated with DeepSeek.

## License

MIT
