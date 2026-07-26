# Regimes, slabs, rebate and the cliff

> Figures below are **FY 2025-26 (AY 2026-27)** and drift every year. Re-confirm
> against incometax.gov.in or the Finance Act before relying on them.

Compute both regimes on the taxpayer's actual tied-out figures and pick the lower.
There is no universal winner: it turns entirely on how much they can genuinely
deduct. Little to claim, and the new regime wins comfortably. A full 80C plus 80D
plus HRA plus home-loan interest, and the old regime can win by a lot.

## New regime (s.115BAC) — the default

Default since FY 2023-24. Wider slabs, ₹75,000 standard deduction against salary,
and almost no other deductions.

| Total income | Rate |
|---|---|
| up to ₹4,00,000 | Nil |
| ₹4,00,001 – ₹8,00,000 | 5% |
| ₹8,00,001 – ₹12,00,000 | 10% |
| ₹12,00,001 – ₹16,00,000 | 15% |
| ₹16,00,001 – ₹20,00,000 | 20% |
| ₹20,00,001 – ₹24,00,000 | 25% |
| above ₹24,00,000 | 30% |

Slabs are the same at every age — no senior-citizen widening here.

**s.87A rebate:** up to ₹60,000, so tax is nil to a total income of ₹12,00,000.

## Old regime

Keeps the popular deductions, with narrower slabs and a ₹50,000 standard deduction.

| Total income | Rate |
|---|---|
| up to ₹2,50,000 | Nil |
| ₹2,50,001 – ₹5,00,000 | 5% |
| ₹5,00,001 – ₹10,00,000 | 20% |
| above ₹10,00,000 | 30% |

Senior citizen (60–79): the nil slab runs to **₹3,00,000**. Super senior (80+): to
**₹5,00,000**.

**s.87A rebate:** up to ₹12,500, so tax is nil to a total income of ₹5,00,000.

## The cliff

The rebate is the sharpest edge in the return, and it cuts twice.

**It is tested on total income *including* capital gains, but offsets only
slab-rate tax.** Never 111A, 112 or 112A. So a filer well under ₹12L still pays
tax on equity STCG — even ₹1 of it — while their entire slab tax vanishes.

**Crossing the threshold forfeits the whole rebate**, not just the tax on the
excess. At ₹11.9L of total income the slab tax is nil; a little more and up to
₹60,000 becomes payable. Marginal relief softens the landing by capping tax at the
excess over ₹12,00,000, and runs out at roughly **₹12,70,588** — past that the
full slab tax applies.

Two consequences worth stating to the taxpayer whenever they are near it:

- Realising one more capital gain can cost far more tax than the gain itself.
- Anything that lowers total income — an employer NPS contribution under 80CCD(2),
  which survives in the new regime — buys headroom under the cliff.

## Common to both

- **Cess:** 4% on (tax + surcharge).
- **Surcharge:** 10% above ₹50L, 15% above ₹1cr, 25% above ₹2cr, 37% above ₹5cr —
  the new regime caps it at 25%. Marginal relief applies just past each threshold.
- **Special rates** (111A, 112A, 112, 115BB, 115BBH) apply in both regimes, sit on
  top of slab tax, and take no rebate.
- **Rounding:** total income and tax round to the nearest ₹10 under s.288B.

## Advance tax, and when 234B/234C vanish

Under **s.208**, advance tax is payable only where the liability reaches
**₹10,000**. Below that, no advance-tax obligation ever arose — so **234B and 234C
are both nil**, however late in the year the income arose. Check this before
computing any interest; it disposes of the question entirely for small liabilities.

**234A** is separate: it runs on tax unpaid after the due date, so it bites only
when the return is filed late.

## Form 10-IEA

A taxpayer with **business or professional income** is in the new regime by default
and must file **Form 10-IEA before the due date** to opt out to the old regime.
They may switch back to new once; flip-flopping is restricted. Past the due date
with no 10-IEA on record, they are in the new regime for that year — say so plainly.

A taxpayer with **no business income** picks the regime in the return itself, every
year, freely. No form needed.

## Computing it

Run this rather than reasoning about slabs in prose. Compute taxable income
*separately* per regime — the old regime allows ₹50,000 standard deduction plus
Chapter VI-A, the new allows ₹75,000 and almost nothing else.

```python
# FY 2025-26 (AY 2026-27). Re-confirm the year's slabs first.

def tax_new(x):
    slabs = [(400000, 0), (800000, .05), (1200000, .10),
             (1600000, .15), (2000000, .20), (2400000, .25)]
    t = p = 0.0
    for cap, r in slabs:
        if x > cap:
            t += (cap - p) * r; p = cap
        else:
            return t + (x - p) * r
    return t + (x - 2400000) * .30


def tax_old(x, age=0):
    base = 500000 if age >= 80 else 300000 if age >= 60 else 250000
    t = p = 0.0
    for cap, r in [(base, 0), (500000, .05), (1000000, .20)]:
        cap = max(cap, p)
        if x > cap:
            t += (cap - p) * r; p = cap
        else:
            return t + (x - p) * r
    return t + (x - 1000000) * .30


def total_tax(slab_income, total_income, special_tax, regime, age=0):
    """special_tax = 111A/112A/112 tax, computed separately and rebate-proof."""
    base = tax_new(slab_income) if regime == "new" else tax_old(slab_income, age)
    cap, threshold = (60000, 1200000) if regime == "new" else (12500, 500000)

    if total_income <= threshold:
        slab = base - min(base, cap)                  # rebate, slab tax only
    else:
        # Marginal relief: just past the threshold, slab tax is capped at the
        # excess over it, so the cliff becomes a ramp.
        slab = min(base, max(0.0, total_income - threshold))

    return (slab + special_tax) * 1.04                # 4% cess
```

Marginal relief is what turns the cliff into a ramp — without it the script
overstates tax by tens of thousands for anyone just past the threshold.

Then reconcile line by line against the portal's Part B-TTI: gross tax, rebate,
cess, special-rate tax, 234A/B/C, taxes paid, amount payable. They should agree to
the rupee, allowing s.288B rounding.

## Presenting the choice

Give the taxpayer two numbers — "old regime ₹X, new regime ₹Y" — with the
old-regime deductions listed so they can see what they would need to actually hold.
Where the gap is small, mention that the new regime needs no proofs and is simpler
to defend under scrutiny; some people weigh that.
