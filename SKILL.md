---
name: interview-plan-implement-review-loop
description: >-
  Plan and implement a repository change through one-question-at-a-time clarification,
  a written plan with explicit user approval, and a subagent implementation and
  review loop that resolves blocking findings. Use for approval-gated feature work
  and refactors where the user wants this workflow.
---

# Interview Plan Implement Review Loop

Use the user's conversational language for communication and plan documents.
Follow repository conventions for code, filenames, and existing artifacts.

## Investigate and clarify

The main agent investigates before delegating. Read the relevant code, docs,
tests, existing plans, and repository instructions. Identify concrete artifacts
for each applicable area and report gaps that affect the plan.

Before asking a question, show these four sections in order. Localize their headings.

1. **Collected findings** — Evidence from the five areas above.
2. **Working plan context** — Goal, current behavior, established decisions, and constraints.
3. **Still ambiguous** — A numbered list of the remaining decisions that can change
   scope, architecture, validation, rollout, data handling, or user-visible behavior.
4. **Next clarification question** — One question about a numbered decision, or
   an explicit indication that all material decisions are resolved.

Resolve factual questions from the repository. Ask the user when a material decision
requires their judgment. Ask one numbered question at a time. Put the question
first, then explain the current behavior, the ambiguity it resolves, and how the
answer would affect the plan. Offer two or three viable options labeled `A)`,
`B)`, and optionally `C)`. Describe the implementation and practical tradeoffs
for each option. End with a recommendation and ask the user to choose.

After each answer, refresh the four sections. If the user explicitly defers a
decision, record what they deferred and how work can proceed within the agreed
scope. Update the four sections one final time before writing the plan, even if
the initial investigation settles all material decisions.

## Write the plan and obtain approval

Once the material decisions are resolved, write the plan at the repository's
designated location. If it has no convention, use `docs/plans/plan_YYYYMMDDHHMMSS.md`
and create the directory if needed. Include the goal, current findings, decisions,
scope, implementation steps, validation, and risks. Map each previously listed
ambiguity to its decision in the plan.

Link the plan file and explicitly ask for approval. End the request by saying
that implementation starts after approval. Wait for the user's explicit approval
before assigning subagents or starting implementation. If the user revises the
scope, update the plan and obtain approval for the new scope. Keep the plan with
the implementation, including it in the pull request when one is in scope and
repository policy allows it.

## Implement and review checkpoints

Use Codex with subagent support and a Git repository where scoped checkpoint
commits are allowed. If the environment blocks either prerequisite, explain the
blocker before implementation and confirm any change to the approved workflow.

After approval, record the starting Git commit as the baseline and preserve any
pre-existing user changes. Keep unrelated changes out of checkpoint commits.
Assign implementation to one development subagent.
Assign review to a separate subagent after the first checkpoint is ready. Reuse
both sessions across passes while available. Give each subagent a task-specific
brief and use the main agent's model settings where supported. Use the host's
completion notification or waiting tool to get each response before advancing.

The main agent coordinates and verifies the work. The development subagent owns
implementation and fixes. The main agent may update the approved plan or
task records required by the repository after verifying the state they describe.

For each implementation or fix pass, provide the approved scope, relevant
decisions, file responsibilities, findings to address, and validation expectations.
Ask the development subagent to commit only the scoped changes, including the
approved plan if it belongs in the branch. Request the following evidence.

- Changed files, base SHA, and checkpoint SHA.
- Focused checks and tests, with the command, input scope, commit or range, and result.
- Completion status, remaining risks, and any blockers.

Use repository instructions to choose focused checks at intermediate checkpoints.
Include tests for directly affected behavior and consumers. If unrelated changes
could affect validation, check the recorded commit in an isolated checkout.

Give the reviewer the exact base and head SHAs, changed files, approved plan,
validation results, and prior findings with their reported resolutions. Ask for
a flat list of actionable findings with `P0`–`P3` priorities and file locations,
or `No findings`.

- First review — `baseline..checkpoint`.
- Fix review — `previous-checkpoint..checkpoint` and verification of all prior findings.
- Final review — `baseline..candidate` for the complete approved change.

Use the recorded SHAs in place of the labels above so each review target stays
stable. If a subagent response omits required evidence or exceeds the approved
scope, obtain a corrected result before advancing. Replace a subagent when its
session is unavailable or repeatedly fails to return usable results.

## Resolve findings and finish

The main agent evaluates each finding against the approved plan and evidence.

- **P0** — Critical correctness, security, data loss, or release blocker.
- **P1** — Significant bug, regression risk, or missing validation that blocks completion.
- **P2** — Concrete defect risk or violation of the plan, architecture, or an established invariant.
- **P3** — Minor improvement, preference, or incidental cleanup.

Send all unresolved P0, P1, and P2 findings to the developer in one fix batch.
Cover other instances of the same root cause within the approved behavior and
directly affected code and tests. Then send the new delta and the reported fixes
to the reviewer. Repeat without a new approval for each in-scope pass. Pause for
a new material decision, a scope change that requires approval, or a blocker
the main agent cannot resolve. If a user decision supersedes a finding, record
it in the plan and include it in the next review.

Once the checkpoint review clears all blockers, record the candidate SHA. Run
the repository's required final checks against that candidate, using an isolated
checkout if unrelated changes could affect the results. Then request the
full-range review. If validation or review requires a fix, create a new candidate
and rerun checks whose inputs changed. Reuse a recorded result only when its
inputs remain unchanged and repository policy permits reuse. Review the full
range of the new candidate before completion.

Finish once the approved scope is complete, required validation passes, and the
full-range review and prior finding resolutions leave no blocking findings.
Report the plan, implementation, validation, and any remaining P3 findings.
