# Plan format

The plan lives in `plan.md` in Build's working folder. Nobody reviews it but
the plan check, and it stays in the folder after Build, so a later session can
see what was done.

```md
# Plan: short name of the work

Spec: where it lives, as a link or a path

## Constraints

- A constraint from the spec or design that every task must respect.
- A coding standard that concerns the tasks, quoted or linked.
- A fact from the design or the codebase that the tasks need, such as the
  commands that run the tests, lint, and typecheck.

## Choices left to Build

- The choice and a short reason.

## Tasks

### T1. Short name

- Implements: R1, D1
- Depends on: none
- Files: the files this task creates or changes
- Change: what changes, in one to three lines
- Check: the command that proves it, and the expected result
- Done: anything a later task or session needs to know

## Log

- Decided: a choice made while building, and a short reason.
```

## Writing tasks

- Make each task as small as it can be while still having a check of its
  own. A task without a check cannot be called done.
- Refer to the spec's requirements (R1) and the design's decisions (D1) by
  number instead of repeating them. The numbers stay in the plan: the code
  a task writes never cites them.
- "Depends on" decides what can run in parallel. Two tasks depend on each
  other when they touch the same files or interfaces, when either changes the
  database schema or the dependencies, when one uses code the other
  regenerates, or when both need the same running service, such as a
  development server or a database.
- Put preparatory refactoring first, then the behavior, then the clean-up.
- No implementation code. Describe the change; the task's subagent writes
  it. When a task's description grows toward the size of its code, cut it
  back.

## Tracking the work

Each task records its own result. Add the `Done:` line only once the task's
check has passed; a task without it is not done. When a small plan error was
handled, change the task to what was actually done and say what changed in
its `Done:` line.

The log holds what belongs to no task. Add to its end, and never rewrite an
entry:

- `Decided:` or `Deferred:` a choice the design left to Build, a review
  finding left unresolved, or a problem noticed but not fixed, with a short
  reason.
- `Review round 1: 3 confirmed, fixed; 1 dropped.` Fixes for confirmed
  findings are added as new tasks, numbered on from the last one (T4, T5),
  so they do not clash with requirement numbers.
- `Stopped at T4:` the reason, when Build stops.
- `Finished.` once the final checks pass.
