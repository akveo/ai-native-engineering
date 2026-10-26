# Specify

Specify turns a request into a spec: the agreed description of a change and of
how its completion will be judged. The [specify skill](../skills/specify/SKILL.md)
runs it. This guide explains why it exists and why it works the way it does.

## How to use it

[Install the skills](install.md), then run the skill on a raw ticket:

```
/specify 142
```

Start from an idea, and the skill offers to create the ticket once the spec is
ready:

```
/specify Let managers export the team's monthly hours to CSV
```

Or run it with no arguments after discussing a request in the conversation.
The skill first confirms what it takes the request to be:

```
You:   Customers say they can't find old invoices. How does the invoices page work today?
Agent: It lists invoices newest first, 20 per page, with no search or filters.
       Finding an invoice from last year means paging through every invoice since.
You:   Could customers just filter by date?
Agent: Yes. A date range filter would cover that case, and invoices already
       store their issue date.
You:   /specify
```

Specify one request per session. When two requests share a session,
questions and answers about one leak into the other, and the spec ends up with
decisions that were made for something else.

## Why it exists

A request rarely says everything. When something is unclear, an agent does not
stop to ask. It guesses and builds on the guess. The guess then shows up in
review, when changing it means redoing the work.

Specify finds these gaps before any code is written and lets a human settle
them. A few minutes of decisions at this point save hours of rework later.

The result is a spec that [Build](build.md) designs from and its reviewers
check against. Anything the spec leaves open, Build has to guess.

## The agent investigates, the human decides

The human's time should go to decisions, not to facts. So before asking
anything, the agent reads the code, the
[glossary](project-context.md#domain-language), the
[architecture decisions](project-context.md#architecture-decisions), and
related tickets, and never asks what they can answer. It brings the human how
the system behaves today and a recommended answer for every open choice.

Every product and scope choice still goes to the human, however small. An
obvious choice settled quietly by the agent, even as an assumption, is a
decision nobody made.

## What, not how

The spec is written for the person who owns the outcome, often someone who does
not read code, so it describes what users see and do in the glossary's terms.

Keeping the approach out matters more than it looks. A technical proposal
invites a technical discussion, and a workaround agreed there can quietly
change product behavior. So when the human raises how something should be
built, the agent asks about the product concern behind it and leaves the
approach to Build, where an engineer decides it. Technical work, such as a
refactoring, is the exception: its outcome is technical, so its spec is too.

## Questions in rounds

Decisions depend on each other: an answer opens new questions and closes
others. One question at a time is slow, and everything at once means guessing
at answers not yet given. So the agent asks every question that can be answered
now, then works out what the answers opened. Clear work needs no rounds at all,
and rounds that keep coming suggest the request is too big.

## Slicing happens here

A pull request should be reviewable in one sitting, so large work is split.
Which slices exist and in what order is a product decision, so it is made here
with the human, not later in Build.

Slices are vertical: each delivers behavior a user can see and check. Slicing
by layer, such as data first and screens later, delivers nothing checkable
until the last layer lands. The slice that delivers value soonest comes first.

## The spec lives in the ticket

The ticket is the only handoff between Specify and Build, so the spec is the
ticket's body, not a separate document. The draft goes in only once the rounds
end, so the human never reads content that is about to change, and reviews the
spec as it will be read.

## What a spec holds

A spec states the problem, the desired outcome, the requirements with
acceptance examples, and the scope. Requirements are numbered so the design,
tests, and reviews can refer to them. Each is a rule made concrete with
examples, because examples alone leave the rule to guesswork. Scope names what
stays the same as well as what changes, which prevents many regressions. The
spec says what must be true and leaves how to test it to Build. The skill's
[spec format](../skills/specify/references/spec-format.md) has the template.

## Agreement is explicit

Build and its reviewers trust the spec, so it must be something the human
actually agreed to. Specifying ends with that agreement to the ticket as it
reads, and the agent suggests the
[next action](ticket-tracking.md#the-next-action), usually Build. It can also
end without a spec: with an investigation, a narrower request, or a decision to
drop the work.

For the same reason, the spec is never adjusted quietly. If Build finds that
the outcome or the scope must change, the work returns to specifying.
