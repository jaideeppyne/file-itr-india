# Driving the e-filing portal

`eportal.incometax.gov.in`. The return-filing UI is a single-page Angular app —
functional, with a set of repeatable traps that cost hours when met cold.

## The flow

1. **e-File → Income Tax Returns → File Income Tax Return** → select AY → **Online**
   → select the form → reason for filing.
2. The portal pre-fills from AIS and 26AS. Verify the pre-fill against your
   tie-out: accept what is right, correct what is wrong, add what is missing.
3. Work the **Return Summary**: open each schedule, fill, **Confirm**.
4. **Proceed to Verification** → the taxpayer ticks the declaration →
   **Proceed to Validation**.
5. Clear both validation passes to zero **defects**.
6. Hand off: pay → submit → e-verify.

## Resuming, and the button that destroys the draft

Sessions expire after roughly 15 minutes of inactivity and drop you to
"Unauthorized". The draft survives server-side.

To get back: dashboard → **Resume Filing**. The adjacent **File Now** starts a
fresh return and **deletes the saved draft** — the note beside it says so. Reach
for Resume Filing every time.

Navigating by URL raises "Are you sure you want to Logout?" — answer **No** to stay.
Breadcrumb links navigate without it.

Expect to be logged out mid-fill at least once on a long return, and expect
schedule-level edits made just before a timeout to be the ones that did not persist.
Re-check them on return rather than trusting the confirmed badge.

## Confirming, and what un-confirms

Confirm every schedule. Editing any schedule silently flips the ones downstream —
Part B-TI and Part B-TTI especially — back to "Provide your confirmation". After
any edit, walk back down and re-confirm.

**Part B-TTI has no Confirm button of its own.** Confirm it from the Return
Summary row instead — click the row's *Provide your confirmation* label. Its own
footer offers **Pay Later / Pay Now** where other schedules offer Confirm.

**Skip Questions** on the schedule questionnaire preserves a curated schedule
selection; answering the questions can silently re-add schedules you removed and
reset answers such as "income from business = Yes".

Schedules the portal auto-adds with mandatory blanks — ESOP is the usual one —
are removed via **Select Schedule**. An ESOP schedule with no ESOP carries
mandatory PAN and DPIIT fields that block validation.

## The defect catalogue

Both validation passes report "Category of Defect A" errors that block upload.
These are the ones worth recognising on sight.

| Defect | Fix |
|---|---|
| *Please select from the drop down at field **Nature of employer*** | Set **Others** for any non-government employer |
| *Please select dropdown in **Value of perquisites 17(2)** / **profit in lieu 17(3)*** | Empty ₹0 rows carrying mandatory dropdowns. **The portal will not delete the last row in a breakup section**, so instead select a neutral nature — *Other benefits or amenities* and *Any Other* — with Description `Nil` and amount ₹0. Totals stay nil and no tax changes |
| *Please enter **Description*** on a 17(2)/17(3) row | The "Other…" natures require free text alongside |
| *Description is mandatory where amount is more than 0 for … **Receipts not in the nature of income*** in **Schedule EI** | Fill the Description with the basis for the exemption, naming the section it rests on |
| *Income under 44AD/44ADA/44AE is greater than zero → fill Sl.No 6 of **Part A-BS*** | The no-account balance sheet is empty. Enter a positive **cash balance**; debtors, creditors and stock can be zero |
| Blank **Schedule where offered** / **Item number of schedule** in Schedule FA | Usually a **duplicate row** rather than a missing value — see below |
| Secondary address or employer-nature dropdowns blank | Fill from the profile |

## Check a table before adding to it

A Schedule FA table can already hold a row that renders blank until the section is
expanded. Clicking **Add Another** then creates a *second* row, and filling that one
leaves the original behind — incomplete, and disclosing the same asset twice.

Expand the section and read the existing rows first. Where a row is already there,
tick its checkbox and use **Edit**; row-level Edit stays disabled until a row is
selected.

## Recording the challan

Sequence the payment **before** submission, so the amount payable reads ₹0 on the
return rather than leaving a demand outstanding against someone who has paid.

**The challan does not reliably auto-populate into Schedule IT**, including after
paying through the portal's own Pay Now. Plan to enter it by hand.

**The reference shown on the payment confirmation screen is the payment-gateway
transaction ID, not the OLTAS CIN.** Schedule IT wants the **BSR code** (7 digits)
and **challan serial number** (5 digits), and those appear only on the
**downloaded challan receipt PDF**. Ask the taxpayer for that file and read them
off it — an inferred BSR code fails OLTAS matching and raises a demand against
someone who has already paid.

Enter it under **Tax Paid → Advance Tax and Self Assessment Tax**: BSR code, date
of deposit, challan serial, amount. Then confirm Tax Paid and check Part B-TTI item
16 reads **₹0**.

## Bank accounts need pre-validating first

The return can only list accounts already **pre-validated in the taxpayer's
profile**. Adding one runs through Profile → My Bank Accounts → Add Bank Account
and ends in **Proceed To E-Verify**, which needs their OTP.

So raise it during document-gathering, not mid-fill. All non-dormant accounts held
at any time during the year get reported, including one opened mid-year.

## Verifying before submission

Read the **preview PDF** or the downloadable **JSON** and match every figure against
your own computation:

- income per head, and total income
- gross tax, rebate, cess, special-rate tax
- 234A, 234B, 234C
- TDS, TCS, advance tax and self-assessment challan
- amount payable — **₹0** after payment, allowing s.288B rounding
- regime flag, residential status, bank accounts, the foreign-asset flag
- the verification block: name, parentage, capacity, PAN, place, date

The JSON is the authoritative artifact. Matching it is the last check before the
taxpayer submits.

## The declaration is theirs

The verification page carries a checkbox — *"I solemnly declare that … the
information given in the return is correct and complete"* — and it gates the
validation button. That declaration belongs to the taxpayer; ask them to tick it.
It resets each time they leave the page.
