---
name: file-itr-india
description: >-
  Prepares and e-files an Indian income tax return (ITR-1/2/3/4) — tying every
  rupee to a source document, computing both regimes to pick the cheaper, driving
  the e-filing portal, and clearing validation to zero defects. Use when the user
  mentions ITR, Form 16, 26AS, AIS, old vs new regime, 115BAC, 80C/80D/HRA,
  44ADA/44AD presumptive income, capital gains on Indian shares or mutual funds,
  crypto or VDA, foreign assets or Schedule FA, or self-assessment tax and
  234A/234B/234C interest. India personal income tax only — not GST, TDS returns,
  company returns, or non-Indian tax.
---

# File an Indian income tax return

Every rupee **ties out** to a source document. Both regimes get computed before
one is chosen. The portal reaches zero **defects** before anything is submitted.

The aim is the lowest *legal* tax: claim everything the taxpayer genuinely has,
and leave out anything they cannot substantiate.

Rules change every assessment year. Confirm the current year's slabs, rates and
thresholds against incometax.gov.in before relying on any figure here, and tell
the taxpayer the figures are theirs to stand behind.

Track the run with a task list — this spans hours and several sessions, and the
portal will log you out in the middle of it.

## The handoff

Three acts belong to the taxpayer. Do the work up to each, state exactly what is
needed, then hand over.

| Act | What you supply |
|---|---|
| **Log in** | Ask them to reach the dashboard and say when they are there |
| **Pay** | The exact amount, Minor Head **300 — Self-Assessment Tax**, and the AY |
| **Submit and e-verify** | The preview reconciled to your own computation, and the **30-day** e-verification deadline — missing it voids the filing entirely |

Their password, OTP, EVC, card and bank credentials stay theirs to type.

Recommending products ("buy this policy and save tax") sits outside this skill —
lay out the factual options and let them choose.

## Steps

### 1. Fix the person and the year

AY = FY + 1 (FY 2025-26 → AY 2026-27). Establish residential status, age band,
and every income source they can name.

Surface the due date now. Filing late costs 234A interest, and a filer with
business income who misses it without Form 10-IEA is locked into the new regime.

**Done when** the AY, residential status, age band and due date are all fixed.

### 2. Gather

Ask for all of it up front:

- **Form 16** from every employer
- **Form 26AS** and **both AIS and TIS**
- A statement for **every** bank account, including ones opened mid-year
- A **tax P&L** from every broker, and a statement from every platform that paid them
- Prior-year ITR, if they have carried-forward losses

Then ask what deductions they hold — people overpay because nobody asked. Run the
categories aloud: **EPF/PPF/ELSS/LIC/tuition/home-loan principal** (80C), **NPS**
(80CCD), **health insurance** (80D), **home-loan interest** (24b), **rent and
landlord PAN** (HRA), **education-loan interest** (80E), **donations** (80G),
**savings interest** (80TTA/TTB), **disability or specified-illness certificates**,
**EV loan** (80EEB).

→ `references/deductions-old-regime.md` — limits, and the proof each needs

If any bank account is missing from their e-filing profile, have them add and
**pre-validate** it now — it needs their OTP, and the return can only list
accounts already validated in the profile.

**Done when** every document above is in hand and every deduction category has an
answer, including "none".

### 3. Tie out

One number per income head, each traced to a document, and every credit in every
statement placed in exactly one bucket — a named income head, or a named
not-income reason (own-account transfer, gift from a relative, loan, refund,
capital returning from a broker).

→ `references/reconciliation.md` — the method, and where AIS has **blind spots**

The tie-out is what tells you which heads exist. Load each one's reference before
entering a figure under it:

| Head the tie-out surfaced | Read |
|---|---|
| Shares, mutual funds or property sold | `references/capital-gains.md` |
| Freelance, creator, consulting or business receipts | `references/presumptive-business.md` |
| Any asset outside India — foreign shares, ESOPs, accounts | `references/foreign-assets.md` |
| Crypto or NFTs | `references/virtual-digital-assets.md` |

**Done when** no credit anywhere is left unclassified, and every head found has
its reference loaded.

### 4. Choose the form

Three facts bar **ITR-1 and ITR-4 outright**, whatever the income looks like.
Check each explicitly:

- director in a company at any time during the year
- held **unlisted equity shares** at any time — including shares in a *foreign*
  unlisted company
- holds any **foreign asset**

Then pick the simplest form that fits:

| Form | Fits |
|---|---|
| **ITR-1** | Resident, total income ≤ ₹50L, salary + one house property + other sources |
| **ITR-2** | Salary, capital gains, several properties, foreign assets — no business income |
| **ITR-4** | Resident presumptive (44AD/44ADA/44AE), ≤ ₹50L, no capital gains |
| **ITR-3** | Business or professional income that ITR-4 cannot take — e.g. presumptive *plus* capital gains |

Salaried plus creator income plus a few share sales is **ITR-3**: capital gains
rule out ITR-4. Salaried plus share sales, no business, is **ITR-2**.

**Done when** the form is named and all three disqualifiers checked against the facts.

### 5. Compute both regimes

Compute total income and tax under **both** regimes in a script, from the same
tied-out figures, before the portal computes anything. You want an independent
number to catch the portal, not the reverse.

Show the taxpayer both totals with the old-regime deductions listed, so they can
see what they would need to actually hold.

→ `references/regimes.md` — slabs, rebate, the ₹12L **cliff**, surcharge, cess,
Form 10-IEA, and a script skeleton

**Done when** both totals exist from one script run and the taxpayer has seen them.

### 6. Fill the portal

Fill schedule by schedule, confirming each.

→ `references/portal-workflow.md` — the flow, what un-confirms, and the traps
→ `references/portal-automation.md` — if driving the portal with a browser tool

**Done when** every schedule is confirmed and every figure entered traces to a
line in the tie-out.

### 7. Clear both validation passes

There are **two**, and clearing the first does not clear the second:

1. **Internal Validation** — fires on *Proceed To Validation*
2. **Upload Level Validation** — fires after, with additional rules

Each reports "Category of Defect A" errors that block upload. Fix, re-run, repeat.

**Done when** both passes return **zero defects**.

### 8. Verify, then hand off

Read the preview PDF (or the downloadable JSON) and match **every** figure against
your own computation: income per head, gross tax, rebate, cess, special-rate tax,
234A/B/C interest, taxes paid, and amount payable. They should agree to the rupee,
allowing the portal's nearest-₹10 rounding under s.288B.

Then hand off per **the handoff** above.

**Done when** every preview figure has been matched, and the taxpayer holds the
amount, the head, and the e-verification deadline.
