# file-itr-india

An agent skill for preparing and e-filing an Indian income tax return — ITR-1, 2,
3 or 4 — on `eportal.incometax.gov.in`. Works with
[Claude Code](https://claude.com/claude-code) and
[Codex](https://developers.openai.com/codex).

It exists because the hard parts of filing are not the tax rates. They are
reconciling every rupee to a document, choosing the regime by actually computing
both, and surviving a portal that fails silently in a dozen documented ways.

> **Current for FY 2025-26 (AY 2026-27).** Slabs, rates and thresholds move with
> most Budgets, and the portal changes between filing seasons. Every rate table in
> the skill is marked with the year it describes and tells the agent to re-confirm
> before relying on it — but if you are filing for a later year, treat the numbers
> as a starting point and check them against
> [incometax.gov.in](https://www.incometax.gov.in/).

## What it does

- **Ties out every rupee.** Every credit in every statement lands in a named income
  head or a named not-income reason. Leftovers get surfaced, not absorbed.
- **Computes both regimes** from the same figures, in a script, before the portal
  computes anything — so you catch the portal rather than the reverse.
- **Picks the form** on the facts, including the three that bar ITR-1 and ITR-4
  outright and that no income-shape test catches.
- **Drives the portal** schedule by schedule, with a catalogue of the validation
  defects it raises and the fix for each.
- **Verifies the preview** line by line against the independent computation before
  anything is submitted.

## What stays with you

Three acts belong to the taxpayer, and the skill hands each over rather than
performing it:

| Act | Why |
|---|---|
| **Logging in** | Your password and OTP stay yours |
| **Paying** | The skill states the exact amount, head and AY; you pay |
| **Submitting and e-verifying** | The declaration is yours to make, and e-verification has a **30-day** deadline that voids the filing if missed |

It also stays out of product recommendations. "Buy this policy and save tax" is
financial advice; laying out the options factually is not.

## Install

`SKILL.md` plus a `references/` folder is the format both agents read, so it is
the same clone either way — only the destination differs.

**Claude Code**

```bash
git clone https://github.com/dakshshah96/file-itr-india.git \
  ~/.claude/skills/file-itr-india
```

**Codex**

```bash
git clone https://github.com/dakshshah96/file-itr-india.git \
  ~/.agents/skills/file-itr-india
```

For a single project rather than your whole machine, clone into
`.claude/skills/` or `.agents/skills/` inside the repo instead.

Start a new session afterwards. Both agents load the skill from its description,
so just say what you need — "help me file my ITR", "I have my Form 16 and a
broker tax P&L" — and it fires on its own. In Codex you can also call it directly
with `$file-itr-india`.

## Structure

```
SKILL.md                              the workflow, the handoff, form and regime choice
references/
  reconciliation.md                   tying income to documents; where AIS is blind
  regimes.md                          slabs, rebate, the ₹12L cliff, the tax script
  deductions-old-regime.md            what to collect if the old regime is in play
  capital-gains.md                    111A/112A, equity-oriented tests, Schedule 112A
  presumptive-business.md             44ADA/44AD for creators and freelancers
  foreign-assets.md                   Schedule FA — including the calendar-year trap
  virtual-digital-assets.md           crypto and NFTs under 115BBH
  portal-workflow.md                  the filing flow and the defect catalogue
  portal-automation.md                driving the Angular UI without silent failures
```

Reference loads by branch: every run reads reconciliation and regimes; capital
gains, presumptive income, foreign assets and crypto load only for the runs that
hit them.

## A few things it knows that cost real time to learn

- Validation runs in **two passes**, and clearing the first does not clear the second.
- The challan **does not** reliably auto-populate — and the reference on the payment
  screen is the gateway transaction ID, not the OLTAS CIN. The BSR code and challan
  serial live only on the downloaded receipt PDF.
- **Schedule FA reports the calendar year**, while every other schedule in the same
  return reports the financial year.
- The s.87A rebate is tested on total income *including* capital gains but offsets
  only slab-rate tax — so equity STCG is payable even well under the threshold, and
  crossing it forfeits the whole rebate rather than taxing the excess.
- A leaked `cdk-overlay-backdrop` swallows every click on the page with no error
  anywhere.

## Scope

India personal income tax only. Not GST, not TDS returns (24Q/26Q), not company
returns, not other countries.

## Contributing

Pull requests welcome. If this skill got something wrong, or missed a trap you
hit, open an issue or send a PR.

For portal problems, include the exact defect text and the AY you were filing
for. For tax corrections, cite the section.

## Credit

Inspired by [shivprime94/file-itr](https://github.com/shivprime94/file-itr).

This is a rewrite rather than a fork — better structured, with bug fixes and
improvements found while filing a return end to end.

## Disclaimer

This is not tax advice, and its author is not your chartered accountant. Rules
change every assessment year; AIS and 26AS are frequently incomplete or wrong; and
the figures you file are yours to stand behind. Verify anything that matters against
[incometax.gov.in](https://www.incometax.gov.in/) or a qualified professional.

## Licence

MIT — see [LICENSE](LICENSE).
