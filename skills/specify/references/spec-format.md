# Spec format

The spec lives in the issue: its first line is the title, the rest is the
body. It says what the change is, not how it is
built, and is written for the person who owns the outcome, who may not read
code: glossary terms and what users see, no file, function, table, or column
names. Technical work is the exception; there the outcome itself is technical.

Keep it proportional to the work: a small change fits on one screen, with a
few sentences and concrete acceptance checks; ambiguous or consequential work
needs more. Remove any section that does not resolve real ambiguity.

Write each paragraph and each list item on one line. The tracker shows every
line break, so hard-wrapped text reads as broken lines.

## Core sections

```md
# Short title naming the outcome in product words

## Problem and context

What happens today, as users experience it, who or what is affected, and
why it is worth changing. Include evidence such as reports or links.

## Desired outcome

The change and what becomes possible or better. For technical work, describe
the engineering outcome directly.

## Requirements and acceptance

### R1. Short requirement name

The behavior or technical property the change must satisfy.

- Given a starting condition, when an action happens, then a result follows.
- An important alternative, failure, or boundary case.

## Scope and boundaries

- Limits on users, surfaces, data, environments, or supported cases.
- Related work that is explicitly excluded.
- Existing behavior or compatibility that must be preserved.
```

## Optional sections

Add these only when the change needs them.

```md
## Glossary

- Add **Term**: the proposed definition.
- Change **Term**: the new definition, and why.

## Open questions and assumptions

- Question: what is still undecided, and whether it blocks implementation.
- Assumption: what we rely on for now, and who can confirm it.

## References

- Related work, designs, research, or decisions.
```

## Writing requirements

State each requirement as a general rule, then make it concrete with examples
beside it. The examples illustrate the rule; they do not replace it.

Give an example only where a reader could get the rule wrong: a boundary, a
duplicate, a failure, an exception. An example that states the obvious, such
as "no holiday and no leave, then not listed", only makes the spec longer.

Use Given/When/Then where it clarifies behavior. For a structural change, a
direct check is clearer, such as "the pricing module has no database
dependency." Add measurable limits, with the conditions they apply under, only
when they are real; do not invent targets.

Number requirements (R1, R2) so that the design, plan, tests, and reviews can
refer to them.

Scope is as much about what stays the same as what changes. Name excluded
work, unsupported cases, and existing behavior that must keep working. This is
where many regressions are prevented. Name only what someone could
reasonably expect to change, not everything that stays the same.

A constraint the human states belongs in the spec as behavior users can
observe, such as "turning the filter on or off is instant". How the change is
built, and how it will be tested, are left to Build; the acceptance examples
are what its tests and reviews check.

The **Glossary** section lists the terms the change adds or redefines, with the
definition the human agrees to. The pull request that brings the behavior
writes them to the glossary; the build skill does this itself.

Use the tracker's own relationships for dependencies, and link supporting
documents without moving essential requirements out of the issue.

## Adjust to the kind of work

| Work | The spec needs | Completion evidence |
| --- | --- | --- |
| Feature | The user's goal and main journeys, rules, failure cases, exclusions | The acceptance examples work, including failures |
| Bug | Reproduction conditions, actual and expected behavior, evidence, behavior to preserve | The original failure is fixed and a regression check catches it |
| Refactoring | The concrete engineering problem, the structural improvement, behavior to preserve | The improvement is visible and preservation checks pass |
| Architecture | The drivers, required properties, constraints, and the decision when it is made | The required properties are demonstrated |

- **Bug:** a report does not need a known root cause. Keep suspected causes
  separate from observed facts; a cause is a hypothesis until evidence
  supports it.
- **Refactoring:** name a tangible improvement, such as "isolate pricing rules
  from database access." "Make the architecture cleaner" cannot be judged.
  Passing the existing tests may not prove preservation on its own.
- **Architecture:** separate choosing an approach from implementing it. The
  choice may be an ADR; the implementation is a change with its own spec.
- **Technical work in general:** do not invent user stories. Describe the
  engineering outcome directly.

## Sliced work

Write the draft as one spec, with requirements numbered across all slices,
and add a section that maps them to slices:

```md
## Slices

1. **Short name:** what the user can do after this slice. R1, R2.
2. **Short name:** what this slice adds. R3. After 1.
```

On agreement, the spec is divided:

- **The parent issue** keeps the title, the problem, the desired outcome, the
  scope, and the slices with links to the sub-issues. It is done when its
  outcome holds, not just when every slice is closed.
- **Each slice** becomes a sub-issue whose body is its own spec: a title, a
  line linking the parent for context, and its requirements with their
  acceptance, numbered as in the full spec. Add slice-specific scope when it
  matters, such as what a later slice will add. Build reads the slice as its
  spec and the parent only for context.
