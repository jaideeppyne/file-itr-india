# Capital gains

## The two buckets you meet most

| | Asset | Holding | Rate |
|---|---|---|---|
| **STCG u/s 111A** | Listed equity, equity MF, business trust — STT paid | ≤ 12 months | **20%** for transfers on/after 23 Jul 2024 (15% before) |
| **LTCG u/s 112A** | Same assets | > 12 months | **12.5%**, after a **₹1,25,000** annual exemption |

The 112A exemption applies to the *aggregate* of equity shares and equity-oriented
units, so a combined LTCG under ₹1.25L attracts nil tax however it splits.

Neither takes the s.87A rebate — see the **cliff** in `regimes.md`, which is where
that interaction bites hardest.

## Let AIS settle equity-oriented or not

The most consequential classification call for mutual-fund gains is
equity-oriented (111A/112A special rates, plus the 112A exemption) versus
non-equity (slab STCG; 12.5% LTCG without indexation after 24 months; or always
slab under s.50AA for post-Apr-2023 specified debt funds). Tax software and CA
computations get this wrong routinely — usually by dumping equity-fund LTCG into
"other than 112A" and losing the ₹1.25L exemption, or equity-fund STCG into slab
income at 30%.

Classify by the **AIS information code**, not by fund name or feel:

| Code | Meaning | Treatment |
|---|---|---|
| **SFT-18-EMF** | Sale of unit of equity oriented mutual fund (STT on each row) | 111A / 112A |
| **SFT-18-OTU** | Sale of other unit (STT column zero) | non-equity rules |
| **SFT-17-LES** | Sale of listed equity share | 111A / 112A |

Traps to check by name:
- **Arbitrage funds** are equity-oriented despite behaving like debt — 111A/112A.
- **Balanced-advantage, dynamic asset allocation and liquid funds** usually are
  not — slab STCG, and a CA treating them at slab is right.
- **Switch-outs count as redemptions** and carry STT for equity funds, so they get
  the same treatment as an ordinary sale.

Reviewing someone else's computation: reproduce their total first under their
classification. If it matches to the rupee, the disagreement is pure
classification, and the AIS codes settle it.

## Aggregating a broker tax P&L

Schedule CG wants **full value of consideration** and **cost of acquisition** per
bucket, not just the net gain. Sum sale value and buy value separately across the
trades in each bucket; the difference must reproduce the broker's own stated
profit for that bucket to the rupee. If it does not, the bucket boundaries are wrong.

Watch for a leading blank column when parsing a broker's spreadsheet export — an
off-by-one silently turns quantity into buy value and still produces plausible
totals.

## Schedule 112A takes a consolidated row

Where every holding was **acquired after 31 January 2018**, grandfathering cannot
apply, so scrip-wise detail is not required. Selecting *"After 31st January 2018"*
makes the portal replace whatever scrip detail was entered with ISIN
`INNOTREQUIRD` and name `CONSOLIDATED`, and keep only the aggregate consideration
and cost.

So enter **one consolidated row** for the whole 112A total. Building a row per
scrip first wastes the effort and is discarded on save.

Where any holding predates 1 February 2018, the grandfathering columns (FMV as on
31-Jan-2018, and the lower-of test) do matter, and the rows must be scrip-wise.

## Quarterly breakup drives 234C

Schedule CG asks for gains split by the quarter they accrued in — up to 15 Jun,
16 Jun–15 Sep, 16 Sep–15 Dec, 16 Dec–15 Mar, 16–31 Mar. The portal computes 234C
from it, so the split needs to be right.

Two rules make it behave:

**The quarterly figures must sum to the schedule total.** Round each quarter so
the total ties exactly, rather than rounding each independently and leaving a
rupee adrift.

**Quarters take no negatives.** A quarter with a net loss cannot be entered as a
negative — the table draws from Schedule BFLA, which is post-set-off. Absorb the
loss into the following quarters' gains, earliest first, so the total still ties.
A Q1 loss of ₹20,000 against Q2 ₹8,000 and Q3 ₹50,000 becomes Q1 nil, Q2 nil,
Q3 ₹38,000 — the ₹20,000 absorbed by ₹8,000 then ₹12,000, and the total still
₹38,000.

Where the liability is under ₹10,000 no advance tax was due at all, so 234C is
nil regardless — but the disclosure still has to be accurate.

## Where it flows

Schedule CG → **Schedule SI**, which applies the special rates → Part B-TI's
capital-gains line → Part B-TTI's "tax at special rates".

Verify at Schedule SI: the tax shown against each section should equal gain × rate,
with 112A reading nil while the gain sits inside the ₹1.25L exemption.
