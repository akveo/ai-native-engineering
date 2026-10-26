# Architecture report format

Save the report as `docs/reports/architecture-check/YYYY-MM-DD-<area>.md`,
for example `2026-10-16-src-orders.md` for `src/orders/`.

Write the report for an engineer who knows the codebase. Keep each candidate
short enough to decide on from the report alone, and link to files rather
than quoting long code.

````md
# Architecture report

- **Run:** 2026-10-16, commit `abc1234`
- **Area:** `src/orders/`
- **Why this area:** never checked; 38 changes since the last run

## Possible defects

- Refunds round the total half-down, checkout rounds it half-up, so a full
  refund can differ from the charge by a cent: `src/orders/refund.ts:118`,
  `src/orders/checkout.ts:44`. Product question: which rounding is intended?

## Candidates

### 1. Order totals are computed in three places

**Impact:** High

**Where:** `src/orders/checkout.ts:40`, `src/orders/refund.ts:112`,
`src/admin/order-view.ts:27`

**Problem:** Each place recomputes totals with its own rounding and discount
rules. The last two discount changes had to edit all three, and the rounding
has already drifted (see possible defects).

**Change:** One module owns order totals; the three places call it.

**What gets easier:** A pricing rule changes in one place, and its tests
cover every caller.

**Cost:** Three call sites and their tests. The rounding defect must be
settled first, so the move does not pick a rule by accident.

### 2. ...

## Since the last report

- Payment retries spread across two services: addressed in `abc0001`.
- Refund state machine with an unused state: still there, repeated as
  candidate 3.

## Assumptions and open questions

- `src/orders/legacy/` looks unused, but nothing proves it; left out.

## Coverage

| Area | Last checked | Commit |
| --- | --- | --- |
| `src/orders/` | 2026-10-16 | `abc1234` |
| `src/catalog/` | 2026-09-14 | `9f8e7d6` |
| `src/admin/` | never | |
````

## Rules

- **Run:** the date and the commit the run looked at.
- **Area:** the area's path or name, with a short note only when the run
  took part of it. Do not list its members.
- **Why this area:** the argument that named it, or the reason the check
  chose it.
- **Possible defects:** code that behaves wrongly today, one item each, most
  harmful first, with no introduction: what goes wrong in one or two
  sentences, the code that shows it, and the product question that decides
  the fix, if any. List every defect you found; keep each one short. Mark one from an earlier report as
  repeated. Not capped,
  not counted toward the candidates. Leave the section out when there are
  none.
- **Candidates:** at most five, most valuable first. Mark a candidate from an
  earlier report as repeated. With no candidates, write "No candidates in
  this area." and keep the other sections.
- **Since the last report:** the possible defects and candidates from this
  area's previous report. Leave the section out when the area had none.
- **Assumptions and open questions:** what you assumed about the code where
  something was unclear, and what you could not confirm from the repository.
  Product questions go with the defect or candidate they decide. Leave the
  section out when empty.
- **Coverage:** every area, the date it was last checked, and that run's
  commit. When a run read only part of an area, list that part and the rest
  as separate rows. Carry forward the rows of areas not checked this run from the
  newest report.
