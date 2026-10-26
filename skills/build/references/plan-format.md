# Plan format

The plan lives in `plan.md` in Build's working folder. Nobody reviews it but
the plan check, and it stays in the folder after Build, so a later session can
see what was done.

```md
# Plan: ticket title

Ticket: link, or the confirmed description of a small change

## Constraints

- A constraint from the spec or design that every task must respect.
- A coding standard that concerns the tasks, quoted or linked.
- A fact from the investigation that the tasks need, such as the commands that
  run the tests, lint, and typecheck.

## Choices left to Build

- The choice and a short reason.

## Tasks

### T1. Short name

- Implements: R1, D1
- Depends on: none
- Files: the files this task creates or changes
- Change: what changes, in one to three lines
- Check: the command that proves it, and the expected result

## Progress

- T1: done. Anything the next task or session needs to know.
```

## Writing tasks

- Make each task as small as it can be while still having a check of its
  own. A task without a check cannot be called done.
- Refer to the spec's requirements (R1) and the design's decisions (D1) by
  number instead of repeating them.
- "Depends on" decides what can run in parallel. Two tasks depend on each
  other when they touch the same files or interfaces, when either changes the
  database schema or the dependencies, when one uses code the other
  regenerates, or when both need the same running service, such as a
  development server or a database.
- Put preparatory refactoring first, then the behavior, then the clean-up.
- No implementation code. Describe the change; the task's subagent writes it.

## Progress

Update the progress after every task, and after every review round:

- `T1: done.` with anything a later task or a fresh session would need.
- `T2: changed:` when a small plan error was handled, and how.
- `Decided:` or `Deferred:` a choice the design left to Build, a review
  finding left unresolved, or a problem noticed but not fixed, with a short
  reason.
- `Review round 1: 3 confirmed, fixed; 1 dropped.` Fixes for confirmed
  findings are added as new tasks, numbered on from the last one (T4, T5),
  so they do not clash with requirement numbers.
- `Stopped at T4:` the reason, when Build stops.
- `Finished.` once the final checks pass.
