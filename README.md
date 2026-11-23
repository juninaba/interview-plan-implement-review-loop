# Interview Plan Implement Review Loop

This Codex skill guides a repository change from requirements through implementation and review. Codex inspects the repository, asks one question at a time about decisions that need your input, and writes a plan. Once you approve the plan, separate subagents implement and review the change. They repeat the cycle until all blocking findings are resolved.

For example, when adding CSV export for search results, Codex asks whether to export all results or just the current page and records your decision in the plan. Each follow-up review checks the changes since the previous checkpoint. The final review covers the full commit range.

## Installation

You need Codex with subagent support and a Git repository where you can create checkpoint commits. To install the skill for your user account, run the following commands.

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/juninaba/interview-plan-implement-review-loop.git \
  "$HOME/.agents/skills/interview-plan-implement-review-loop"
```

If the skill doesn't appear in the skill list, restart Codex. [OpenAI guide to skills](https://learn.chatgpt.com/docs/build-skills)

## Usage

Open Codex in the repository you want to work on and include the skill name in your request.

```text
Use $interview-plan-implement-review-loop to add CSV export for search results.
```

The plan location and validation commands follow the target repository's conventions. The default plan location is `docs/plans/plan_YYYYMMDDHHMMSS.md`. For the full workflow, see [SKILL.md](SKILL.md).
