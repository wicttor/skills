# Agentic Skills

A collection of reusable skills and tools for AI agents. Each skill packages focused instructions for a particular workflow and lives in its own directory under [`Skills/`](Skills/).

## Skills

| Skill | Description |
| --- | --- |
| [Audit and Ask](Skills/audit-and-ask/SKILL.md) | Audit a document or project, group issues, and work through decisions one group at a time before a final consistency pass. |

## Repository layout

```text
Skills/
  <skill-name>/
    SKILL.md
```

Each skill directory contains its own entry point and may include supporting documentation or package metadata. To add a skill, create a directory under `Skills/` with a `SKILL.md` file containing the skill's name, description, and instructions, then add it to the table above.

## Releases

See the [changelog](CHANGELOG.md) for collection release history. The root [`package.json`](package.json) contains package metadata for publishing this collection.
