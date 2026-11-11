# Design format

The design lives in `design.md` in Build's working folder. It exists so that
an engineer can review the approach and approve it before Build runs on its
own. That engineer is its only reader: every line must help them decide
whether to approve. What only the later agents need, such as facts, file
lists, and commands, goes into the plan.

Every design starts with the summary. The other sections are used only when
the work needs them, in this order:

```md
## Design

**Summary.** The approach in a few sentences.

### Decisions

**D1. Short name** (ADR)

The choice and its reason, with the fact it rests on and its source.

- Alternative: another option, and what it would cost.

### Areas

- Security and access control: one line.
- Data: one line.

### Risks

- Assumption: what a decision relies on but could not be confirmed.
- A risk, and how the design limits it.

### Shape

One diagram of the components, or of before and after.
```

A narrow change may need only the summary, such as "Follows the pattern in
`OrderService.cancel`."

## Sections

- **Decisions:** only the choices the engineer would want to agree on, the
  most important first. A decision is a choice between alternatives: behavior
  the spec already fixes and a pattern that simply applies are not
  decisions. A choice the engineer must settle is written as a
  recommendation: the choice and why, then each alternative and what it
  would cost, so the engineer can take it or pick another. A choice the
  evidence settled is shorter: the choice and its reason, with an
  alternative only when one was seriously considered. Do not label which is
  which: the order and the alternatives show it. Mark a decision that needs
  an ADR: chosen over a real alternative, costly to change later, and not
  something a future reader could work out from the code. Build writes each
  marked decision as an ADR file in this change, so approving the design
  approves the ADR. Number decisions (D1, D2) so that the plan and the
  reviews can refer to them.
- **Areas:** one line for each area below that the change touches, however
  small the change, because risk here does not follow size. An area the
  change does not touch is left out.
- **Risks:** assumptions first, because they are where the design is most
  likely to be wrong.
- **Shape:** only when the change has a structure that a diagram makes
  clearer than words.

## Areas

Some choices are expensive to get wrong, hard to undo, and hard to spot in a
diff. When the change touches one of these areas, its line says:

- **Security and access control:** who can do what, and what changes, or
  "no change" when the change touches the area without changing access.
- **Data:** schema changes, migrations, and deleting or transforming data,
  and how to roll back.
- **External contracts:** what public APIs, integrations, and other systems
  that depend on this one will see, and whether it stays compatible.
- **Building blocks:** the existing components, helpers, and services the
  change reuses, and any new dependency, pattern, or component, with the
  reason the existing ones do not fit.

The project's agent instructions may add areas to this list.

## Write it for the engineer

- Mark guesses as guesses.
- Cite a fact only where a decision rests on it, next to that decision. No
  file lists or `file:line` inventories.
- No code, pasted schemas, or interfaces. Describe them in words.
- Do not restate the spec, not even in your own words. Say how the approach
  meets a requirement and cite its number: "the account triggers keep R3",
  not what R3 says.
- No product answers and no changes to the spec. They are added to the spec
  when the human answers, so the design only cites them.
- Nothing about choices left to the plan, such as naming, internal
  structure, or the order of tasks.
- No empty sections.
