# Project context

An agent can only work with what it knows. The project context gives every
session what it needs: what the project is, which terms, decisions, and
coding standards apply, where the work is [tracked](ticket-tracking.md), and
what the agent may touch.

The aim is the right context, not the most. Each part starts small, or not at
all, and grows from real work. Keep it up to date: a context that no longer
matches the project misleads agents.

## Quick start

Open an agent session in your project's repository and ask:

```text
Set up the project context in this repository following <link to this page>. Go part by part, and wait for my review after each one.
```

## Agent instructions

The agent instructions file, such as `AGENTS.md` or `CLAUDE.md` at the
repository root, is the entry point: agents load it at the start of every
session. If the project needs both file names, keep the content in one and
make the other point to it, such as a `CLAUDE.md` that contains only
`@AGENTS.md`.

Keep it to what every session needs. It holds the project overview and the
environment in full, and for each other part a short section that says where
it lives and when to read it:

- [Project overview](#project-overview)
- [Domain language](#domain-language)
- [Architecture decisions](#architecture-decisions)
- [Coding standards](#coding-standards)
- [Ticket tracking](ticket-tracking.md)
- [Environment](#environment)

## Project overview

The agent instructions open with a few sentences on what the product is, who
uses it, and its main parts. Keep them general: a list of features or screens
goes stale, and an agent finds those in the code when a task needs them.

```md
# Project overview

Shop is an online store. Customers browse products and place orders on the
storefront, staff manage the catalog and orders in the admin panel, and
partners sync stock through a public API.
```

## Domain language

A shared vocabulary makes people, agents, tickets, and code mean the same
thing by the same word. Without it, agents drift between synonyms, and a vague
term such as "account" can hide two different concepts until the
implementation is already wrong.

The vocabulary lives in a **glossary**, `GLOSSARY.md` at the repository root.
It opens with the rules for changing it, and the terms grow with the work, not
in advance: [Specify](specify.md) and [Build](build.md) flag vague or
conflicting terms and settle them with the human, and the glossary changes in
the pull request that ships the behavior, so it never describes something that
does not exist yet. A project with little domain complexity can do without one
until it needs it.

### Setup

1. Create `GLOSSARY.md` at the repository root. Its opening lines are the
   rules for the glossary; terms go below them, grouped under headings as
   natural clusters appear:

````md
# Glossary

Terms that mean something specific in this project.

**What belongs here.** A term belongs when a new engineer would need it
defined to follow a conversation, a ticket, or the code, and when it is a
concept in its own right. Keep out general programming concepts,
implementation details such as paths, class names, tables, and libraries,
configuration values, and specs, plans, or decisions.

**Format.** Each term looks like this:

```md
**Order**:
A customer's request to buy one or more products.
_Avoid_: Purchase, transaction
```

- Define what the term is in one or two sentences.
- Pick one canonical term and list other words for it under _Avoid_.
- Add an _Alias_ only for a synonym used with exactly the same meaning, such as
  an abbreviation.
- Add a short note only for rules that are not obvious, such as lifecycle or
  ownership.
- Group terms under headings when natural clusters appear.

**Rules.**

- Read the glossary before working in an area, and use its terms in tickets,
  names, tests, and proposals.
- When a term conflicts with the glossary or is vague, say so and propose a
  precise one.
- Record a settled term in the spec or the design; the glossary itself
  changes in the pull request that brings the behavior.
- Remove a term when its concept disappears.
- An agent may propose and edit terms; a human reviews the changes.
````

2. Add this section to the agent instructions:

```md
## Domain language

Use the terms in `GLOSSARY.md`. To add or change a term, follow the rules at
its top.
```

For an existing project, ask an agent to draft a first glossary from the
schema, the main types, and the docs, and review it before it goes in.

## Architecture decisions

A consequential decision needs a record that it was made deliberately and why.
Without one, a reasonable reader may "fix" something intentional or reopen a
trade-off that was already settled.

Decisions are kept as **architecture decision records (ADRs)**, short files
in `docs/adr/`. Few decisions need one: only those that are hard to reverse,
surprising without context, and the result of a real trade-off, such as the
shape of the system, technology with real lock-in, deliberate exclusions, or
constraints the code does not show. Build drafts an ADR while agreeing the
approach with the engineer, and it ships in the pull request that brings the
decision.

### Setup

1. Create `docs/adr/README.md`:

````md
# Architecture decisions

This folder holds the project's architecture decision records (ADRs). Each one
records a decision that was made deliberately, and why.

## When to record a decision

Record a decision only when all three are true:

1. It is hard to reverse.
2. It would surprise a future reader without context.
3. It was the result of a real trade-off between genuine alternatives.

Otherwise, keep the decision in the ticket or the code.

## Format

Name the file with the next number and a short slug: `0001-short-slug.md`. To
pick the number, take the highest existing number and add one.

```md
# Short title of the decision

What the context is, what we decided, and why.
```

Add a **Considered options** section when the rejected alternatives are worth
remembering, and a **Consequences** section when the effects are not obvious.

## Rules

- Read the ADRs that concern an area before changing it. If your work
  contradicts one, say so; do not override it silently.
- A human agrees to every decision. An agent may draft the ADR.
- Ship the ADR in the pull request that brings the decision.
- Do not rewrite an ADR to say something different. Write a new one that names
  the one it replaces, and add a line to the old one that links to the new
  one.
````

2. Add this section to the agent instructions:

```md
## Architecture decisions

Decisions live in `docs/adr/`. Read the ones that concern the area you change.
If your work contradicts one, say so before going further. To record or change
a decision, follow `docs/adr/README.md`.
```

## Coding standards

Coding standards hold every review to the same bar. Without them, each review
falls back on general taste, the same correction is made by hand pull request
after pull request, and agents repeat the mistake in every session.

They hold only judgement calls no tool can make, such as where a kind of logic
belongs, when to reuse a project building block, or what a test for a kind of
change must cover. Anything a linter, type, test, or CI job can enforce
becomes that check instead: a check fails every time, a sentence only when
someone remembers it.

The standards are written for reviewers, who receive the change and little
else and have room to apply every rule. In [Build](build.md), the code review
checks every change against them, and the plan passes a rule on to the task it
concerns. Human reviewers use the same file, and it wins over their taste
until someone changes it. Rules grow from corrections that repeat, which
[Build's improvement suggestions](build.md#improvement-suggestions) report,
not from a style guide written in advance.

A rule is one sentence a reviewer can check a diff against, with its reason,
for example: "Domain functions return a `Result` and do not throw; callers in
the API layer turn a failed `Result` into a response."

### Setup

1. Create `CODING_STANDARDS.md` at the repository root. Rules are added below
   the opening lines as real corrections reveal them:

````md
# Coding standards

Judgement calls that reviewers check every change against. Anything a tool can
enforce is a lint rule, type, test, or CI check instead, not a line here.

- Write each rule as a sentence a reviewer can check a diff against, with its
  reason. Link the architecture decision it follows from, if any.
- Keep out framework defaults, general good practice, decisions (those are
  ADRs), terms (the glossary), and how the team works.
- Add a rule when the same correction keeps coming back and no check could
  catch it. A human agrees to every rule.
- Remove a rule when it becomes a check or stops applying.
- Group rules under a heading per area, such as errors or UI, once there are
  several.
````

2. Add this section to the agent instructions:

```md
## Coding standards

Reviewers check changes against `CODING_STANDARDS.md`. When a correction keeps
coming back, add a check if a tool could catch it; otherwise propose a rule
there.
```

For an existing project, look for rules the team already applies by hand in
`CONTRIBUTING.md`, a style guide, the agent instructions, or comments that
repeat across recent reviews. Check that each rule still matches the code
before you move it; move what a tool could enforce into checks, and put only
the rest in the file.

## Environment

Agents work with more than the code: they query databases and read logs, and
with the right tools they can deploy or change data. The environment says
which systems the project runs on and what agents may do with each, so that
an agent with access to production does not change it by mistake. It lives in
the agent instructions, because any session may touch these systems.

It covers the infrastructure an agent can reach: hosting, databases, and logs
and error reports. For each system, say how agents reach it, what they may and
may not do, and how a person gets access. Never put credentials in the file.
External services the application calls, such as payments or messaging, and
how to run the project locally belong elsewhere: the first are part of the
product, the second are development instructions.

### Setup

Add this section to the agent instructions, with the project's own systems:

```md
## Environment

- Hosting: AWS, with staging and production environments. Do not deploy or
  change either unless a human asks.
- Database: PostgreSQL. Read-only access to staging through
  `npm run db:staging`; no access to production.
- Logs and errors: Datadog and Sentry, through their MCP servers.
- Access: ask a project admin for an AWS account and a Datadog invite.
```

For an existing project, ask an agent to draft it from the deployment and CI
configs, `.env.example`, and the MCP servers in use, and decide yourself what
agents may do with each system.
