---
name: plan-this
description: Turn a prompt, file, or document into an implementation plan that follows the bundled plan template. Run only when the user invokes /plan-this.
---

# /plan-this

Produce one implementation plan, as a Markdown file, from whatever the user passed: a free-text prompt, one or more files, or both. The plan must follow `assets/plan-template.md` exactly, with the same frontmatter, sections, and order.

The reader of the plan may be a human, a coding agent, or a different tool entirely. Write it so any of them can execute it without this conversation. Do not reference specific AI tools, agents, IDEs, or chat history, and do not write "as discussed". Define every term where it first appears.

## Workflow

1. **Read the source fully.** Read every file the user referenced. If the input is only a prompt, the prompt is the source.
2. **Index the source.** Assign `S1`, `S2`, … to each source (or to each section of a long document, e.g. `S1 §3`). Every unit, exclusion, and open decision cites these IDs, so a reviewer can trace any line of the plan back to what was asked.
3. **Separate what's fixed from what's open.** Fixed items are the requirements, constraints, exclusions, and approaches the source dictates. Open items are anything the source doesn't settle. Never invent requirements to fill gaps. Record them as Open Decisions with a recommended default. If a gap blocks the whole plan and you can't tell what is being built, ask the user one focused question before writing.
4. **Inspect the codebase if there is one.** Use real file paths and existing conventions in the `Files:` lists. If there's no codebase, propose paths that follow the stack's standard layout and note that choice in an Open Decision.
5. **Design, then decompose.** Sketch the High-Level Technical Design first; then break it into Implementation Units (see rules below).
6. **Write the file** (see Output) and reply with the path plus a summary of two or three lines: unit count, risk level, number of open decisions.

## Writing the units

Units carry most of the plan. Each one should be a vertical slice that can be built, tested, and reviewed on its own.

- **Write the test first.** Every unit lists a test file and its scenarios. Each acceptance criterion (`ACn.m`) maps to exactly one scenario with the same ID, so the implementer can write the failing test before the code. Write scenarios with real inputs and real expected outputs, and include an edge or failure case in every unit where one exists.
- **One behavior per criterion.** If an acceptance criterion contains "and", split it into two criteria.
- **Keep units small and ordered.** Prefer more, smaller units over few large ones. Dependencies must form a directed acyclic graph, and U1 has none. Order the units so the earliest ones produce something that works end to end.
- **Keep domain logic independent of frameworks.** Domain logic must not depend on frameworks, I/O, or delivery mechanisms, and dependencies point inward. Give each unit one responsibility. Name things by what they do in the domain, not by the pattern used.
- **Describe behavior, not code.** Goals and criteria state what and why. Leaving naming, class shapes, and internal structure to the implementer is fine. The design section points a direction rather than specifying the code.

## Other sections

- **Risk** in the frontmatter is the highest impact in the risk table, raised if the change touches auth, payments, data migration, or irreversible operations.
- **Alternatives:** list only the alternatives you weighed. If the source fixes the approach, add one line saying so.
- **Operational / Rollout Notes:** include only the items that apply. Otherwise write the single `Not applicable` line with the reason.
- **Open Decisions** and **Learning Gaps:** write `None.` when empty rather than deleting the section, so every plan has the same shape.

## Output

- Save to `docs/plans/<plan-id>-<kebab-title>.md` at the project root unless the user names another location.
- `plan-id` is `YYYY-MM-DD-NNN` using today's date; `NNN` is one higher than the highest existing plan with today's date in the target folder (start at `001`).
- `created` and `updated` are today; `version` is `1.0`.
- Remove all template placeholders and bracketed hints. A finished plan contains no `[...]`.

## Quality check before saving

Re-read the plan once as an implementer who has never seen the source:

- Every unit has Source, Goal, Dependencies, Files (including a test file), Acceptance Criteria, and matching Test Scenarios.
- Every AC ID has exactly one scenario with the same ID.
- Every out-of-scope item and open decision cites a source or names the unit it affects.
- No tool, agent, or vendor names appear unless the source requires them.
- Add nothing the source didn't ask for. Record anything uncertain as an Open Decision.
