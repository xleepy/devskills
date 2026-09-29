---
name: delegate
description: Delegate execution to lower-cost subagents while the main session coordinates and reviews. Use when the user requests this division of work.
---

# Delegate

Keep decisions and acceptance in the main session. Assign investigation,
implementation, and test execution to subagents. A subagent is a separate
agent with its own task and context.

Use this mode for the current task and its follow-ups until the user
changes it. Follow the user's specified role. Otherwise, coordinate
the work and review the results.

## Keep scope and context bounded

Delegation does not expand authorization. Keep advisory and review tasks
read-only unless the user authorizes changes. Pass applicable user
constraints and repository instructions to each subagent.

Read only the instructions, interfaces, selected code, and evidence needed
to make decisions. Assign broad searches, bulk reads, edits, and test runs
to subagents. Inspect important changes directly without repeating the
full investigation.

## Select model and effort

Use the active tool schema and available model list. Prefer a lower-cost
model that can meet the acceptance criteria. Do not infer cost from a
model's age or name.

Choose supported effort according to the task:

| Task | Starting effort |
| --- | --- |
| Targeted lookup, extraction, mechanical edit, known test command | Low |
| Bounded implementation or investigation with clear criteria | Medium |
| Subtle defect, complex behavior, consequential review | High |

Adjust these starting points for ambiguity and the cost of an incorrect
result. State the selected model, effort, and reason before dispatch.
Keep the main session's model unchanged.

If no suitable lower-cost model is available, state that limit and use
a capable model within the user's constraints. If cost information is
unavailable, state that savings are unverified.

## Assign one outcome

Give each subagent a short, self-contained assignment. Include only the
fields needed for the task:

```text
Outcome and acceptance criteria:
Scope: read-only or changes allowed; owned files or module
Context: working directory, instruction paths, relevant files
Constraints: prior decisions, required behavior, authorization limits
Verification: required checks and observable results
Return: status, result, evidence, unresolved issues, artifact paths

Complete this assignment yourself. Do not redelegate.
Return decisions outside your scope to the main session.
```

Include relevant user constraints that a fresh agent cannot infer from
files. Give paths and targeted questions instead of full repository
files or conversation history.

Run independent tasks concurrently when useful. Give each subagent
distinct write ownership. Run dependent tasks and edits to shared files
in sequence. When using isolated worktrees, assign responsibility for
integration and verify the combined result.

Reuse a subagent for corrections to the same task. Start a fresh
subagent for unrelated work.

## Use the host's controls

Use native subagent tools and the controls exposed by the active host.

- **Codex:** When `collaboration.spawn_agent` exposes `model`,
  `reasoning_effort`, and `fork_turns`, set the model and effort explicitly.
  Use `fork_turns: "none"` with the self-contained assignment. A full-history
  fork can prevent model overrides and copy unnecessary context.
- **Claude Code:** Use the native `Agent` tool or its current equivalent.
  Select the model when supported. Set effort through an exposed per-agent
  control or a compatible existing subagent definition. Keep this skill
  in the main session; do not add `context: fork`.

Distinguish requested settings from confirmed settings. If per-agent
effort cannot be set, report that it is inherited or unavailable.
Instructions to "think harder" do not configure effort.

If native delegation is unavailable, explain the limit and ask whether
the user wants direct execution. Do not change global settings or launch
a separate paid CLI session as a substitute.

Consult these references only when the active tools leave a control unclear:

- [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)

## Check results and correct gaps

Request a short report with:

- Status: complete, partial, or blocked.
- Result and changed file paths.
- Verification commands and outcomes: passed, failed, or not run.
- Unresolved issues and paths to supporting evidence.

Use about 200 words by default. Allow more detail for consequential
findings. Store large logs and reports in files when permitted by the
task's scope. Keep secrets out of reports.

Track task, agent, model, effort, status, and evidence location. Use a file
only when the task needs a durable record. Use completion notifications
or supported waits instead of repeatedly polling transcripts.

Compare each result with its acceptance criteria. Inspect relevant
artifacts and verification evidence before accepting the result.

When acceptance fails, identify the gap and request a focused correction.
Increase effort or model capability when the evidence supports that change.
Stay within the user's budget and model constraints. Avoid identical retries.

Complete the task when all requested outcomes are verified. Otherwise,
report the concrete blocker and the decision needed from the user.
The final response states the result, verification, and remaining limits.
