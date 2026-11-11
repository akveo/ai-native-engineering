# Review briefs

Each check runs in a fresh, read-only subagent. Give it the brief below and
the inputs it names, and nothing else: not the conversation, and, except for
the plan check, not the plan.

The reviews of the change get the list of changed and new files from
`git status`, because a diff does not show new files. Read each new file in
full.

## Design check

Inputs: the design, the spec, the design format, and read access to the code.

The design is read by an engineer who has to approve it. Check that:

- every requirement is covered by some part of the approach;
- every area in the design format that the change touches has its line;
- every decision is a real choice between alternatives, with a reason;
- every choice the engineer must settle, such as a real trade-off, going
  against an architecture decision or pattern, or something new, is written
  with its alternatives and what each costs;
- no choice the reviewer would want to agree on is left to the plan;
- the design makes no product choice the spec does not contain.

Then look for what the design leaves silent: the choices that building it
will force, such as where the code goes, what it reuses, how data is stored
or moved, and what other systems see. Report each one a reviewer would want
to agree on and the design does not settle.

Then mark every passage that does not belong:

- the spec retold: a requirement in other words, or listed as the rules the
  code enforces, where citing its number would do;
- product answers or proposed changes to the spec, which belong in the spec;
- the plan's work: migration lists, column lists, the steps a function or
  module performs, how work is split, and choices left to the plan;
- facts no decision rests on;
- code, schemas, interfaces, file inventories, and code names no decision
  rests on;
- a "decision" that only adopts what the spec or an existing building block
  already settles.

Do not mark a passage only to shorten the design: a real choice, its reason,
a fact it rests on, and a risk stay, however long the design gets.

Report each problem with the passage or decision it concerns and what to
change. Report nothing when the design is sound.

## Plan check

Inputs: the plan, the spec, the design.

Check that:

- every requirement, design decision, ADR, and term the spec settles for the
  glossary has a task;
- no task makes a choice the approved design should have made;
- every task has a check that can actually run;
- the files and the dependencies between tasks are consistent;
- the tasks stay within the spec and the design.

Report each problem with the task it concerns and what to change. Report
nothing when the plan is sound.

## Spec review

Inputs: the spec, the design, the changed and new files.

Does the change do what the spec and the design say, and nothing else? Check
that every requirement has code and, where it applies, a test behind it; that
every design decision is followed; that every term the spec settles is in
the glossary; and that nothing was added that neither asked for.

## Code review

Inputs: the spec, the design, the project's coding standards if any, the
changed and new files.

Is the changed code correct, secure, maintainable, and consistent with the
project? Look for bugs, unhandled failure and boundary cases, missing or weak
tests, security problems, and code that ignores the project's existing
patterns or components.

Report code, comments, or tests that cite a requirement, decision, or task
number (R9, D3, T2) or Build's working files: these do not ship with the
change, so the reference cannot be followed. The reason belongs in words.

Check the change against every coding standard that applies to it, and name
the rule in each finding against one. A documented standard overrides general
taste: do not report code that follows a standard, even when you would write
it differently.

## Area review

Inputs: as for the code review, plus the area's line in the design.

Used when the change touches security and access control, data, or an
external contract. Check only that area, in depth: who can do what, what
happens to existing data and how to roll back, or what other systems relying
on the contract will see.

## Later rounds

From the second round on, a reviewer gets the findings the fixes address and
the files the fixes changed, instead of the whole change. Check that each
finding is resolved and that the fixes broke nothing. Report a new problem
only when a fix caused it.

## Finding format

Every reviewer reports findings in the same form:

- **Where:** file and line.
- **Breaks:** the requirement, design decision, or coding standard, or the
  plain defect.
- **What goes wrong:** a concrete input or state, and the wrong result; for a
  coding standard, the code that breaks the rule.
- **Fix:** a suggested fix.
- **Severity:** serious, or minor.

Problems in code the change did not touch, and anything a linter or the
typecheck would catch, are not findings. A review with no findings is a valid
result; do not report something to appear thorough.

## Validation

Inputs: the findings, the changed and new files, read access to the code.

For each finding, check it against the code and answer: confirmed, not
confirmed, or uncertain, with the evidence. Try to show that the failure in
"what goes wrong" actually happens. A finding you cannot confirm is dropped.
A serious finding that stays uncertain is marked for investigation, not
dropped.
