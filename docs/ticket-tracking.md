# Ticket tracking

A ticket is the durable record of potential and active work. It lets a person
or agent see what needs attention, what happens next, and when the work is
done.

We recommend starting with **GitHub Issues**: it keeps work close to the
repository, and agents can use it directly through the `gh` command-line
tool. A project on another tracker adapts these conventions to it.

## Quick start

Open an agent session in your project's repository and ask:

```text
Set up ticket tracking in this repository following <link to this page>, and wait for my review.
```

## Two states

A ticket is open or closed, and the closure reason tells finished work from
dropped work. Every extra state is one more thing to keep current, so
assignees, labels, and the body carry the rest until a real project shows it
needs more. Open should not mean "keep forever": close dormant tickets that
nobody intends to pursue, so that the open list reflects real intent.

## Issue types

The issue type says what kind of work a ticket is. GitHub's default types,
*Task*, *Bug*, and *Feature*, are enough to start.

## The next action

Every open ticket should make clear what happens next: whether it still needs
a first look, waits for information, needs [specifying](specify.md), or is
ready for [Build](build.md). Without that, people and agents have to read the
whole history to find out, and work stalls between stages unnoticed.

How the next action is marked is the project's choice: a label per action,
board columns, or a tracker's own states. Keep any other labels few, and add
one only when someone will filter by it.

## The body is the front page

Comments keep the discussion, but the next person or agent should not have to
reconstruct the current request from them. The body states where the ticket
stands now, and grows with the work: a small change may need only a few
sentences and concrete completion checks.

## Sub-issues

Work too large for one pull request becomes sub-issues of one parent ticket,
one per end-to-end [slice](specify.md#slicing-happens-here) that Specify
decides. Each slice is then specified, built, and reviewed as its own pull
request, while the parent keeps the overall outcome. Build works on the
sub-issues, never on the parent. Specifying, planning, and building are
activities on the same ticket, not separate tickets.

## Setup

1. Create `docs/ticket-tracking.md`, and fill in the project's issue types and
   how it marks the next action:

```md
# Ticket tracking

Tickets live in GitHub Issues for this repository. Use the `gh` command-line
tool.

## States

A ticket is open or closed. Close it with a reason: *completed*, *not planned*,
or *duplicate*. Do not add other states. Close dormant tickets that nobody
intends to pursue as *not planned*.

## Issue types

Use *Task*, *Bug*, or *Feature*.

<!-- List any other issue types the project uses and when to choose them. -->

## Next action

Every open ticket shows what happens next. When one action finishes, mark the
next one.

<!-- Say how the next action is marked, and list the actions with when each
is done. -->

## Agent rules

Agents suggest the next action and mark it only when a human confirms.

<!-- List any cases agents may decide alone, such as closing exact
duplicates. -->

## Other labels

<!-- List any other labels the project uses and when to apply them. Add a label
only when someone will filter by it. -->

## Ticket body

The body is the current state of the ticket: the problem, the desired outcome,
acceptance conditions, scope, agreed decisions, and open questions. Edit it as
understanding changes; comments keep the discussion, not the result. Keep it
proportional to the work.

## Sub-issues

Work too large for one pull request becomes sub-issues of one parent ticket,
one per end-to-end slice. The parent keeps the outcome, the scope, and the
order of the slices; each sub-issue has its own requirements and acceptance,
and an issue type like any other ticket. The parent is never built itself.
Close it when its sub-issues are closed and its outcome is met. A task list
in the body is enough for small steps that need no owner or status of their
own.
```

2. Add this section to the agent instructions:

```md
## Ticket tracking

Tickets live in GitHub Issues. See `docs/ticket-tracking.md`.
```

For an existing project, build the file from the labels, issue types, and
board the project already uses, and keep its own way of marking the next
action.
