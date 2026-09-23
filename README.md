# devskills

Reusable skills for coding agents. Each skill follows the [Agent Skills format](https://agentskills.io/home) and contains instructions in a `SKILL.md` file.

## Available skills

| Skill | Purpose |
| --- | --- |
| [delegate](skills/delegate/SKILL.md) | Assign execution to capable, lower-cost subagents. Keep the main session responsible for advice, architecture, review, or a role you specify. |

## Install

Use the [`skills` CLI](https://www.npmjs.com/package/skills). Install Node.js 22.20 or later and Git first. npm includes `npx`.

Install `delegate` for Codex and Claude Code across your projects:

```sh
npx skills add xleepy/devskills --skill delegate --agent codex claude-code --global
```

For one project, run this command from the project directory:

```sh
npx skills add xleepy/devskills --skill delegate --agent codex claude-code
```

Select only `codex` or `claude-code` if you use one agent. Keep the default symlink option to share one installed copy between agents. The CLI manages the links.

List available skills without installing them:

```sh
npx skills add xleepy/devskills --list
```

List your global installations or update `delegate`:

```sh
npx skills list --global
npx skills update delegate
```

See the [CLI documentation](https://github.com/vercel-labs/skills#readme) for installation options and supported agents.

## Use delegate

In Codex:

```text
$delegate implement this task. Act as architect and reviewer.
```

In Claude Code:

```text
/delegate investigate this defect. Act as advisor. Report findings before making changes.
```

You can specify another main session role. The main session chooses each subagent's model and reasoning effort, assigns a bounded task, and checks the result. Subagents return short reports with evidence and artifact paths. Raw logs and large reports stay in files.

The skill requires native subagent support. Model and effort selection depend on the active host tools. The skill reports unavailable controls and does not change global model settings. An advisory or review task stays read-only unless you authorize changes.

## Repository layout

```text
skills/
└── delegate/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

`SKILL.md` contains the portable instructions. `agents/openai.yaml` contains optional Codex display metadata.

## Add or update a skill

Create `skills/<skill-name>/SKILL.md`. Start with YAML frontmatter:

```markdown
---
name: skill-name
description: Explain what the skill does and when an agent should use it.
---

# Skill Name

Instructions for the agent.
```

Use a lowercase name with letters, numbers, and single hyphens. Match the folder name. Keep the name within 64 characters and the description within 1,024 characters. Keep the main instructions short. Add `scripts/`, `references/`, or `assets/` only when the skill needs them. Link supporting files from `SKILL.md` so agents can load them when needed.

Before publishing, check discovery from the repository root:

```sh
npx skills add . --list
```

Check the skill against the [Agent Skills specification](https://agentskills.io/specification). Discovery alone does not verify behavior. Try a representative task in the intended agent and check that the result follows the skill's instructions.

## Further documentation

- [Agent Skills overview](https://agentskills.io/home)
- [Agent Skills specification](https://agentskills.io/specification)
- [skills package on npm](https://www.npmjs.com/package/skills)
- [skills CLI documentation](https://github.com/vercel-labs/skills#readme)
- [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)
