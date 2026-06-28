---
name: progressive-disclosure-guard
description: Utiliser quand un livrable risqué, plan, revue, migration, handoff, instruction d'installation, spec, prompt d'implémentation ou changement substantiel doit être finalisé ; rester silencieux pour typo, lookup, statut simple ou changement sans risque de preuve.
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
context:
  - Read source-of-truth files before relying on memory.
  - Read relevant code before interpreting prose, inventories, or cited paths.
  - Inspect code, scripts, skills, agents, hooks, and configs before scoring reviews.
  - For risky skill calls, check skill usefulness twice and justify material unread files.
  - Keep checks bounded; do not recursively invoke PDG on PDG itself.
---

<!--
GENERATED FILE - DO NOT EDIT DIRECTLY
source: pdg.skill.md
source_hash: cefa05b23617046128602608725436c5578f3b1f72f5053344b9ebc41587d107
generated_by: pdg generate-skills
target: claude
-->

# PDG - Progressive Disclosure Guard

## En bref

Le PDG vérifie qu'un livrable est prêt à être finalisé. Il force trois choses :
préserver l'existant, distinguer connu/inconnu et exiger une preuve réelle avant
de déclarer `done`.

Use this skill before finalizing specs, plans, implementation prompts,
architecture reviews, UX critiques, handoff docs, code reviews,
install/migration instructions, or substantial code changes. Ne pas l'invoquer
pour typo, formatting, lookup, statut simple ou changement sans risque de
handoff, contrat, source of truth, preuve, installation ou génération.

## Claude Mechanics

- Use Claude planning tools for task tracking.
- Use Claude subagents only when file ownership is disjoint.
- Do not run parallel agents on the same files.
- Do not mark a risky Claude implementation as reviewed by the same Claude context.

## Invariants

Always:

- read source-of-truth files and nearby code, scripts, skills, agents, hooks,
  configs, and tests before any review, score, approval, implementation decision,
  or constraint; never treat prose, inventories, or cited paths as evidence until
  the source is open;
- for reviews, comparisons, or scores, build a source-grounded claim matrix with
  `claim`, `source`, `verdict`, and `impact`;
- for risky skill calls, run the Skill Invocation Pass, check twice that the
  skill is needed, and mark material unread files `Unknown` with a reason;
- name behavior, files, callbacks, routes, stores, pipelines, generated outputs
  and install paths to preserve;
- turn vague words into `MUST` / `MUST NOT`, and require proof through a real
  command, route, install path, workflow, source, or artifact;
- label same-agent review as `PDG self-check, not independent review`;
- require `PDG-LARGE-FILE-JUSTIFICATION:` above 200 lines and
  `PDG-BROAD-FILE-JUSTIFICATION:` for broad `service`, `utils`, `manager`, or
  `handler` files.

Never:

- replace a working route, store, hook, pipeline, state machine, prompt path,
  persistence contract or install flow without end-to-end verification;
- create a parallel engine, store, router, workflow, generator, or doctrine file
  when an existing one should be extended;
- claim `done`, `safe`, `verified`, `tested`, `working`, `installed`, or
  `updated` without naming the proof checked;
- score, approve, reject, compare, or interpret a spec/review from prose alone when code, scripts, skills, agents, hooks, configs, or tests could confirm or falsify the claim;
- treat generated files as canonical when a source file and generator exist;
- bulk-load catalogs, doctrines, folders, fixtures or skill trees when a focused
  source answers the risk.

## Trigger Boundary

Invoke PDG when any of these are true:

- another human, Claude, Codex, or reviewer will execute the output;
- the work is a spec, plan, handoff, review, install or migration instruction;
- the diff changes behavior, public contracts, generated outputs, install steps, verification claims, or more than 3 files;
- wording could let a rushed implementer satisfy the text while breaking the intended behavior.

Stay silent when all of these are true:

- the task is typo-only, formatting-only, read-only lookup, one-command status, or similarly low-risk;
- no handoff, install instruction, behavior, contract, source-of-truth, generated output, or verification claim changes;
- no final answer needs to assert safety beyond the command or fact just observed.

If the boundary is ambiguous, run only a two-line trigger check: `PDG triggered: yes/no` and `reason: ...`. Continue with a full PDG pass only when the answer is yes.

## Mission Frame

Before expanding a task, identify mission, objective lock, explicit constraints,
forbidden outcomes, success criteria, source of truth, smallest useful step,
verification path, deliverable, and facts that are known, unknown, or
unverifiable.

Treat the request as a mission under constraints, not as rails and not as permission to invent a new objective.

Preserve explicit user instructions. If the requested path conflicts with safety, truth, feasibility, or the stated objective, name the conflict before changing method.

Freedom of method is not permission to silently change the mission.

## Mission Brief

For risky tasks, use a short brief: Mission, Objective Lock, Constraints, Success
Criteria, Progressive Disclosure Gates, Deviation Protocol, Verification
Protocol, and Deliverable.

Do not turn the mission brief into a giant plan. Use the smallest frame that prevents hidden drift.

## Deviation Protocol

No silent deviation from explicit user choices is allowed.

If you depart from the literal request, use this exact format before acting on the departure:

```text
DEVIATION: I am doing X instead of Y because Z.
```

The real objective can justify a different method only when the deviation is visible, bounded, and still respects higher constraints.

## Verification Protocol

Do not claim `done`, `safe`, `verified`, `tested`, `working`, `installed`, or `updated` unless you name the real command, route, preview, install path, workflow, source, or artifact checked.

If verification is not possible, write:

```text
NOT VERIFIED: [reason]
```

## Workflow

1. Decide whether PDG triggers; if not, say why in one line and stop.
2. Run the Skill Invocation Pass if a skill drives the work.
3. Inspect existing artifacts and overlaps before known/unknown classification.
4. Classify known knowns, known unknowns, unknown knowns, and unknown unknowns.
5. Name preserved behavior and source-of-truth files.
6. Red-team words such as `refactor`, `simplify`, `wire`, `reuse`, `support`,
   `migrate`, `install`, `generate`, `verified`, or `done`.
7. Convert ambiguity into `MUST`, `MUST NOT`, non-goals and forbidden shortcuts.
8. Require regression proof through the real workflow.
9. Label same-agent review as self-check.

## Skill Invocation Pass

When a skill is used for work that will produce a decision, review, score, handoff, implementation, install instruction, or durable artifact, start with a short invocation pass instead of dumping the whole skill context.

State the selected skill, entrypoint read, why it triggered, why a lighter answer is insufficient, minimum sources to inspect, and the rule for loading references, fixtures, scripts or extra skills.

Before finalizing, apply the unread-file rule:

- for every material file named, cited, proposed, scored, or used as support but not read, give one short reason or mark the related claim `Unknown`;
- do not list every file read unless it changes the decision;
- do not carry a large receipt in the main context when a focused justification answers the risk.

The goal is not a large receipt. The goal is to make unread evidence impossible to hide.

## Overlap Inspection Pass

Before interpreting a spec, review, score, or comparison, open the sources that could confirm or falsify each material claim. A path list, inventory, document outline, or prose summary is not evidence until the referenced source has been opened.

Inspect code, scripts, skills, agents, hooks, configs, and tests with `rg` or
focused reads. Inspection depth starts with files named in the diff plus one level of direct dependents: importers, callers, config consumers, generated
outputs, or install paths. Do not recurse unless a named risk justifies it. If
the set is large, inspect highest-risk dependents and mark the rest
`inspection bounded, residual risk noted`.

Output `artifacts inspected` and `overlap findings`; classify overlaps as
`reuse`, `extend`, `avoid`, `replace`, or `none`. If inspection is skipped or
blocked, mark the claim `Unknown`, cap confidence, and name the blocked source.

## Known/Unknown Pass

- **Known knowns:** explicit requirements, existing behavior, named files, routes, callbacks, contracts, tests, constraints, and sources already covered.
- **Known unknowns:** decisions still required before implementation; convert them into phase gates, required decisions, or explicit deferrals.
- **Unknown knowns:** assumptions hidden by "already done", "simple", "wire", "support", "MVP", "later", or "for now"; convert them into hard constraints or acceptance criteria.
- **Unknown unknowns:** failure modes the implementer is unlikely to check; convert them into regression proof, non-goals, monitoring/logging, or follow-up.

Bias toward uncertainty. If an item could fit multiple quadrants, classify it as unknown rather than known.

## Enforce Progressive Disclosure Everywhere

- Apply the smallest PDG pass that covers the named risk; expand only when
  evidence shows broader behavior, contract, source-of-truth, or generated-output
  risk.
- Docs/specs/plans: index first, focused pages second.
- Code: narrow entry point first, then split orchestration, domain logic, IO,
  state, persistence, rendering, prompt construction, and validation.
- Runtime/API/UI flows: summary or smallest real workflow first, details on
  demand.
- Prompts/agents/skills: select one domain or skill from name/description first,
  check twice that it is needed, then load references only when evidence requires
  them.
- Tests/verification: start with the shortest real workflow, then broaden by
  named risk.

A solution that works by dumping all knowledge, all domains, all tests, all UI, or all doctrine into one large artifact is a failed implementation unless explicitly requested.

## Documentation Generation Mode

When generating or updating durable docs with LLM help:

1. Build a source inventory before drafting: inspected files, routes, APIs, env
   vars, modules, data stores, generated outputs and unknowns.
2. Classify sources by audience, relevance and safety: user/product,
   architecture, operator, internal-only, stale, secret-bearing, generated,
   binary asset, or out of scope.
3. Generate in layers: overview/index first, focused pages second, cross-cutting
   architecture only when evidence supports it.
4. Preserve human overrides and curated source-of-truth sections. MUST NOT
   overwrite them silently.
5. Suggested questions, examples, summaries, limitations and dependencies MUST be
   answerable from named sources.
6. Removed behavior and stale questions MUST be removed unless retained as
   history.
7. Verify with deterministic checks plus a real route, preview, or workflow.

## Documentation Review Passes

When generated or updated documentation is durable, user-facing, or used by another agent, run three explicit passes after the first draft:

1. **Coverage pass:** compare inventory, changed files, routes, APIs, env vars, pages, modules, removed behavior, and generated outputs against the draft. Every relevant change appears in one intended place, or is listed as intentionally undocumented.
2. **Grounding pass:** every feature, dependency, architecture claim, limitation, default question, example, and suggested next action points to named source evidence. Inventory alone is not grounding; unsupported claims are removed, marked `Unknown`, or converted into questions for the human.
3. **Regression pass:** verify the real generated output path still works. Check links or previews, generated-file drift, preserved human overrides, stale removals, binary asset justification, and the product route or install path when applicable.

Every actionable review finding that is machine-checkable MUST become a fixture,
regression test, or checklist item before final. The final receipt names the
three passes, inventory counts or exclusions, skipped checks, and residual risk.

## PDD Mode

When durable documentation must be created, converted, updated, reviewed,
indexed, or consumed by a chatbot, and PDD is available, use it as the
documentation engine.

PDD is an external documentation engine contract, not a PDG dependency. PDG MUST
NOT import, vendor, or reimplement PDD runtime behavior. Require PDD receipts
before claiming completion: source inventory, source map, manifest, coverage,
grounding, regression, stale-removal when behavior disappeared, and preserved
human overrides.

For existing docs, convert or update through PDD so the output keeps artifacts,
evidence mapping, stale removal, and regression proof. For chatbots, consume PDD
artifacts or APIs; do not create a parallel scanner, generator, source map, or
review contract.

## Fallbacks

- Ambiguous trigger boundary: do the two-line trigger check and stop unless PDG clearly triggers.
- Source of truth missing: write `Unknown`, ask for the source, inspect a named file, or lower the score; do not proceed from memory as if it were fact.
- Verification blocked: report the exact blocked command or route, why it is blocked, and the narrower check that was still possible.
- Verification not run: write `NOT VERIFIED: [reason]` and do not claim `done`, `safe`, `verified`, `tested`, `working`, `installed`, or `updated`.
- No second reviewer: label the result `PDG self-check, not independent review`; provide the human validation card.
- Generated output drift: update only the canonical source or generator, regenerate, and do not hand-edit generated variants.

Human validation card: changed files; real workflow or command to inspect; expected result; risk the human is accepting; exact approval sentence: `Approved after human validation.`

## Pièges fréquents

- Confondre PDG avec une permission de charger tout le contexte.
- Ne jamais remplacer une preuve par une intention ou un résumé.
- NE PAS appeler une revue indépendante si le même agent a produit le diff.
- Hors périmètre : débat de priorité produit ou réécriture non demandée.

## Examples

Input: `Refactor auth flow and clean up callbacks.`
PDG output: triggered. MUST preserve current login/logout routes, session storage, callbacks, and tests. MUST NOT create a parallel auth pipeline. Required proof: run the real login workflow or named auth test.

Input: `Install PDG here for Codex.`
PDG output: triggered. MUST start with audit only, report exact files, preserve existing `AGENTS.md`, install `.agents/skills/progressive-disclosure-guard/SKILL.md`, merge the trigger block only after approval, and verify the installed path.

Input: `Fix README typo.`
PDG output: not triggered unless the edit changes install instructions, handoff text, verification claims, or generated output.

Input: `Rename 5 CSS variables for consistency across 8 files.`
PDG output: triggered because the diff touches more than 3 files, but minimal pass is enough if `rg` shows no external component depends on the old names and preserved behavior is listed. Full overlap inspection is not needed for cosmetic renames with no behavioral impact.

Input: `Change the default timeout from 30s to 60s in config.ts.`
PDG output: triggered even if the diff is one file, because it changes behavior other modules depend on. MUST name callers or config consumers, verify no test assumes 30s, and confirm the change does not mask a performance issue.

## Output

Add a section named `PDG pass` with: trigger decision; skill invocation pass if used; artifacts inspected; overlap findings; source-grounded claim matrix; material unread files and `Unknown` claims; known/unknown quadrants; bad implementation path; guardrail added; preserved behavior; forbidden shortcuts; regression proof required.

## Final Checklist

- [ ] trigger boundary checked;
- [ ] requested outcome, constraints, forbidden outcomes and success criteria identified;
- [ ] no silent reinterpretation of the user request;
- [ ] source of truth read or marked `Unknown`;
- [ ] material unread files justified or related claims marked `Unknown`;
- [ ] overlap inspection and source-grounded claim matrix completed;
- [ ] preserved behavior, non-goals and forbidden shortcuts named;
- [ ] dangerous wording constrained with `MUST` / `MUST NOT`;
- [ ] real verification path required or blocked verification reported;
- [ ] generated files treated as generated;
- [ ] generated docs include evidence manifest, preserved overrides, diff/archive and review passes;
- [ ] same-agent review labeled `PDG self-check, not independent review`.
