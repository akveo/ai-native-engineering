# Recurring checks

A recurring check is an agent that regularly looks at the project as a whole,
not at one change, and writes a report. People read the report and decide what
to do.

## Why they exist

Some problems are invisible inside a single change. Architecture drifts, terms
stop matching the glossary, documentation falls behind the code. Each change
looks fine on its own, and the cost shows only across many of them. Nobody
reviewing one pull request is in a position to see it.

Build's [improvement suggestions](build.md#improvement-suggestions) catch
friction within one piece of work. Recurring checks catch what builds up over
time.

## What to check

Start with the check that answers a real need, not with the whole list.

- **Architecture:** structure that makes the next change harder, such as
  shallow modules, leaky seams, and logic spread across many places. The
  [architecture check](../skills/architecture-check/SKILL.md) skill does this.
  Copy the [`skills/architecture-check`](../skills/architecture-check)
  folder into your agent's skills folder, such as `.claude/skills/` for
  Claude Code or `.agents/skills/` for Codex. Then run it on one area, or
  without one and it picks an area itself:

  ```
  /architecture-check src/orders
  ```

  Each run writes a dated report to `docs/reports/architecture-check/`, which
  keeps the latest report for each area checked. To run it on a schedule,
  such as a Claude Code routine or a CI job, commit the skill to the
  repository.
- **Domain language:** [glossary](project-context.md#domain-language) terms
  the code no longer uses, or other words used for them.
- **Documentation:** docs and agent instructions that no longer match the
  code, such as commands that changed or paths that moved.
- **Test health:** tests that fail at random, grow slow, or no longer test
  what their name says.
- **Recent work:** a look back over recent tickets and pull requests for review
  findings that keep recurring, builds that stopped, and work that went back to
  Specify.

Where a tool can do the job, use the tool. Dependency updates belong to
Renovate or Dependabot, and a rule that can be stated precisely belongs in a
lint rule or a test, which never forgets and costs nothing to run.

To add a check of your own, copy the architecture check's shape and keep its
rules:

- **It runs unattended.** It never asks a question and writes its assumptions
  into the report, because a check that needs someone sitting with it stops
  running.
- **It only writes a report.** A change nobody asked for needs the review the
  check was meant to save. The report reaches the repository through a pull
  request a human merges.
- **It looks at one area per run**, so it reads each part with care and covers
  the whole project over several runs.
- **Every finding has evidence, and the report is capped.** "Nothing found" is
  a valid result; one invented finding makes every later report harder to
  trust.
- **The reports are the state.** Each run starts from a fresh copy of the
  repository, so the reports record what was checked and when. A run writes
  a new dated report and replaces the previous one for the same area only, so
  the findings for other areas stay until those areas are checked again. A
  finding nobody acted on appears again while the code still shows it.

When a check keeps finding the same kind of problem, turn it into a lint rule
or a test, and stop asking the agent to look for it.
