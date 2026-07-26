# Presumptive taxation — creators, freelancers, small business

Content creators, freelancers, consultants and independent professionals can
usually declare income **presumptively**, avoiding books of account and audit
while they stay within the limits. For a small or medium operator this is almost
always the right choice.

## 44ADA or 44AD

| | s.44ADA — profession | s.44AD — business |
|---|---|---|
| Presumptive income | **50%** of gross receipts | **8%** of turnover, or **6%** where received through banking or digital channels |
| Limit | ₹50L gross receipts (₹75L if ≤5% cash) | ₹2cr turnover (₹3cr if ≤5% cash) |
| Fits | specified professions and CBDT notified-profession codes | traders, small business |

**Business code 16021 — "Social media influencers" — is a *profession* code and
appears only under the 44ADA dropdown.** Starting under 44AD, the code is simply
absent, which invites mis-classification into some adjacent business code. For an
online creator, choose **44ADA + 16021** and declare 50%.

Pick the section that legally fits rather than the one with the smaller
percentage: 44AD's 6% looks cheaper, but profession income declared as business
income is the wrong return.

## Building gross receipts

Gross receipts are every rupee earned from the activity in the FY:

- platform payouts — Stripe, YouTube, X, PayPal, brand deals — summed from the
  payout files **and** confirmed against bank credits, so you know the money landed
- contract and professional receipts visible in 26AS under 194C and 194J

A payout already captured inside a 26AS entry counts once.

```
# Illustrative figures only.
Contract receipt (26AS 194C)        20,000
Platform payouts (bank-confirmed)   30,000
Gross receipts          (62i)       50,000
Presumptive income @50% (62ii)      25,000
```

## Where it goes in ITR-3

- **Part A – P&L, item 62** (44ADA) or **61** (44AD): business code, gross receipts
  split by mode, and the presumptive income. 50% is a floor, not a cap — declare
  more where they genuinely earned more.
- **Schedule BP**: the presumptive figure flows to item 35ii (44ADA) → A37 → D,
  income chargeable under PGBP. It should equal the percentage applied.
- **Part A – Balance Sheet, item 6 — the no-account case**: because income is
  declared without books, this block is mandatory. Sundry debtors, sundry
  creditors, stock-in-trade and **cash balance**. Leaving all four at zero raises a
  blocking **defect**. For a service creator with no inventory: debtors, creditors
  and stock at zero, and a defensible positive **cash balance** — the retained net
  profit is a clean figure. Disclosure only; it changes no tax.

## When presumptive stops working

- **Above the limit** — ₹50L for 44ADA, ₹2cr for 44AD — presumptive is unavailable.
  Regular books, and possibly a s.44AB tax audit, are needed. Escalate; this is
  past a quick self-file.
- **Real profit below the percentage** — declaring the lower actual profit requires
  maintaining books and a tax audit. Most small operators declare the presumptive
  percentage and move on.

## Consequences elsewhere

Business or professional income makes the taxpayer a business filer, which changes
two things:

- The form becomes **ITR-4** (within limits, no capital gains) or **ITR-3**.
  Capital gains alone push a presumptive filer to ITR-3.
- The old regime now needs **Form 10-IEA before the due date**. See `regimes.md`.
