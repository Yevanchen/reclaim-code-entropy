---
name: reclaim-code-entropy
description: Find, rank, and safely remove accidental codebase complexity by proving real consumers, dynamic entrypoints, compatibility obligations, duplicate representations, speculative surfaces, and lifecycle ownership. Use when asked to simplify or clean up a repository, reclaim code entropy, reduce over-engineering or redundancy, find deletion candidates, collapse duplicate state/APIs, remove dead or added-then-abandoned code beyond static-tool output, or implement an evidence-backed simplification pass in any language or stack. Also trigger for 代码化简、熵回收、删代码、清理冗余、收敛抽象、去除过度设计. Do not use as a performance audit unless simplification is the stated goal.
---

# Reclaim Code Entropy

Treat code entropy as maintenance surface with no current load-bearing reason: extra representations, states, APIs, branches, packages, policies, or tests that the product must keep coherent.

Core rule: a scanner produces candidates; only consumer, ownership, history, and verification evidence justify deletion. Prefer a few proved cuts over a long speculative list. Finding nothing safe to remove is a valid result.

## Choose The Mode

- For "audit", "find", "review", or "report", inspect only and return ranked candidates. Do not edit.
- For "apply", "remove", "clean up", "simplify", or "refactor", implement the safest requested cuts and verify them.
- Treat removal of a reachable user capability, supported public API, persisted format, or compatibility path as a product decision. Surface the visible tradeoff before changing it unless the user already made that decision explicitly.

## Establish The Contract

1. Read repository instructions and the nearest scoped equivalents: `AGENTS.md`, `CONTRIBUTING`, architecture docs, ADRs/RFCs/decision notes, package manifests, and test guidance.
2. Inspect `git status`; preserve unrelated work. Identify generated, vendored, migration, fixture, and public-package paths before classifying code.
3. Trace the real runtime flow through entrypoints, configuration, registries, dependency injection, events, queues, persistence, processes/workers, and wire protocols.
4. In apply mode, discover the repository's actual narrow and broad verification commands and run a proportional baseline when feasible. A red baseline limits what later checks can prove; record it instead of claiming a regression.

Do not simplify away validation at trust boundaries, authorization, security controls, accessibility basics, data-loss prevention, durable-data compatibility, or cleanup that establishes resource quiescence.

## Survey For Entropy

Start with large or central production surfaces, not only obvious unused symbols. Use repository-native tools first: `rg --files`, `rg`, compiler/linter output, dependency manifests, and `git log`. Run installed dead-code or dependency tools when useful, but treat every result as a lead.

Look for these candidate classes:

1. **Unconsumed surface**: public methods, exports, events, config keys, hooks, packages, registry notifications, or protocol fields with no production consumer.
2. **Mirrored fact**: two events, caches, summaries, state stores, snapshots, or adapters record the same truth and must be synchronized.
3. **Speculative generality**: unset knobs, fixed feature flags, unused fallbacks, one-implementation interfaces, abandoned stubs, or extension points with no owner.
4. **Extra route or layer**: multiple front doors to one behavior, forwarding-only wrappers, pass-through packages, or helpers that obscure a single caller.
5. **Lifecycle duplication**: several flags, sentinels, promises, queues, or controllers represent one transition such as ready, stopped, settled, flushed, or disposed.
6. **Misplaced defense**: copies, freezes, validators, rollback paths, or hostile-object tests protect a same-process typed handoff rather than an actual trust or ownership boundary.
7. **Hand-rolled infrastructure**: local parsers, retry loops, globbing, framing, diffing, or data structures already covered by the standard library, native platform, or an installed dependency.
8. **Support-only residue**: tests or docs are the only consumers; duplicate expected outputs; obsolete inventories; demo/test packages that impose runtime or publishing cost.
9. **Added-then-abandoned residue**: implementation disappeared but flags, schemas, docs, tests, compatibility branches, or decision notes still describe it.

Do not confuse duplication with necessary independence. Separate backends, adapters, representations, or lifecycle mechanisms may intentionally test a contract or protect distinct owners.

## Prove Or Reject A Candidate

For each exact symbol, behavior, or artifact:

1. Search its symbol, file path, package name, config key, event/wire string, and alternate call syntax across the whole repository.
2. Classify every hit:
   - **Production**: runtime source, shipped configuration, real entrypoints, migrations, operational scripts.
   - **Non-production**: tests, docs, comments, snapshots, generated expected output.
   - **Ambiguous**: examples, fixtures, plugins, reflection, registries, lazy imports, code generation, externally consumed exports. Inspect before classifying.
3. Inspect callers and callees, not only search counts. Check dynamic loading, stringly typed dispatch, routes, plugin manifests, DI containers, serialization, environment-keyed lookup, and external package contracts.
4. Read history and decision records. Ask what problem created the surface, whether that problem still exists, and what evidence now beats the original rationale.
5. Draw ownership for asynchronous or stateful code. Map each state flag, disposer, cancellation path, readiness promise, and terminal outcome to a distinct owner or transition.
6. State what capability or behavior the deletion gives up, even if the answer is "none observable".
7. Estimate net reduction: code, tests dedicated only to that code, docs, config, generated artifacts, dependencies, and concepts removed minus replacement glue and migrations added.
8. Name the smallest check that would fail if the simplification were wrong.

Keep or downgrade a candidate when any of these holds:

- A real production or external consumer exists.
- Dynamic reachability or compatibility cannot be ruled out.
- A current decision record justifies the surface and new evidence does not beat it.
- The change is actually a feature or API decision, not cleanup.
- Churn moves complexity elsewhere without shrinking the contract or number of truths.
- A new dependency needs a wrapper and dedicated tests comparable to the deleted implementation.
- The candidate is tiny, uncertain, or unrelated to the requested scope.

Use this compact evidence record in audit output:

```text
[confidence / risk] candidate
evidence: production consumers; dynamic/public/compatibility checks; owning rationale
cut: exact code, artifacts, dependency, and concept removed
tradeoff: observable capability or behavior lost
verify: smallest decisive check; estimated net reduction
```

## Implement A Proven Cut

1. Work in one ownership boundary at a time. Fix the shared source of entropy rather than patching every caller.
2. Delete the obsolete contract end to end: declaration, implementations, branches, tests that exist only for the removed behavior, exports, config, docs, examples, snapshots, generated inventories, and dependency entries.
3. Preserve tests of the surviving observable contract. Tests are evidence, not an untouchable specification and not disposable merely to improve the line count.
4. Collapse mirrored state onto the load-bearing representation. Do not replace two truths with a synchronization wrapper.
5. Prefer deletion, then standard library/native features, then already-installed dependencies. Add a dependency only when it removes more implementation and dedicated testing burden than its glue and supply-chain cost add.
6. Avoid compatibility shims when there is no compatibility obligation. When one exists, keep the path or design an explicit migration instead of silently deleting it.
7. Keep batches reviewable and reversible. Never discard unrelated user changes.

Net-negative lines are evidence of a cut, not the goal. A safe simplification may add a small test or migration; a large deletion can still be wrong.

## Validate The Result

After each non-trivial batch:

1. Search again for deleted symbols, strings, paths, and stale documentation.
2. Run the narrowest decisive test first, then the repository's broad relevant type, lint, test, build, codegen, or smoke gates.
3. Re-run any static analyzer that produced the candidate.
4. Run `git diff --check` when Git is available and inspect the complete diff for accidental scope expansion.
5. Compare behavior at public, persisted, wire, and user-visible boundaries. Measure performance only if a performance claim is made.

If verification fails, identify whether the candidate was load-bearing, the implementation was incomplete, or the baseline was already red. Revert only the current batch or repair the proof; do not weaken a meaningful check to force the deletion through.

## Report The Outcome

For an audit, rank the strongest candidates by confidence, risk, and net maintenance reduction. Include rejected or uncertain high-value candidates only when the missing evidence is actionable.

For applied changes, report:

- what contract or duplicate truth was removed;
- files, lines, dependencies, and concepts removed where measurable;
- any user-visible capability or compatibility behavior changed;
- exact validation run and result;
- high-value candidates intentionally kept and why.

Do not claim safety from green tests alone, and do not claim value from deletion volume alone.
