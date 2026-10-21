# Build

Build turns an agreed spec into a reviewed change, ready for one pull request:
the whole spec, or one [slice](specify.md#slicing-happens-here). The
[build skill](../skills/build/SKILL.md) runs it. This guide explains why it
exists and why it works the way it does.

## How to use it

Copy the [`skills/build`](../skills/build) folder into your agent's skills
folder, such as `.claude/skills/` for Claude Code or `.agents/skills/` for
Codex. Then run the skill on a ticket in a fresh session:

```
/build 142
```

The ticket usually holds a spec agreed in [Specify](specify.md), or one slice
of it. A small, obvious change can skip Specify. Its ticket only needs to state
the outcome and its check, such as "Exporting an empty report no longer fails;
a regression test covers it". Or skip the ticket too and describe the change.
The skill confirms the outcome and its check before going on:

```
/build Fix the date picker's default date
```

Start each build in a fresh session. Earlier discussion in the same session
distracts the agent and can carry choices nobody agreed to. Build should work
only from the ticket, or the confirmed description, and the technical design
it agrees with the engineer.

## Why it exists

Once the spec says what to build, an agent can write the code quickly. Two
things still go wrong. The agent makes technical choices on its own, and the
engineer first sees them in the diff, when changing them means redoing the
work. And the agent that wrote the code checks it too, so it tends to find it
correct.

Build fixes both. An engineer agrees a technical design before any code is
written, and independent agents review the result. In between, the agent works
alone, so the engineer spends time only where their judgement matters.

## Technical design with an engineer, right before building

Build starts with an engineer, not with the person who owns the outcome. The
product is settled in the spec, and what is left is technical, so each part of
the work gets the person who can decide it.

The technical design is made right before the code, in the same session.
Nothing has changed in the code since it was written, so it still fits.

The agent settles what the evidence settles, such as following an existing
pattern, and asks the engineer only where reasonable engineers could differ: a
real trade-off, going against an
[architecture decision](project-context.md#architecture-decisions), or a new
dependency. The engineer's time goes to judgement, not to facts.

## Product questions go back to the spec

A design that answers product questions makes product decisions nobody agreed
to, and how the code works today never settles how new behavior should look.
So Build first checks that the ticket settles what users will see. A small gap
is asked and written into the spec, because the reviewers check against it.
More than a few, or one that changes the scope, sends the work back to
[Specify](specify.md).

## The design names the risky choices

Some choices are expensive to get wrong and hard to spot in review: security
and access, data and migrations, external contracts, and building blocks, that
is, what the change reuses and why anything new is needed. The design
addresses each of them explicitly, even when the answer is "no change". A
project can add its own.

Otherwise the design covers only what the engineer has to agree on, so it is
short. A trivial change in one place gets a one-line approach and goes on
without waiting. The design stays in Build's working folder, not in the
ticket, which holds the product.

## Approval is the handoff

One message asks the engineer to approve the design and everything outside the
code: answers to add to the spec, architecture decisions to write, and
problems to turn into tickets. It ends with "Approve, and I'll run the rest
without you", so the engineer knows when they can leave.

## Why the rest runs without a human

The spec and the approved design are the agreement, so after approval Build
delivers them instead of reopening them. It makes the choices the design left
to it, and stops when a choice belongs to someone else, such as an unclear
requirement or anything irreversible. A stop with a clear reason is a good
result. A guess that reaches review is not: it costs a rewrite and hides a
decision nobody made.

Each task runs in a fresh subagent with only what it needs, so a long build
does not drown in its own history. The main agent runs every check itself,
because a subagent's report is not evidence. When the same failure survives
two fixes, Build tries another approach once, then stops rather than looping.

## Review the result

Reviewers run in fresh, read-only subagents, so they did not write the code
and cannot change it. A spec review asks whether the change does what the spec
and design say, and nothing else. A code review asks whether it is correct,
secure, maintainable, and follows the
[coding standards](project-context.md#coding-standards). Reviewers see the
spec, the design, and the changes, but not the plan, so they judge the result
rather than the intention.

A reviewer asked to find problems usually finds some, even in sound work. So
another subagent confirms each finding against the code before anything is
fixed. Review rounds are limited to three, and a serious finding that keeps
coming back points to a problem in the spec or design, so Build stops.

## Improvement suggestions

A build can show that the setup around the work is getting in the way: a
command or piece of context the agent could not find, an agent instruction
that was wrong, a mistake a check could have caught, or a correction the human
had to repeat. Build then ends with suggestions, each saying what happened,
what to change, and where. Each one comes from a real moment in the work, so
the setup improves where it actually failed, and most builds have none.

For example, tests failed because the database was not running, and the agent
took several attempts to learn that integration tests need
`docker compose up db`. The suggestion: the test script starts the database,
or the agent instructions name the command.

A mistake that should not recur is best caught by a check, such as a lint
rule, a type, a test, or a CI job, because a check never forgets. Only a
judgement call no check could make becomes a
[coding standard](project-context.md#coding-standards). The human decides: a
small fix to the context is made right away, real work becomes a ticket, and an
ignored suggestion is dropped. If the problem is real, it will come up again.
