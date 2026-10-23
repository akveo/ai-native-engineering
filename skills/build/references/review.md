# Review briefs

Each check runs in a fresh, read-only subagent. Give it the brief below and
the inputs it names, and nothing else: not the conversation, and, except for
the plan check, not the plan.

## Design check

Inputs: the design, the spec, the design format.

The design is read by an engineer who has to approve it. Check that:

- every requirement is covered by some part of the approach;
- every area the design must address is addressed: security and access
  control, data, external contracts, and building blocks;
- every decision is a real choice between alternatives, with a reason;
- no choice the reviewer would want to agree on is left to the plan;
- the design makes no product choice the spec does not contain.

Then mark every passage that does not belong:

- the spec retold: a requirement in other words, or listed as the rules the
  code enforces, where citing its number would do;
- the plan's work: migration lists, column lists, the steps a function or
  module performs, how work is split;
- code, schemas, interfaces, file inventories, and code names no decision
  rests on;
- a "decision" that only adopts what the spec or an existing building block
  already settles.

A design longer than about three pages almost always has such passages, so
look hardest there. Do not mark a passage only to shorten the design: a real
choice, its reason, a fact it rests on, and a risk stay, however long the
design gets.

Report each problem with the passage or decision it concerns and what to
change. Report nothing when the design is sound.

## Plan check

Inputs: the plan, the spec, the design.

Check that:

- every requirement, design decision, ADR, and glossary change has a task;
- no task makes a choice the approved design should have made;
- every task has a check that can actually run;
- the files and the dependencies between tasks are consistent;
- the tasks stay within the spec and the design.

Report each problem with the task it concerns and what to change. Report
nothing when the plan is sound.

## Spec review

Inputs: the spec, the design, the changes in the working tree.

Does the change do what the spec and the design say, and nothing else? Check
that every requirement has code and, where it applies, a test behind it; that
every design decision is followed; that the agreed glossary changes are
made; and that nothing was added that neither asked for.

## Code review

Inputs: the spec, the design, the project's coding standards if any, the
changes in the working tree.

Is the changed code correct, secure, maintainable, and consistent with the
project? Look for bugs, unhandled failure and boundary cases, missing or weak
tests, security problems, and code that ignores the project's existing
patterns or components.

Check the change against every coding standard that applies to it, and name
the rule in each finding against one. A documented standard overrides general
taste: do not report code that follows a standard, even when you would write
it differently.

## Area review

Inputs: as for the code review, plus the design's section on the area.

Used when the change touches an area the design must address: security and
access control, data, or an external contract. Check only that area, in
depth: who can do what, what happens to existing data and how to roll back,
or what other systems relying on the contract will see.

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

Inputs: the findings, the changes in the working tree, read access to the
code.

For each finding, check it against the code and answer: confirmed, not
confirmed, or uncertain, with the evidence. Try to show that the failure in
"what goes wrong" actually happens. A finding you cannot confirm is dropped.
A serious finding that stays uncertain is marked for investigation, not
dropped.
