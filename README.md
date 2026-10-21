# AI-Native Engineering

AI-native engineering means building software with AI agents as part of the
team. The aim is to deliver faster without losing quality, and generating code
faster is not enough for that. Agents need enough context, access, and checks
to do useful work on their own.

People decide what to build and make the important choices. Agents do the work
in between: they investigate, plan, write the code, and review it.

We have tried to put together an essential base of practices and workflows
you can start with. Adapt it to your needs.

## Practices and workflows

### Project context

Gives every agent session what it needs to know about the project: what it
is, the terms it uses, the architecture decisions behind it, and the coding
standards. It starts with one agent instructions file, such as `AGENTS.md` or
`CLAUDE.md`, that links to the rest, and is updated whenever an agent gets
something wrong for lack of context.
[Read more](docs/project-context.md).

### Ticket tracking

Keeps each piece of work in a ticket that always shows the current request and
what happens next, so people and agents can see the state of the work at a
glance. [Read more](docs/ticket-tracking.md).

### Specify

The product side of the work, before any code is written. An agent works with
the person who owns the outcome to settle what should change, for whom, and
how to tell it is done, and writes this into the ticket as a spec.
[Read more](docs/specify.md).

### Build

The technical side of the work. An agent proposes a technical design, and once
an engineer approves it, plans the work, writes the code, and has it reviewed
by independent agents, without a human watching. [Read more](docs/build.md).

### Recurring checks

Catch problems that no single change shows but that build up over time, such
as architecture that drifts or terms that stop matching the code. Agents look
for them regularly, and people decide what to fix.
[Read more](docs/recurring-checks.md).

## Tailor it to your project

As you adopt it, keep the main idea in place: people make the product
decisions and the important technical ones, and agents do the work in between.

There is room to grow, for example in handling incoming requests and alerts,
testing the running system, exploring an unclear goal, and prototyping.

## Resources

The [resources page](docs/resources.md) lists the projects, skills, and techniques
we learned from. Many have useful ideas this guide does not cover, so look
there for something that fits your project, and suggest a resource you found
helpful.

## Contributing

Contributions are welcome. Share a practice that worked for you, improve the
existing guidance, or suggest a useful resource.
