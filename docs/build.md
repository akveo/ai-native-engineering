# Build

Build turns an agreed spec into a reviewed change, ready for one pull request.
The [build skill](../skills/build/SKILL.md) runs it. This guide explains why
it exists and why it works the way it does.

## How to use it

[Install the skills](install.md), then run the skill on a spec in a fresh
session. Build does not care where the spec lives: point it at a ticket, a
file, or a link, or paste the spec into the chat:

```
/build 142
```

The spec is usually one agreed in [Specify](specify.md). A small, obvious
change can skip Specify and only state the outcome and its check, such as
"Exporting an empty report no longer fails; a regression test covers it". Or
just describe the change. The skill confirms the outcome and its check before
going on:

```
/build Fix the date picker's default date
```

Start each build in a fresh session. Earlier discussion in the same session
distracts the agent and can carry choices nobody agreed to. Build should work
only from the spec, or the confirmed description, and the technical design it
agrees with the engineer.

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
pattern, and leaves to the engineer only where reasonable engineers could
differ: a real trade-off, going against an
[architecture decision](project-context.md#architecture-decisions), or a new
dependency. The engineer's time goes to judgement, not to facts.

Those choices are not asked one by one in the chat. They go into the design
as recommendations, each with its alternatives and what they cost, so the
engineer judges them next to the whole approach and settles them all at
approval. The agent asks before writing only when an answer would make a
different design, not just a different line in it.

Investigating is part of the design, not a step before it. An agent reading
code already forms an approach, and the choices it makes along the way would
otherwise never be seen. So the design starts by sketching the approach and
listing every choice it makes, and each choice sends the agent to the
evidence that settles it.

## Product questions go back to the spec

A design that answers product questions makes product decisions nobody agreed
to, and how the code works today never settles how new behavior should look.
So Build first checks that the spec settles what users will see. A small gap
is asked and written into the spec as soon as the human answers, because the
reviewers check against it; the design never carries it. More than a few, or
one that changes the scope, sends the work back to [Specify](specify.md).

## The design names the risky choices

Some choices are expensive to get wrong and hard to spot in review: security
and access, data and migrations, external contracts, and building blocks, that
is, what the change reuses and why anything new is needed. When the change
touches one of them, the design gives it a line of its own, even when the
answer is "no change". A project can add its own.

The design exists so that the engineer can review the approach and approve
it, and that engineer is its only reader. So it holds only what helps them
decide: the approach, the decisions the engineer would want to agree on, the
risky areas, and the assumptions and risks. Facts, file lists, and commands
the later agents need go into the plan instead. The agent that wrote it
rarely cuts its own text and does not see the choices it made without
noticing. So before approval a fresh agent that has not seen the
conversation marks what retells the spec or does the plan's work, and finds
the choices the design leaves silent. The design stays in Build's working
folder, not in the spec, which holds the product.

## Approval is the handoff

One message asks the engineer to approve the design, including the
architecture decisions it marks to write. It warns when the change looks too
big for one pull request, because splitting it is a product decision. Problems
found outside the request are not part of it: they do not change the
approach, so Build records them in its plan and lists them when it finishes,
for the human to follow up. The message ends by making clear that this is
the last point to change the approach, so the engineer knows when they can
leave.

The engineer can change any decision in the reply. Picking an alternative the
design already lists is approval with that choice. A change that other
decisions rest on, or an option the design did not consider, makes Build
rework the design and ask again. When there is nothing to approve, such as a
design that is only a one-line approach, Build says the approach and goes on
without waiting.

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
fixed. Later rounds review only the fixes, not the whole change again,
because fresh reviewers always find something new to say and the rounds
would never settle. Rounds are limited to three, and a serious finding that
keeps coming back points to a problem in the spec or design, so Build stops.

## What Build leaves

Build leaves the change uncommitted in the working tree, for the human to
commit and open the pull request. Its last message says what was done, how
it was verified and what was not, such as "not tried in the running app",
the review findings it could not resolve, and the problems outside the
request it kept out of the change. Everything else stays in Build's working
folder: the design, and the plan with each task's result and a log of what
happened.

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
small fix to the context is made right away, real work is planned, and an
ignored suggestion is dropped. If the problem is real, it will come up again.
