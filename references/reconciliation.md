# Tying income to source documents

One number per income head, each traced to a document. Build the table and stay
on it until every figure **ties out** — an auditor, or the taxpayer a year from
now, should be able to follow any rupee back to the paper it came from.

## What each document gives you

| Document | What it settles |
|---|---|
| **Form 16** (each employer) | Gross salary 17(1), perquisites 17(2), profits in lieu 17(3), TDS, and the regime the employer applied |
| **Form 26AS** | Every TDS/TCS entry against the PAN, with deductor TAN and section — 192 salary, 194 dividend, 194C contract, 194J professional, 194-IB rent |
| **AIS / TIS** | The wider feed: interest, dividends, securities sales, mutual-fund transactions |
| **Bank statements** | Ground truth for interest credited, and for tracing whether money actually landed |
| **Broker tax P&L** | Realised STCG/LTCG with dates, cost and STT flag |
| **Platform payout files** | Creator and freelance gross receipts |

## The bucket discipline

Read every credit in every statement and put it in exactly one bucket. Two kinds:

**Income** — salary, professional receipts, interest, dividends, rent, gains.

**Not income, with a named reason** — transfers between the taxpayer's own
accounts, gifts from relatives, loan proceeds and repayments, merchant refunds,
capital returning from a broker, reimbursements against actual expense.

Leftovers are the point of the exercise. A credit nobody can name is either
undeclared income or a reason not yet given — surface it and ask.

Useful signals when reading statements:
- The taxpayer's **own mobile number** in a UPI handle marks self-transfers
- `ACH-CR-<COMPANY>-NACH` credits are **dividends**, and cheaply reveal an equity
  portfolio the taxpayer forgot to mention
- Broker withdrawals are capital returning, not income — but they prove capital
  gains exist and a tax P&L is needed
- Regular small credits from one person are usually shared expenses; confirm rather
  than assume

## AIS is a map with blind spots

Treat AIS as evidence, not truth. It fails in **both** directions.

**It omits.** Peer-to-peer lending platforms, small banks below the reporting
threshold, foreign platforms, and cash receipts frequently never appear. Income
absent from AIS is still taxable and still declared — omitting it is
under-reporting and invites a s.270A penalty. P2P interest belongs in Schedule OS
under *"Others including interest from Companies, NBFCs & HFCs"*, since the
RBI-registered platforms are NBFC-P2Ps.

**It understates.** A head AIS does report can still be low — some payers report
late or not at all. Where a broker's own dividend statement reconciles exactly to
the bank credits and AIS is short, the broker and bank are right. Declaring more
than AIS shows is safe; declaring less is not.

**It double-counts.** The same payout seen by two reporters appears twice. A
payout already inside a 26AS entry counts once.

Where AIS shows income nobody can trace, ask before dropping it — it may be a
duplicate, a joint-account entry, or genuinely theirs.

## When one payer's credits exceed their Form 16

A single employer whose bank credits far exceed the Form 16 gross is common, and
the gap needs a name before it can be excluded.

Separate the **fixed monthly component** from the rest. A constant monthly credit
sitting a little under a round number usually explains itself as that round number
less monthly professional tax — a state levy of a few hundred rupees — and twelve
of them tie to the Form 16 gross.

The irregular balance is one of: **bonus or variable pay** (salary — should be in
Form 16), **consulting fees** (business income, changes the form and opens
presumptive taxation), or **expense reimbursement** (not income). Form 16 and 26AS
settle the first two. Only the taxpayer can settle the third.

Where they call it reimbursement, record it: reimbursement is a clean exclusion
only if each payment covers an actual expense incurred for the payer, is backed by
bills, and is booked as an expense in the payer's accounts. Say so plainly, note
the amount and dates, and move on — it is their call. If the taxpayer is also a
director of the paying company and signed their own Form 16, that is the single
item most likely to be questioned, and worth flagging as such.

## Worked pattern

```
# Invented figures, for shape only.
SALARY
  Employer A (Form 16 / 26AS-192)             9,00,000
  − standard deduction (new regime)            −75,000
  Income from Salary                           8,25,000

CAPITAL GAINS (broker tax P&L)
  STCG 111A, listed equity, STT paid             40,000
  LTCG 112A, equity + equity MF                  90,000

OTHER SOURCES
  Savings interest, 3 banks                       8,000   ← incl. a bank absent from AIS
  P2P lending interest                            1,500   ← AIS blind spot
  Dividends                                       2,500   ← broker sheet = bank credits

TOTAL INCOME                                   9,67,000
```

Every line names its source. A line that cannot name one is not ready to be filed.
