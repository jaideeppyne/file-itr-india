# file-itr-india

An agent skill for filing an Indian income tax return (ITR-1, 2, 3 or 4) on
`eportal.incometax.gov.in`. Works with
[Claude Code](https://claude.com/claude-code) and
[Codex](https://developers.openai.com/codex).

Tax rates are the easy part. What takes the time is matching every credit in your
bank statements to something that explains it, working out which regime is
actually cheaper for you, and getting through a portal that fails silently in a
dozen different ways.

> Written for FY 2025-26 (AY 2026-27). Slabs and thresholds move with most
> Budgets, and the portal changes between filing seasons. Every rate table in the
> skill carries the year it describes and tells the agent to re-confirm it, but if
> you're filing for a later year, check the numbers against
> [incometax.gov.in](https://www.incometax.gov.in/) before you trust them.

## What it does

It works the return in order: gather, reconcile, pick the form, compute, fill,
validate, verify.

Reconciliation is the part most guides skip. Every credit in every bank statement
has to land somewhere, either under an income head or under a stated reason it
isn't income (a transfer between your own accounts, a gift from a relative, a
loan, money coming back from your broker). Whatever's left over gets raised with
you instead of quietly ignored, because that leftover is usually either income
nobody remembered or a document nobody sent.

It computes both regimes in a script before the portal computes anything, so
there's an independent number to check the portal against rather than the other
way round.

Form selection checks three things that rule out ITR-1 and ITR-4 whatever your
income looks like: whether you were a director in a company, whether you held
unlisted shares at any point, and whether you hold anything abroad. None of them
show up in an income-shape test, and all three are common if you work at a
startup.

After that it fills the portal schedule by schedule, then reads the final preview
line by line against its own arithmetic before you submit anything.

## What you do yourself

Three things stay with you.

You log in, because your password and OTP are yours. You make the payment, once
the skill has told you the exact amount, the head (Minor Head 300) and the
assessment year. And you submit and e-verify, because the declaration is yours to
make.

E-verification has a 30-day deadline. Miss it and the return is void, as if you'd
never filed at all.

It also won't tell you to buy an insurance policy to save tax. Explaining what a
deduction is worth is fine. Recommending a product isn't.

## Install

Both agents read the same format, so it's the same clone. Only the destination
changes.

Claude Code:

```bash
git clone https://github.com/dakshshah96/file-itr-india.git \
  ~/.claude/skills/file-itr-india
```

Codex:

```bash
git clone https://github.com/dakshshah96/file-itr-india.git \
  ~/.agents/skills/file-itr-india
```

For one project rather than your whole machine, clone into `.claude/skills/` or
`.agents/skills/` inside the repo instead.

Start a new session afterwards. Both agents pick the skill up from its
description, so you can just say what you want: "help me file my ITR", or "I've
got my Form 16 and a broker tax P&L". In Codex you can also call it directly with
`$file-itr-india`.

## Structure

```
SKILL.md                              the workflow, what stays with you, form and regime choice
references/
  reconciliation.md                   tying income to documents; where AIS is blind
  regimes.md                          slabs, rebate, the ₹12L cliff, the tax script
  deductions-old-regime.md            what to collect if the old regime is in play
  capital-gains.md                    111A/112A, equity-oriented tests, Schedule 112A
  presumptive-business.md             44ADA/44AD for creators and freelancers
  foreign-assets.md                   Schedule FA, including the calendar-year trap
  virtual-digital-assets.md           crypto and NFTs under 115BBH
  portal-workflow.md                  the filing flow and the defect catalogue
  portal-automation.md                driving the Angular UI without silent failures
```

Every run reads `reconciliation.md` and `regimes.md`. The rest load only when they
apply, so a salaried filer with no capital gains never pulls in the crypto or
foreign-asset material.

## Things that cost time to find out

Validation runs twice. Clearing the first pass doesn't clear the second, which
applies extra rules on top of it.

The challan often doesn't auto-populate into Schedule IT, even when you pay
through the portal's own Pay Now button. The reference on the payment
confirmation screen is the payment gateway's transaction ID, not the OLTAS CIN.
Your BSR code and challan serial appear only on the receipt PDF you download.
Guess at them and the payment won't reconcile, so you get a demand notice for tax
you've already paid.

Schedule FA reports the calendar year. Every other schedule in the same return
reports the financial year.

The section 87A rebate is tested on total income including capital gains, but it
only offsets slab-rate tax. Equity STCG is payable even if you're well under the
threshold, and crossing the threshold costs you the entire rebate rather than tax
on the excess.

A leaked `cdk-overlay-backdrop` will swallow every click on the page without
raising an error anywhere. Buttons still take focus, coordinates still resolve,
and nothing happens.

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

This is a rewrite rather than a fork: better structured, with bug fixes and
improvements found while actually filing a return.

## Disclaimer

This isn't tax advice and it's no substitute for a chartered accountant. The
rules change every assessment year, AIS and 26AS are often incomplete or plain
wrong, and the numbers you file are yours to defend. Check anything that matters
against [incometax.gov.in](https://www.incometax.gov.in/) or ask a professional.

## Licence

MIT. See [LICENSE](LICENSE).
