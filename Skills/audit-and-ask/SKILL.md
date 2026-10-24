---
name: "audit-and-ask"
description: Audit a document the user provides, or a project if they ask, group the issues, then resolve them one group at a time through multiple-choice "ask me" questions, and finish with a cross-group refactor and decision log.
---

# Audit and Ask

Turn a bloated or contradictory document into a set of explicit decisions. Every answer settles one decision. After each answer, re-check the whole issue list so that consequences carry across groups instead of being fixed in isolation.

Use whenever the user wants to clean up a document, reduce scope creep or complexity, find conflicts or contradictions, resolve open questions, or says "ask me", "audit this doc", "review and decide", or "help me decide group by group", even if they don't name the method.

## Phase 1: Audit

1. Read every document provided. If two documents overlap, for example a GDD and a spec, treat the duplicated rules as an issue in itself.
2. List issues with stable IDs, `group.number`, for example 2.3. Look for:
   - Conflicts. Two places say different things, or a rule contradicts its own stated goal.
   - Circularity and softlocks in loops, economies and flows.
   - Scope creep. Features that exist only to support other features.
   - Undefined behavior. "May", "TBD", rules with no owner or no failure case.
   - Terminology drift. The same thing under several names.
   - Unproven targets presented as locked rules.
   - Stale open decisions that were already locked elsewhere.
   - Hygiene. Numbering, naming, duplicated rules, tests that don't match the rules.
3. Group the issues, 5 to 8 groups. Order groups by dependency. The root model whose answers change everything else goes first. Put document hygiene last because it depends on every other answer.
4. Present the groups with one line per issue, one sentence each. Then start Group 1 immediately; do not ask permission to begin.

## Phase 2: Ask loop (per group)

Ask one question at a time, using a multiple-choice question tool when one is available, such as a popup or any visual prompt that is not text only. If none is available, fall back to a short lettered list. The user chose this format because it is fast to answer on any device.

### Writing a question

- Name the issue IDs it addresses.
- 2 to 4 options. Put the option you recommend first and label it "(Recommended)". Give every option a one-line consequence, including the cost of that recommended option.
- Prefer the question that unlocks the most other issues. Do not ask about details that a prior answer will settle.
- Batch two or three questions in one popup only when they are independent and small.
- Say plainly when an option changes a rule the documents call "locked". Locked rules can be reopened if they cause the complexity, but the user decides.
- Bias toward simplification. When an option removes a system, say what it removes: transactions, tests, UI states, hub services.

### After every answer

Write a short notes block, then ask the next question. The notes block covers:

- Resolved. Which issue IDs are now closed.
- Partly resolved or changed. Earlier decisions this answer modifies.
- New issues. Add IDs, with a one-line reason.
- Doc cleanup queued. Sections that will need editing later. Do not edit the documents during the loop.
- Still open in this group.

Re-check the whole group each time. If an answer made a pending question moot, drop it. If it created a contradiction, ask about the contradiction next.

### Free-text answers

Treat them as the most informative answers. Restate your reading in one line, check it against prior decisions, and name any conflict. If the answer reopens an earlier decision, say so and ask a focused follow-up with options that reconcile the two.

## Phase 3: Group completion

When a group has no open items:

1. List the decisions as `D#` with one line each. Number them continuously across groups.
2. State the impact on the other groups: issues now resolved, issues that changed, new issues.
3. Refactor the remaining issue list. Mark resolved, renumber only if needed, add new items.
4. Start the next group's first question in the same message.

Use mechanical fixes for typos, numbering and glossary cleanup without a question, and mention them in the notes.

## Phase 4: Final pass

1. After the last group, run a short cross-check for residual details: values that need numbers, assumptions you made along the way, rules that no decision covers, and known trade-offs worth testing.
2. Summarize the resulting simplified model and a cut/deferred table. For a loop-based product, show the main loop in 5 to 7 steps.
3. Ask how to apply the result with a question: write a decision log only, write revised documents, run another review pass, or resolve residual details first.
4. Create files only after the user chooses. Respect any stated format preference, for example Markdown only and only when asked.

### Decision log contents

- Date, scope, and status.
- The simplified model.
- Decisions D1 through Dn, grouped by audit group.
- Cut/deferred table.
- Edit checklist per document section.
- Old-to-new ID map when tests or requirement IDs were renumbered, merged or removed.
- Residual details that still need values.

## Style

- Keep messages short. The user may be on a phone, so aim for about one screenful per turn, and use lists over prose for options and notes.
- Lead with the result; skip preamble.
- Never decide for the user. Recommend, explain, and let them override.
- Keep decisions traceable: every decision cites the issue IDs it resolved.
