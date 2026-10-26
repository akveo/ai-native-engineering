---
name: specify
description: Turns an issue, a described idea, or the current conversation into an agreed product spec in the issue — the desired outcome, numbered requirements with acceptance examples, and scope — through codebase investigation and rounds of questions to the human, sliced into end-to-end sub-issues when it is too big for one pull request. Use before Build, for work whose product side is not settled.
argument-hint: "[issue number | idea description]"
disable-model-invocation: true
---

# Specify

Turn a request into a spec: the agreed description of a change and of how its
completion will be judged. The spec is what Build designs from and what its
reviewers check against, so every gap left here becomes a guess later.

Specify settles the product in the issue; Build then agrees the approach with
an engineer and builds it.

This is where human judgment has the most leverage. Do the legwork,
investigating, drafting, and spotting gaps, and leave the decisions to the
human. The depth follows the uncertainty of the work: a small change may need
no questions, a larger one as many rounds as it takes to leave nothing
assumed.

Specify decides what the change is, not how it is built; that is Build's
job. Write the spec and every message for the person who owns the outcome,
usually someone on the product side who does not read code: in the glossary's
terms, about what users see and do, without file, function, table, or column
names. Technical work, such as a refactoring, is the exception:
its outcome is technical, so the spec is too.

Keep every message to the human short; they read all of it. Do not paste the
spec into the chat: link to the issue, and the human reads it there.

## Input

`$ARGUMENTS` is one of:

- **An issue number or URL.** Read the issue, including its comments and
  linked issues. This is the usual case.
- **A description** of an idea, feature, or bug. That is the starting request.
- **Nothing.** The request is what has been discussed in this conversation.
  Summarize what you take the request to be and confirm it before going on.

Specify one request per session. If this conversation already holds another
request, or several, ask which one to specify, and suggest a fresh session for
the others: answers given for one request leak into the spec of another.

A rough idea is a valid starting point. Turning it into a spec is the work
below; do not reject it for being incomplete, and do not fill its gaps with
guesses.

## Where the work lives

There is no working folder. While the rounds go on, the work lives in this
conversation. Once they end, the draft goes straight into the issue, and the
human reviews it there, as it will be read. Build investigates the code
itself, so nothing else needs to outlast this session. If the session is cut
off before the draft is written, run the skill again on the issue.

## 1. Investigate before asking

The human's attention is for decisions, not for facts you can find. Before
asking anything, read the relevant code, the glossary and the architecture
decisions that concern the area if the project has them, project documentation,
related tickets, and history such as blame and linked pull requests. Use the
glossary's terms throughout. Never ask the human something the code can answer;
it spends their time on the part of the work you can do yourself.

Keep the investigation bounded to what the request needs. Exploring the whole
system is not specifying.

When you find a problem outside the request, such as a separate bug, note it
and keep it out of this spec; widening the scope quietly is how specs grow
unreviewable. Offer to open a separate issue for it at the end.

## 2. Present your understanding

Tell the human how the system behaves today, described as users see it, so they
can correct a misreading before the questions build on it. Give code evidence,
such as file and line, only when the human asks for it. Label existing behavior
as context, not as a new requirement. When you rely on something you could not
check, such as an external service's behavior, mark it as an assumption and say
who could confirm it.

Show only the facts the questions rest on. The first round of questions
follows in the same message. When the investigation leaves nothing open, write
the spec straight away, check it, and ask for agreement in that first
message.

## 3. Ask in rounds

Ask about every product or scope choice the request leaves open, however small
it is: what the intent is when it is unclear, what wins when the request
conflicts with the code, a decision, or another requirement, and which
alternative to take in goals, scope, or trade-offs. These are the human's
decisions. Do not ask what the evidence settles, such as existing behavior,
or how the change will be built: that is Build's.

Answers change the questions: one answer raises new ones and makes others moot.
So ask in rounds. Each round asks every open question that you can already word
and give options for. A question waits for a later round when the answer to
another open question could change its wording or options, or make it
unnecessary.

Number each question, give it a short title, and put your recommendation on a
line of its own. Phrase it as a question that proposes the recommendation, so
the human can take it with a plain "yes". Use the terms of what users see,
without code names or how data is loaded. For a real choice, name the
alternatives and what each would change. Put the questions that matter most
first. When the evidence supports no recommendation, say so.

> **Q1. Shipped orders.** Should lines follow the same rule as whole orders,
> so a line cannot be cancelled once the order has shipped? A shipped order
> cannot be cancelled today.
>
> → Yes.
>
> **Q2. Refund timing.** Should a cancelled line be refunded right away? The
> request says "credit", which could also mean a credit on the next invoice;
> that would need changes to invoicing.
>
> → Yes, right away. A credit can be a separate request.

The human can reply to each question by its number. A reply such as "all
good except 2" accepts the rest.

After each round, work out what the answers opened. An answer that widens or
moves the scope usually raises new questions: who sees what, edge cases, and
what happens to the behavior around it. Ask them as the next round. A product
choice you settle on your own, even as an assumption or a proposal, is a
decision the human never made.

The rounds end when every product or scope choice has the human's answer or,
when nobody present can decide it, is recorded as an open question. Many
questions are fine; many rounds are not. If the rounds keep coming, the request
is probably too big: propose slicing it. When nobody present can decide a
question, note it as an open question for the spec, say whether it blocks
implementation, and move on.

When a question needs a fact you do not have yet, find it yourself rather than
asking. When the lookup takes a while, a subagent can run it while you go on
with the questions that do not need the fact. For an external fact, such as
what a partner service allows, use a primary source, such as its official
documentation, and put the link under References in the spec. What you could
not confirm stays an assumption.

### Slices

Once the rounds have settled the scope, judge whether the change fits one
pull request: a reviewer should be able to review it in one sitting. Judge it
from the investigation, not from a technical design; Build tells the human if
a slice still turns out too big. When it does not fit, or the request holds
several outcomes that are useful on their own, ask how to slice it as one more
round, before writing the spec.

Slice into vertical slices. Each slice is end to end: it delivers behavior a
user can see and check, on its own, with its own acceptance. Never slice by
layer, such as the data first and the screens later: a layer delivers nothing a
user can check.

Put the slice that delivers value soonest first, usually the simplest
complete path, and add the rest as increments. Which slices exist and in what
order is a product decision: ask it as a question in the rounds, with your
recommended slicing. If a request holds outcomes that do not depend on each
other at all, they are separate issues rather than slices.

### Who decides

Product and scope decisions belong to the human. If the human is not
available, they wait; do not answer them on the human's behalf.

How the change is built is decided in Build, not here. Do not propose an
approach in the spec: a proposal invites a technical discussion that can
quietly reshape the product. An assumption is never the place for a product
choice either: that is a question.

When the human raises how something should be built, say that Build will
agree the approach with them, and ask about the product concern behind it, if
there is one, such as a list growing too long. When the human states a
technical constraint, write it as behavior users can observe where you can,
such as "turning the filter on or off is instant"; otherwise suggest raising
it when the build starts.

### Terms

When a term conflicts with the glossary or is vague, say so and propose a
precise one. Once a term is settled, put the glossary change in the spec's
Glossary section, so the human agrees to it with the spec. Do not edit the
glossary itself: it changes in the pull request that brings the behavior the
term describes, whoever builds it; the build skill does this itself.

## 4. Write the spec

Read [references/spec-format.md](references/spec-format.md) for the sections,
how to write requirements, and what each kind of work needs. Keep the spec
proportional: a small change fits on one screen, and every section must
resolve real ambiguity.

Write the spec once the rounds have ended. Writing it earlier means rewriting
it after every round, and makes the human read content that is about to
change. Write each paragraph and each list item on one line: the tracker shows
every line break, so a hard-wrapped spec reads as broken lines.

Mark assumptions and open questions as such in the spec. An assumption stays
an assumption until the human confirms it; not objecting is not confirming.

## 5. Check the draft

Before asking for agreement, check the spec for:

- a requirement that could be read in two ways;
- contradictions between requirements or with the code;
- missing failure and boundary cases;
- acceptance that cannot be verified;
- a product choice the human never answered, or an assumption presented as a
  requirement;
- a spec that only restates the request;
- filler, empty sections, and user-story lists where a few requirements would
  do;
- examples that state the obvious, and existing behavior the change does not
  put at risk;
- a limit or rule you added while writing that no answer settled, such as
  "only for a holiday today". It is a product choice, even when it seems to
  follow from an answer;
- file, function, table, or column names, an approach to building the change,
  and invented metrics.

Fix what matters and leave the rest. A search for gaps always finds some, and
gold-plating a spec costs as much as leaving a real gap.

Writing the spec often exposes a gap the rounds missed. When it is a product or
scope choice, the rounds are not over: ask it as a new round, in the same
format, and ask for agreement only after it is answered. Do not present it as
done, as in "I added two limits".

## 6. Agree

Put the draft into the issue: the first line of the spec is the title, the
rest is the body.

- **Existing issue:** set the title and replace the body with the draft. Keep
  the reporter's original text at the end, under "Original request", only when
  it holds something the spec does not, such as screenshots, links, or wording
  that matters. Otherwise drop it; the issue's history still has it.
- **No issue yet:** ask whether to create the issue with the draft, with the
  right issue type, and create it on confirmation. If the human declines, the
  spec stays in this conversation.

Link to the issue and ask for explicit agreement. A reply agrees to the issue
as it read when you asked. The human may edit the issue directly while
reviewing, so read it again before every change and keep their edits:

- After corrections, work out what they open, as after any round's answers.
  When they leave a product or scope choice open, the rounds start again:
  ask first, round after round, and leave the issue as it is. Update the
  issue once, after the last round, check it as in step 5, and say in a line
  or two what changed. Corrections that open nothing go in straight away.
- After agreement, do not change the spec without telling the human.

The spec is ready when Build can design the approach without guessing about a
material requirement. Questions that would change the expected result or the
scope must be resolved; routine implementation choices can stay open.

Specifying can also end without a spec. These are valid results too:

- an investigation, when a question needs more than specifying can give;
- a narrower or sliced request;
- a decision to defer or drop the work.

## 7. Finish

1. **Sliced work:** the issue becomes the parent, and each slice becomes a
   sub-issue, as the spec format describes. Create the sub-issues on
   confirmation, with the order set as dependencies in the tracker.
2. **Suggest the next action** and mark it on confirmation, as the project's
   ticket tracking describes. If the project has no ticket tracking file,
   only name the next action in the summary:
   - Build when the product is settled; Build agrees the approach with an
     engineer before it builds. For sliced work, each slice is marked for
     Build;
   - an investigation when an open question needs research first.

   For a deferred or dropped request, suggest closing the issue as *not
   planned* with the reason.
3. **Offer separate issues** for problems found outside the request, and
   create them on confirmation.
4. **End with a short summary:** the issue link and the next action, then
   only what exists of: sub-issues, open questions, assumptions, glossary
   changes, and separate issues. Leave out empty items rather than writing
   "none".
