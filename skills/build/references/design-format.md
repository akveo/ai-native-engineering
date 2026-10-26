# Design format

The design lives in `design.md` in Build's working folder. The human approves
it before Build runs on its own. It never goes into the issue. Every design
starts with the summary. The other sections are used only when the work needs
them, in this order:

```md
## Design

**Summary.** The approach in one sentence, and the decisions the reviewer
should look at most closely.

### Decisions

**D1. Short name**

The choice and a one-line reason.

- Rejected: an alternative, and why not.
- Consequence: what this makes easier or harder.

### Current state

- A fact a decision rests on, with a link to the code or, for an external
  fact, to its primary source.
- Assumption: what a decision relies on but could not be confirmed.

### Shape

One diagram of the components, or of before and after, when the change has
structure. Interface and data changes described in words.

### Risks

- A risk, and how the design limits it. Include migration and rollback when
  relevant.

### Not in this design

- Non-goals, and choices left to the plan.

### ADRs

- A decision that meets the criteria for an architecture decision. The file
  ships in this pull request.

### Glossary

- Add **Term**: the proposed definition, for a technical term the design
  introduces. It ships in this pull request.
```

A narrow change may need only the summary, such as "Follows the pattern in
`OrderService.cancel`."

## Write it for the reviewer

The design is read by an engineer who has to approve it. Write for that
person, not for the next agent.

- Put the summary and the decisions first; the facts behind them come after.
- Give one recommendation. Mark guesses as guesses.
- Cite only the facts a decision rests on. No file lists or `file:line`
  inventories.
- No code, pasted schemas, or interfaces. Describe them in words.
- Do not restate the spec, not even in your own words. Say how the approach
  meets a requirement and cite its number: "the account triggers keep R3",
  not what R3 says.
- A list of the rules the code enforces is the spec again: name the
  requirements it keeps, and describe only how it keeps them.
- A decision is a choice between alternatives. Behavior the spec already
  fixes is not a decision: cite the requirement. An existing building block
  with nothing to weigh gets one line, not a decision.
- No empty sections.
- Aim for one to three pages. Length follows the decisions, not the size of
  the spec, so a complex change may need all three; a longer design is
  almost always retelling the spec or doing the plan's work.

Number decisions (D1, D2) so that the plan and the reviews can refer to them.

## Choices left to the plan

Name the choices the plan may make on its own under "Not in this design", such
as naming, internal structure, or the order of tasks. Anything the reviewer
would want to agree on is a decision here, not a choice left to the plan.
