---
name: build
description: Turns an agreed spec, or a small, obvious change, into working, reviewed code. Agrees the technical design with an engineer, then plans, builds, and reviews the change without a human watching. Leaves the changes uncommitted.
argument-hint: "[issue number | small change description]"
disable-model-invocation: true
---

# Build

Turn an agreed spec into working, reviewed code. One run produces the work for
one pull request: the whole spec, or one slice.

The issue holds the product; Build settles the technical side and delivers
it. Build has two halves:

- **With the human:** check the input, investigate, design the approach, and
  get it approved. The reviewer is an engineer: make the choices the evidence
  supports yourself, and bring them the ones where reasonable engineers could
  differ.
- **Alone, after approval:** plan, run the tasks, review the result. Do not
  ask questions in this half: make the choices the design left to Build, and
  stop when a choice belongs to someone else. A stop with a clear reason is a
  good result; a guess that reaches review is not.

Build works from the issue and the design alone. If this conversation already
holds other work, suggest starting a fresh session before going on: earlier
discussion distracts and can carry choices nobody agreed to.

Work in the branch or worktree you are given. Do not create or switch
branches, commit, or push; that is the caller's job. Leave the result as
changes in the working tree.

Keep every message to the human short; they read all of it. Do not paste the
design or the plan into the chat: link to the files.

## Input

`$ARGUMENTS` is one of:

- **An issue number or URL.** Read the issue, its comments, and its linked
  issues.
- **A description** of a small, obvious change, such as "fix the date
  picker's default date". There is no issue. Restate the outcome and its check
  in a line or two and confirm them before going on; the confirmed outcome
  stands in for the spec everywhere below, and steps that change the issue are
  skipped. If the change needs a product
  decision or more than one pull request, it is not obvious: suggest creating
  an issue and running the specify skill on it, and stop.
- **Nothing.** Ask for an issue or a description.

A **slice** is a sub-issue whose body is its own spec: build what it says.
Read the parent for context only, such as the overall outcome and what earlier
slices delivered. A **parent with slices** is never built itself: list its
open slices, suggest the next one whose dependencies are done, and stop.

Also read the architecture decisions and glossary terms that concern the area,
and the project's coding standards, where the project has them.

## Working folder

Build keeps its work in `.work/<number>-<slug>/` at the repository root, or
`.work/<slug>/` without an issue, and creates the folder on its first run. If
`.work/` is not ignored by git, add it to `.gitignore` and tell the human.

The spec in the issue is the input; Specify leaves nothing else. The folder
holds:

- **`design.md`:** the design.
- **`plan.md`:** the plan with its progress, so a fresh session can resume
  where Build stopped.

Write every file so that it reads without the conversation. The plan is
written only after approval, so an existing `plan.md` means the design is
approved: check the working tree against its progress and continue from the
first unfinished task. With only `design.md`, show it again and ask for
approval. The folder is deleted once the ticket is done; Build does not
delete it.

## 1. Check the input

Build can start when the ticket says what the outcome is and how it will be
checked: usually an agreed spec. A ticket for an obvious change may skip
specifying and carry only the outcome and its check; that means no product
decision is needed.

Look for open product questions before investigating how to build: list what
the user will see, and check that the ticket settles each item. Typical gaps
are which options are offered, defaults, empty and error states, what is kept
or cleared, who can see what, and what exports and counts include.

Send the work back to specifying instead of asking when:

- the ticket does not state the outcome and how it will be checked;
- more than a few product questions are open;
- an open question would change the scope or the expected result.

Say what is missing, suggest running the specify skill on the issue, and
on confirmation mark specifying as its next action, as the project's ticket
tracking describes; without a ticket tracking file, only say so. Then stop.

When only a few small product questions are open, ask them first, marked as
product questions, in the format of step 3. The answers belong in the spec in
the issue, not in the design; update it on the human's confirmation at
approval. The spec is never changed quietly.

## 2. Investigate

Find what the design and the plan need, and nothing more:

- the files, modules, and symbols the change touches;
- what the change can build on: existing components, helpers, and similar
  features that already solve part of it, and how they are used today;
- the tests near the change, and the commands that run the tests, lint, and
  typecheck.

When a decision rests on an external fact, such as how a library, an
external service, or the database behaves, look it up instead of relying on
memory:

- start from the question and the evidence that would settle it, and stop
  when you have it;
- use primary sources: the official documentation for the version the project
  uses, the dependency's source code, or its changelog;
- record the fact in the design with its source. What you infer but could not
  confirm is an assumption, marked as such, and a candidate for a spike.

For larger work, subagents can explore separate areas in parallel. What you
find goes into the design, where a decision rests on it, and into the plan: its
constraints and each task's files and check carry what a fresh session needs.
When you find a problem outside the request, keep it out of the change and
mention it at approval, so the human can open an issue for it.

## 3. Design

### Trivial changes

A change is trivial when it follows an existing pattern in one place, makes no
choice a reviewer would want to agree on, and touches none of the areas listed
under "Write the design". For a trivial change, say the approach in one line,
write it as the summary in `design.md`, and go on to the plan without waiting
for approval. When in doubt, it is not trivial: ask for approval.

### Decide and ask

Decide yourself what the evidence settles: an existing pattern, an
architecture decision, a constraint in the code or the spec. These become
decisions in the design, with a short reason. Evidence settles technical
choices only; what users will see is a product question for step 1.

Ask the human only about:

- a real trade-off: the alternatives differ in cost, risk, or how hard they
  are to reverse, and the evidence does not settle it;
- going against an architecture decision or an established pattern;
- a new dependency or a new pattern in the codebase.

Open the first round with the approach in one or two sentences, so the
questions have a frame. Then ask in rounds: each round asks every open
question that does not wait on the answer to another. Number each question,
give it a short title, and put your recommendation on a line of its own.
Phrase it as a question that proposes the recommendation, so the human can
take it with a plain "yes". Name the alternatives and what each would cost.

> **Q1. Where the filter runs.** Should the project filter run in the browser
> on the rows already loaded, like the person search? Filtering on the server
> would need a new query and a cache change.
>
> → Yes, in the browser.

A reply such as "all good except 2" accepts the rest. Each answer becomes a
decision in the design. The rounds end when no open question would change the
approach. When a question cannot be settled by talking, such as how slow a
query really is, say so and propose a short spike instead of guessing.

Build does not split work; slicing is a product decision made in Specify. When
the change looks too big for one reviewable pull request, say so at approval:
the human decides whether to go on as one pull request or to send the work
back to specifying to slice it further.

### Write the design

Read [references/design-format.md](references/design-format.md) and write
`design.md` once the rounds have ended. When the change touches one of these
areas, the design addresses it explicitly, however small the change:

- **security and access control:** who can do what, and what changes;
- **data:** schema changes, migrations, and deleting or transforming data,
  including how to roll back;
- **external contracts:** public APIs, integrations, and anything other
  systems depend on, including compatibility;
- **building blocks:** the existing components, helpers, and services the
  change reuses, and any new dependency, pattern, or component, with the
  reason the existing ones do not fit.

The project's agent instructions may add areas to this list.

### Check the design

A fresh, read-only subagent checks the design before approval, because the
writer of a design rarely cuts its own text. Give it the design, the spec, the
design format, and the checklist in
[references/review.md](references/review.md#design-check). Apply the findings
once: cut what it marks, and add what it finds missing. When a finding exposes
a choice for the human, ask it as a new round first.

## 4. Approve

Link to `design.md` and ask for explicit approval in one message that also
lists what will change outside the code, so one confirmation covers it:

- product answers from step 1, to add to the spec in the issue;
- ADRs to write, for a decision that was chosen over a real alternative,
  would be costly to change later, and that a future reader could not work
  out from the code alone. They ship as files in this pull request;
- problems found outside the request, as issues to open;
- a warning when the change looks too big for one pull request.

End the message with: "Approve, and I'll run the rest without you." After
corrections, update the design and ask again. After approval, make the
confirmed changes to the issue, and from here on ask nothing.

## 5. Write the plan

Read [references/plan-format.md](references/plan-format.md) for the format.
The plan says what will change in the codebase and how, as a list of tasks:

- split the work into the smallest pieces that can each be checked on their
  own;
- every requirement, design decision, ADR, and glossary change in the spec or
  the design has a task;
- a coding standard that clearly concerns a task goes into the plan's
  constraints, so the task gets it right the first time;
- no implementation code: say what changes and where, and leave the code to
  the task. When a task's description grows toward the size of its code, cut
  it back;
- no choices the reviewer would have wanted to agree on. If the plan needs
  one, stop: the design needs another look with the human.

## 6. Check the plan

A fresh, read-only subagent checks the plan before execution. Give it the
plan, the spec, the design, and the checklist in
[references/review.md](references/review.md#plan-check). Fix the findings
once. If a finding needs a decision, stop: the design needs another look with
the human.

## 7. Execute the tasks

Run each task in a fresh subagent that receives only what it needs: the task,
the plan's constraints, the findings that concern it, what earlier tasks
produced, and where to find the spec and the design. It does not receive the
whole plan or this conversation.

Run tasks in series by default. Run them in parallel only when they cannot
get in each other's way: they touch different files and interfaces, neither
changes the database schema or the dependencies, neither regenerates code
the other uses, and they do not need the same running service, such as a
development server or a database. No more than three at a time. When in
doubt, run in series.

After each task:

1. For a behavior change, the test is written first and seen to fail.
2. Run the task's check yourself once the subagent has reported back; do not
   rely on its report alone.
3. Record the task as done in the plan's progress, with anything the next
   session would need to know. Record nothing before the result is in.

If a failure is still there after two attempts to fix it, a third patch
will not help. Change the approach: give the subagent more context, use a
stronger model, or split the task. If it still fails, stop.

## 8. Handle what you discover

| Discovery | Action |
| --- | --- |
| A small plan error: a wrong path, a renamed symbol, a missing step, a bug caused by the change itself | Handle it and record it in the progress |
| A choice the design left to Build, a pre-existing problem nearby, a check that cannot run | Decide or defer, and record it in the progress |
| A need the design did not cover: a new dependency or pattern, a schema, contract, or security change; a design decision that does not work | Stop: the design needs another look with the human |
| A requirement that is unclear, conflicting, or missing, or work beyond the spec | Stop: the work returns to specifying |
| A destructive or irreversible action, pushing or publishing, credentials, a failed package install, no progress after the fix limit | Stop for a human |

When Build stops, it leaves its changes in place, records where it stopped in
the plan's progress, and tells the human what it found, why it stopped, and
what comes next. Running the skill again resumes from the folder.

## 9. Review the result

When all tasks are done, run the full test suite, lint, and typecheck, and
remove code left over from abandoned attempts. Then review the whole change.

Run two reviews in parallel, each in a fresh, read-only subagent:

- **Spec review:** does the change do what the spec and the design say, and
  nothing else?
- **Code review:** is the changed code correct, secure, maintainable, and
  consistent with the project and its coding standards?

When the change touches an area the design must address, add a review focused on
that area. The reviewers see the spec, the design, the project's coding
standards, and the changes in the working tree, not the plan, so they judge the
result rather than the intention. Their briefs and the finding format are in
[references/review.md](references/review.md).

### Validate before fixing

A reviewer asked to find problems usually reports some, even when the work is
sound. Before fixing anything, a separate subagent checks each finding against
the code. Drop a finding it cannot confirm. Investigate a serious finding that
is uncertain instead of dropping it.

### Fix and repeat

Fix the confirmed findings as new tasks in fresh subagents, and run the
affected checks again. The next round reviews the result. Stop when a round
produces no confirmed findings worth fixing; record minor and pre-existing
findings in the progress. A serious finding that returns after a
fix points to a problem upstream: stop. After the third round, stop whatever
remains. Findings about the design or the requirements follow the table in
step 8.

## 10. Finish

1. Make sure the full test suite, lint, and typecheck pass on the final code.
2. Mark the plan as finished in its progress.
3. Update `design.md` to what was built, if the build changed anything.

Do not change the issue or its next action after approval. Do not commit or
open a pull request. End with the outcome in a line or two, a link to
`design.md`, and only what exists of: choices Build made that are not obvious
from the code, review findings left unresolved, and problems noticed but not
fixed.

If the work showed friction in the setup, add improvement suggestions after
the outcome. Friction is a moment in this session such as:

- a command, test environment, or piece of context you could not find, or
  found only after several attempts;
- a line in the agent instructions that was wrong or out of date;
- a mistake a check could have caught, such as one the review found;
- a review finding you can see in earlier pull requests too;
- a correction the human gave that they say they have given before.

For each, say what happened, what to change, and where: the code, a check, the
coding standards, the glossary, an architecture decision, or the agent
instructions. For a mistake that should not happen again, suggest a check
when a tool could catch it, such as a lint rule, a type, a test, or a CI job,
and a rule in the coding standards only when no tool could. Suggest only what
you can trace to a specific moment in this work, never general advice, and
make no changes yourself. Most builds have none; then say nothing about
improvement.
