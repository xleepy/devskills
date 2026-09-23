---
name: delegate
description: Delegate execution to lower-cost subagents while the main session acts as advisor, reviewer, architect, or a user-specified role. Use when the user asks for this division of work, task-based model and effort selection, or a main session with minimal execution context.
---

# Delegate

Keep the main session responsible for decisions and acceptance. Delegate investigation, implementation, and test execution to subagents. A subagent is a separate agent with its own task and context. Use this mode for the current task and its follow-ups until the user changes it.

## Main session role

Use the role the user specifies. Otherwise, combine advisor, architect, and reviewer: clarify the outcome, choose interfaces and constraints, divide work, assess evidence, and explain decisions. State the role once. Ask about the role only when the choice changes the requested outcome.

Read only the instructions, contracts, selected code, and evidence needed for these decisions. Assign broad searches, bulk reads, edits, and test runs to subagents. Review important changes directly; do not repeat a subagent's full investigation. Send corrections back to the responsible subagent.

Delegation does not expand the task. An advisory or review request stays read-only unless the user authorizes changes. Keep the user's approval requirements and repository instructions in every assignment.

## Choose model and effort

The main session chooses both settings for every assignment. Use the active tool schema and available model list. Prefer a lower-cost model than the main session when it can meet the task's acceptance criteria. Do not assume an older model is cheaper.

| Task | Model choice | Starting effort, when supported |
| --- | --- | --- |
| Targeted lookup, extraction, mechanical edit, known test command | Smallest capable model | Low |
| Bounded implementation or investigation with clear criteria | Lower-cost capable model | Medium |
| Subtle defect, complex behavior, or consequential review | Strongest suitable lower-cost model | High |

These are starting points. Choose effort from ambiguity, reasoning depth, and the cost of an incorrect result. Task length alone does not justify high effort. State the selected model, effort, and reason in one short sentence before dispatch. Keep the main session's model unchanged.

For example, use `gpt-6-luna` for bounded Codex work when the active tool lists it as a cheaper option. Use another model when the live capabilities require it. In Claude, consider `haiku` for simple work and `sonnet` for work that needs more capability, when available. Model names are examples, not a permanent catalog.

If a result fails acceptance, identify the specific gap. Refine the assignment or increase supported effort. Escalate the model when the evidence shows a capability problem. Keep escalation within the user's budget and model constraints. Avoid repeated identical retries. If no suitable lower-cost model exists, report that limit and choose the smallest permitted capable option. Do not claim savings without evidence.

## Dispatch a bounded task

Give each subagent one cohesive outcome. Use this assignment format, with only the fields the task needs:

```text
Outcome and acceptance criteria:
Role and scope: read-only or changes allowed; owned files or module
Context: working directory, applicable instruction paths, relevant files
Decisions and constraints: public behavior, interfaces, authorization limits
Verification: required checks and expected observable behavior
Artifacts: location for reports and raw logs
Return: status, result, evidence, unresolved issues, artifact paths
You are the execution subagent. Complete this assignment yourself.
Return decisions outside this scope to the main session. Do not redelegate.
```

Pass a short, self-contained brief. Include relevant user constraints that a fresh agent cannot infer from files. Have the subagent load the instructions that apply to its module. Give paths and targeted questions instead of copied repository files or full conversation history.

Run independent tasks concurrently only when useful and supported. Assign distinct write ownership. Serialize dependent work and edits to shared files, or use isolated worktrees when project instructions allow them. Keep architecture decisions in the main session while workers execute. Reuse a subagent for a correction in the same task. Start a fresh subagent for unrelated work.

## Keep context small

Keep a compact record of task, agent, model, effort, status, and evidence path. Store a file for this record only when the task is long enough to need it.

Require a final report of about 200 words or fewer by default:

- Status: complete, partial, or blocked.
- Result and changed file paths, with line references where useful.
- Verification commands and outcomes. Distinguish passed, failed, and not run.
- Remaining risks, decisions, or blockers.
- Paths to detailed reports and raw logs.

Allow more detail when needed to explain a consequential finding. Put raw logs, large diffs, search results, and long reports in files. Keep secrets out of reports. Receive summaries and read only the evidence needed to assess them. A status claim alone is not evidence of completion.

Use completion notifications or supported waits. Do not repeatedly poll transcripts. While agents work, handle architecture, acceptance criteria, or user decisions that do not duplicate their work.

## Apply the host's controls

### Codex

Use native subagent tools. If `collaboration.spawn_agent` exposes `model`, `reasoning_effort`, and `fork_turns`, set the first two explicitly and use `fork_turns: "none"` with the brief. A full-history fork can both copy unnecessary context and prevent model overrides. For other Codex tool schemas, use the equivalent supported controls.

### Claude Code

Use the native `Agent` tool, or its equivalent in the active version. Select the model explicitly when the tool supports it. Set effort through an exposed per-agent option or a compatible existing subagent definition. Select a fresh context when supported. Avoid `context: fork` on this skill: the skill's decisions belong in the main session.

If the host cannot set effort per subagent, report that effort is inherited or unavailable. Asking an agent to "think harder" does not configure reasoning effort. Do not change global settings to simulate a per-task choice.

For either host, distinguish requested settings from confirmed settings. If native delegation is unavailable, explain the limit and ask whether the user wants direct execution. Do not silently replace the requested workflow or launch a separate paid CLI session.

## Accept the result

Compare the result with the acceptance criteria. Inspect relevant artifacts and check verification evidence. Resolve conflicts and request focused corrections before accepting the work. Complete when all requested outcomes are verified, or report a concrete blocker with the next decision needed from the user. The final response states the result, verification, and remaining limits.

Host references, for use only when the active tools leave a control unclear:
- [Codex subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)
