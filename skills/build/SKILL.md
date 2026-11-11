---
name: build
description: Turns an agreed spec, or a description of an obvious change, into working, reviewed code, and sends anything less settled back to specifying. Agrees the technical design with an engineer, then plans, builds, and reviews the change without a human watching. Leaves the changes uncommitted.
argument-hint: "[description, path, link, or issue number]"
disable-model-invocation: true
---

# Build

Turn an agreed spec into working, reviewed code. One run produces the work for
one pull request.

The spec holds the product; Build settles the technical side and delivers
it. Build has two halves:

- **With the human:** check the input, design the approach, and get it
  approved. The approver is an engineer: make the choices the evidence
  supports yourself, and set out in the design the ones where reasonable
  engineers could differ.
- **Alone, after approval:** plan, run the tasks, review the result. Do not
  ask questions in this half, and do not change the spec: make the choices
  the design left to Build, and stop when a choice belongs to someone else. A
  stop with a clear reason is a good result; a guess that reaches review is
  not.

Build works from the spec and the design alone. If this conversation already
holds other work, suggest starting a fresh session before going on: earlier
discussion distracts and can carry choices nobody agreed to.

Work in the branch or worktree you are given. Do not create or switch
branches, commit, or push; that is the caller's job. Leave the result as
changes in the working tree.

Keep every message to the human short; they read all of it. Do not paste the
design or the plan into the chat: link to the files.

## Input

`$ARGUMENTS` says what to build: a description in the chat, from a one-line
change to a full spec, or where to find one: a file, a link, or an issue
number. Read it where it lives, with whatever it links to and any discussion
kept beside it, such as an issue's comments. Whatever it is, it is the spec
everywhere below, and step 1 decides whether it is enough to build from.
Without an argument, ask what to build.

When the spec belongs to larger work, such as an issue with a parent, read
the larger work for context only, such as the overall outcome and what is
already built. Build what the spec says.

## Working folder

Build keeps its work in `.work/<slug>/` at the repository root, named after
the work, such as `.work/date-picker-default/`. When the spec has an issue
number, the slug starts with it, such as `.work/142-empty-report-export/`.
Before creating a folder, look in `.work/` for one that belongs to the same
work, and continue there. If `.work/` is not ignored by git, add it to
`.gitignore` and tell the human.

The folder holds:

- **`spec.md`:** the spec, with the answers to product questions, only
  when it lives nowhere else, such as text in the chat. Subagents and a
  later session read it here; a spec that lives elsewhere is read where it
  lives.
- **`design.md`:** the design.
- **`plan.md`:** the plan, with each task's result and a log, so a fresh
  session can resume where Build stopped.

Write every file so that it reads without the conversation. The plan is
written only after approval, so an existing `plan.md` means the design is
approved: check the working tree against the tasks marked done and continue
from the first task that is not. With only `design.md`, show it again and
ask for approval.

## Asking questions

Most technical choices are not asked: they go into the design as decisions,
and the engineer reviews them there. Build asks only what the design cannot
carry:

- **product questions**, from step 1 or found while designing, because their
  answers belong in the spec;
- **a fork:** a technical choice whose answer would make a different design,
  not just a different line in it, such as extending a component or replacing
  it. A design written down the wrong branch is wasted;
- **a spike**, which needs a yes before Build runs it.

Ask in rounds: each round asks every open question that does not wait on the
answer to another. Open the first round with a frame in one or two sentences:
the outcome for product questions, the approach for the others. Number each
question and give it a short title. Put your recommendation on a line of its
own, and phrase the question so that it proposes the recommendation, so the
human can take it with a plain "yes". Name the alternatives and what each
would cost.

> **Q1. Exporting an empty report.** Should exporting an empty report produce
> a file with only the header row? Refusing the export instead would need a
> new error message.
>
> → Yes, a file with only the header row.

A reply such as "all good except 2" accepts the rest. The answers go to
different places:

- **Product answers** belong in the spec. Once the human has answered, add
  them where the spec lives and say in a line what was added. The human's
  answer is the confirmation; the spec is never changed without one, and
  never quietly. The design never holds product answers or lists changes to
  the spec.
- **Answers to a fork or a spike** become decisions in the design.

## When something comes up

After approval, Build meets what the plan did not foresee. Sort each
discovery by who owns the decision:

| Discovery | Action |
| --- | --- |
| A small plan error: a wrong path, a renamed symbol, a missing step | Handle it and update the task in the plan |
| A choice the design left to Build | Decide it, and record it in the plan's log |
| A problem outside the request, such as a pre-existing bug nearby | Keep it out of the change, and defer it in the plan's log |
| A check that cannot run | When another check proves the task, change the task to use it; when the environment is missing, such as a database, stop for a human |
| A need the design did not cover: a new dependency or pattern, a schema, contract, or security change; a design decision that does not work | Stop: add the choice to the design and ask for approval again, as in step 3. Finished tasks stay; once approved, update the plan and continue from where Build stopped |
| A requirement that is unclear, conflicting, or missing, or work beyond the spec | Stop: the work returns to specifying |
| A destructive or irreversible action, pushing or publishing, credentials, a failed package install | Stop for a human |

When Build stops, it leaves its changes in place, records where it stopped in
the plan's log, and tells the human what it found, why it stopped, and
what comes next. It also lists the problems outside the request deferred
so far, and any [improvement suggestions](#improvement-suggestions). Running
the skill again resumes from the folder.

## 1. Check the input

Before designing how to build, check that the input settles the product:
what the outcome is, how it will be checked, and what the product's users, or
the systems that rely on it, will notice. Build makes technical choices only;
a product choice made in the design is one nobody agreed to.

List what will be noticeable, and check that the input settles each item.
Typical gaps are which options are offered, defaults, empty and error states,
what is kept or cleared, who can see what, and what exports and counts
include. How far to look depends on the input:

- **An agreed spec:** its authors may have left details open on purpose.
  Raise a gap only when building would force a guess about it.
- **A short description**, such as "fix the date picker's default date":
  state the outcome and its check yourself, in a line or two, together with
  any gap that matters.

An open question is small when its answer changes one detail of the result,
such as a default or a message, and large when it changes the outcome, the
scope, or how the result is checked. Then one of three things happens:

- **Go on** when nothing is open.
- **Ask first** when two or three small questions are open, or a short
  description needs its outcome and check confirmed. Ask them in one round,
  as [Asking questions](#asking-questions) describes, and add the answers to
  the spec before going on.
- **Send back** when a question is large, more small ones are open, or one
  confirmation would not settle the outcome and its check. Say what is
  missing, suggest running the specify skill on it, and stop.

## 2. Design

Design settles how to build: find the evidence, make the technical choices it
settles, and set out for the engineer the ones it does not. Write down what
you find: nothing from this conversation reaches the tasks, only the design
and the plan do.

### Get oriented

Look at where the change lives and what it can build on: the files, modules,
and symbols it touches, and the existing components, helpers, and similar
features that already solve part of it, with how they are used today. Note
whether the change can be tested: a missing or broken test setup is a risk
for the design. For larger work, subagents can explore separate areas in
parallel.

### Sketch the approach

Say the approach in a few sentences, then list every choice it makes,
including those that already seemed obvious while getting oriented. A choice
is any point where the change could reasonably go another way, such as where
the code goes, what it reuses, how data is stored or moved, and what other
systems see. The list is for you: it is not shown in the chat or stored, and
each choice on it is settled below. The approach reaches the human in the
design's summary, or earlier as the frame of a question.

### Settle each choice

First set aside the choices that need no evidence:

- **Product:** what users will notice. Ask a small one as
  [Asking questions](#asking-questions) describes and add the answer to the
  spec; a large one sends the work back to specifying, as in step 1.
- **Left to the plan:** what the engineer would not want to agree on, such as
  naming, internal structure, or the order of tasks.

Settle each technical choice that remains with evidence before deciding or
asking: the code, the architecture decisions, the spec. When a choice rests
on an external fact, such as how a library, an external service, or the
database behaves, look it up instead of relying on memory:

- start from the question and the evidence that would settle it, and stop
  when you have it;
- use primary sources: the official documentation for the version the project
  uses, the dependency's source code, or its changelog;
- cite the source with the fact. What you infer but could not confirm is an
  assumption, marked as such, and a candidate for a spike.

Then each technical choice is one of two kinds:

- **Settled by the evidence:** decide it yourself. When there was something
  to weigh, it is a decision in the design with a short reason. When an
  existing pattern simply applies, it stays out of the design, unless it falls
  in one of the design format's areas.
- **For the engineer:** a choice you may not make alone, whatever the
  evidence shows: a real trade-off, where the alternatives differ in cost,
  risk, or how hard they are to reverse and the evidence does not settle it;
  going against an architecture decision or an established pattern; or a new
  dependency or pattern in the codebase. Do not ask it: it goes into the
  design as a decision with your recommendation, the alternatives, and what
  each costs, and the engineer settles it at approval. Only a fork is asked
  before writing, as [Asking questions](#asking-questions) describes.

Settling one choice can reveal another: add it to the list.

When a question cannot be settled by talking, such as how slow a query really
is, say so and propose a short spike instead of guessing. On the engineer's
yes, run it before writing the design, as long as it only reads or measures
and changes nothing the change ships; its result settles the choice. A spike
that needs more goes back to the engineer.

When you find a problem outside the request, keep it out of the change. It
goes into the plan's log as deferred, so the human sees it at the end
and decides what to do with it. A problem the change depends on, such as a
bug in the code it builds on, is not outside the request: it is a risk in
the design.

### Write the design

Read [references/design-format.md](references/design-format.md) and write
`design.md` once every choice is settled or set out for the engineer.

### Check the design

A fresh, read-only subagent checks the design before approval, because the
writer of a design rarely cuts its own text and does not see the choices it
made without noticing. Give it the design, the spec, the design format, read
access to the code, and the checklist in
[references/review.md](references/review.md#design-check). Apply the findings
once: cut what it marks, and add what it finds missing; a choice it finds
for the engineer becomes a decision like any other, unless it is a fork:
ask that one first, as [Asking questions](#asking-questions) describes. A
design that is only the summary has nothing to cut: skip the check.

## 3. Approve

Link to `design.md` and ask for explicit approval in one message. The
engineer approves the decisions there, and the ADRs they mark, or changes
any of them.

When the change looks too big for one reviewable pull request, say so in the
message. Build does not split work, because how to split it is a product
decision: the human decides whether to go on as one pull request or to send
the work back to specifying to split it.

End by making clear that this is the last point to change the approach:
after approval, Build runs the rest alone and stops only when a choice
belongs to someone else, such as "Approve, and I'll run the rest without
you."

The engineer may change decisions in the reply:

- **Picking an alternative the design lists**, when nothing else in the
  design rests on that decision, approves the design with that choice, such
  as "all good, but D2 on the server". Update the decision and go on.
- **Any other change**, such as an option the design does not list or a
  decision others rest on, means settling the affected choices again and
  updating the design. When the approach itself changed, check the design
  again. Then ask for approval again.

After approval, ask nothing.

Skip approval when there is nothing to approve: the design is only the
summary and the change does not look too big. Then say the approach in one
line and go on to the plan.

## 4. Write the plan

Read [references/plan-format.md](references/plan-format.md) and write
`plan.md`. First find what the tasks need to run on their own, such as the
tests near the change and the commands that run the tests, lint, and
typecheck. They go into the plan's constraints, together with any coding
standard that clearly concerns a task, so the task gets it right the first
time.

Every requirement, every design decision, every ADR the design adds, and
every term the spec settles for the glossary has a task. Each problem found
outside the request while designing goes into the log as deferred, with
what it is and where.

The plan makes no choice the engineer would have wanted to agree on. If it
needs one, stop: add the choice to the design and ask for approval again, as
in step 3.

## 5. Check the plan

A fresh, read-only subagent checks the plan before execution. Give it the
plan, the spec, the design, and the checklist in
[references/review.md](references/review.md#plan-check). Fix the findings
once. If a finding needs a choice the engineer would want to agree on, stop
as in step 4. A plan with a single task, or one written after approval was
skipped, has little to check: skip the check.

## 6. Execute the tasks

Run each task in a fresh subagent. It does not receive the whole plan or this
conversation, only what the task needs:

- the task and the plan's constraints;
- the `Done:` lines of the tasks it depends on;
- where to find the spec and the design;
- for a fix task, the finding it fixes.

Tell each subagent:

- for a behavior change, write the test first, run it, and report the
  failing output;
- the numbers of requirements, decisions, and tasks (R9, D3, T2) exist only
  in the spec and Build's working folder, and neither ships with the change.
  Code, comments, tests, and other files the change ships never cite them: a
  comment says the reason in its own words. ADRs and other permanent
  references may be cited where they help;
- do not commit;
- make no choice the task does not cover. Report it instead, and you handle
  it as [When something comes up](#when-something-comes-up) describes.

Run tasks in series by default. Run them in parallel only when neither
depends on the other in the plan, no more than three at a time. When in
doubt, run in series. Parallel tasks share the working tree, so run their
checks only once every task in the group has reported back.

After each task:

1. Run the task's check yourself; do not rely on the subagent's report alone.
2. Compare the files it changed with the task's `Files:`. A difference is a
   small plan error to record in the task, or a discovery to handle as
   [When something comes up](#when-something-comes-up) describes.
3. Mark the task done in the plan, with anything a later task or session
   would need to know. Record nothing before the result is in.

If a failure is still there after two attempts to fix it, a third patch
will not help. Change the approach: give the subagent more context, use a
stronger model, or split the task. If it still fails, stop, and say in the
log what was tried, so a later session does not try it again.

## 7. Review the result

When all tasks are done, run the full test suite, lint, and typecheck, and
remove code left over from abandoned attempts. Then review the whole change.

Run the spec review and the code review in parallel, each in a fresh,
read-only subagent. When the change touches security, data, or an external
contract, add an area review for each; building blocks are left to the code
review. Give each reviewer the inputs its brief in
[references/review.md](references/review.md) names, and not the plan, so it
judges the result rather than the intention.

### Validate before fixing

A reviewer asked to find problems usually reports some, even when the work is
sound. Before fixing anything, a separate subagent checks each finding against
the code. Drop a finding it cannot confirm. For a serious finding it marks
uncertain, try to reproduce the failure yourself, ideally with a failing
test: when it happens, the finding is confirmed; when it does not, record
the finding in the plan's log as unresolved.

### Fix and repeat

Record each round in the plan's log. Fix every confirmed finding, minor ones
too, as new tasks in fresh subagents, and run the affected checks again. A
note about code the change did not touch is not a finding: when it matters,
defer it in the log like any problem outside the request.

The next round reviews only what the fixes changed, with the findings they
address. Stop when a round confirms no findings. A serious finding that
returns after a fix points to a problem upstream: stop. After the third
round, stop and record whatever remains in the log. Findings about the
design or the requirements are handled as
[When something comes up](#when-something-comes-up) describes.

## 8. Finish

When the review fixed anything, run the full test suite, lint, and
typecheck again on the final code. When one fails, fix it as a new task; the
fix limit in step 6 applies. Then record in the plan's log that Build is
finished.

End with the outcome in a line or two and a link to `design.md`. Then say
how the change was verified and what was not, such as "not tried in the
running app" or "integration tests skipped: no database". Then list only
what exists in the plan's log, each group on its own:

- findings left unresolved, to look at before committing;
- problems outside the request, for the human to follow up, each with what
  it is and where.

## Improvement suggestions

When Build finishes or stops, add improvement suggestions if the work showed
friction in the setup. Friction is a moment in this session such as:

- a command, test environment, or piece of context you could not find, or
  found only after several attempts;
- a line in the agent instructions that was wrong or out of date;
- a mistake a check could have caught, such as one the review found;
- a correction the human gave that they say they have given before.

For each, say what happened, what to change, and where: the code, a check, the
coding standards, the glossary, an architecture decision, or the agent
instructions. For a mistake that should not happen again, suggest a check
when a tool could catch it, such as a lint rule, a type, a test, or a CI job,
and a rule in the coding standards only when no tool could. Suggest only what
you can trace to a specific moment in this work, never general advice, and
make no changes yourself. Most builds have none; then say nothing about
improvement.
