---
name: "audit-and-ask"
description: Audit a document provided or a project if asked for, group the issues, then resolve them one group at a time through multiple-choice "ask me" questions, and finish with a cross-group refactor and decision log.
---

# Audit and Ask

A structured way to turn a bloated or contradictory document into a set of explicit decisions. The value comes from forcing one decision at a time, re-checking the whole issue list after each answer, and carrying consequences across groups instead of fixing things in isolation.

Use whenever the user wants to clean up a document, reduce scope creep or complexity, find conflicts or contradictions, resolve open questions, or says "ask me", "audit this doc", "review and decide", or "help me decide group by group", even if they don't name the method.

## Phase 1: Audit

1. Read every document provided. If two documents overlap (e.g. GDD and spec), treat duplicated rules as an issue in itself.
2. List issues with stable IDs (`group.number`, e.g. 2.3). Look for:
   - **Conflicts**: two places say different things, or a rule contradicts its own stated goal.
   - **Circularity and softlocks** in loops, economies and flows.
   - **Scope creep**: features that exist only to support other features.
   - **Undefined behavior**: "may", "TBD", rules with no owner or no failure case.
   - **Terminology drift**: the same thing under several names.
   - **Unproven targets** presented as locked rules.
   - **Stale open decisions** that were already locked elsewhere.
   - **Hygiene**: numbering, naming, duplicated rules, tests that don't match the rules.
3. Group the issues (5 to 8 groups). Order groups by dependency: the root model whose answers change everything else goes first. Put document hygiene last because it depends on every other answer.
4. Present the groups with one line per issue. Keep each line to a single sentence. Then start Group 1 immediately; do not ask permission to begin.

## Phase 2: Ask loop (per group)

Ask **one question at a time**, using a multiple-choice question tool (popup, ui element, or any other visual not text only system) if one is available. If none is, fall back to a short lettered list. The user chose this format because it is fast to answer on any device.

### Writing a question

- Name the issue IDs it addresses.
- 2 to 4 options. Put the option you recommend first and label it "(Recommended)". Give every option a one-line consequence, including the cost of the option you recommend.
- Prefer the question that unlocks the most other issues. Do not ask about details that a prior answer will settle.
- Batch two or three questions in one popup only when they are independent and small.
- Say plainly when an option changes a rule the documents call "locked". Locked rules can be reopened if they cause the complexity, but the user decides.
- Bias toward simplification: when an option removes a system, say what it removes (transactions, tests, UI states, hub services).

### After every answer

Write a short notes block, then ask the next question. The notes block covers:

- **Resolved**: which issue IDs are now closed.
- **Partly resolved or changed**: earlier decisions this answer modifies.
- **New issues**: add IDs, with a one-line reason.
- **Doc cleanup queued**: sections that will need editing later. Do not edit the documents during the loop.
- **Still open** in this group.

Re-check the whole group each time. If an answer made a pending question moot, drop it. If it created a contradiction, ask about the contradiction next.

### Free-text answers

Treat them as the most informative answers. Restate your reading in one line, check it against prior decisions, and flag conflicts explicitly. If the answer reopens an earlier decision, say so and ask a focused follow-up with options that reconcile the two.

## Phase 3: Group completion

When a group has no open items:

1. List the decisions as `D#` with one line each. Number them continuously across groups.
2. State the impact on the other groups: issues now resolved, issues that changed, new issues.
3. Refactor the remaining issue list (mark resolved, renumber only if needed, add new items).
4. Start the next group's first question in the same message.

Use mechanical fixes (typos, numbering, glossary cleanup) without a question; mention them in the notes.

## Phase 4: Final pass

1. After the last group, run a short cross-check for residual details: values that need numbers, assumptions you made along the way, rules that no decision covers, and known trade-offs worth testing.
2. Summarize the resulting simplified model (for a loop-based product, the main loop in 5 to 7 steps) and a cut/deferred table.
3. Ask how to apply the result with a question: write a decision log only, write revised documents, run another review pass, or resolve residual details first.
4. Create files only after the user chooses. Respect any stated format preference (for example, Markdown only and only when asked).

### Decision log contents

- Date, scope, and status.
- The simplified model.
- Decisions D1..Dn grouped by audit group.
- Cut/deferred table.
- Edit checklist per document section.
- Old-to-new ID map when tests or requirement IDs were renumbered, merged or removed.
- Residual details that still need values.

## Style

- Keep messages short. The user may be on a phone: about one screenful per turn, lists over prose for options and notes.
- Lead with the result; skip preamble.
- Never decide for the user. Recommend, explain, and let them override.
- Keep decisions traceable: every decision cites the issue IDs it resolved.
