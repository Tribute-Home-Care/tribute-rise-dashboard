# Tribute Rise Dashboard

MCE workspace for caregiver 6-month performance reviews ("Rise meetings") and the
Rise meeting tracker. Built for the Caregiver Excellence team (Charlotte Brown).

**Live page:** open `index.html` (or the GitHub Pages URL once enabled). Enter the
shared team password on first load — it is stored in your browser only.

## What's live from Viv (no uploads needed)

Synced through the Tribute API proxy, refreshed on a rolling cache (~30 min for
meeting forms, ~6 h for the heavy feeds):

- Caregiver roster, hire dates, markets, MCE assignment, Tribute Secure enrollment + TS level
- Service exceptions with full incident text (6-month window)
- On-time check-in % and **% of check-ins done manually** (per caregiver)
- Visit-note / task completion %
- Compliments — count *and* the actual text, for use in the meeting
- Last-minute bookings (visits picked up within 24 h of start)
- Weekend work — months a caregiver worked but had no weekend visit (from the
  "Weekend working Caregivers" report)
- Rise Meeting forms (#10102) and 45/90-day meeting forms for **all** markets
  (MA #10136/10137, MD #10138/10139, CHI #10140/10142) — full history, which
  drives due dates, segments, notes and the "Last Rise Meeting" card

## What's still uploaded (iSolved — optional)

These add data Viv doesn't have; everything else works without them:

| File | Adds |
|---|---|
| `PTO_and_Sick_Balances.xlsx` | PTO/sick balances, iSolved supervisor, job title |
| `Certifications.xlsx` | expiring/expired document alerts |

## Tuning knobs (top of the `<script>` in index.html)

- `LM_THRESHOLD_HRS` — what counts as a "last-minute" booking (default 24 h; the
  feed carries up to 72 h of lead time so this can be tuned without a redeploy)
- `ONTIME_RULE` — `onTime1` (Viv's "CheckIn On Time", ≤1 min late; default) or
  `onTime7` (7-minute grace)

## Backend

`vivProxy?endpoint=rise_dashboard` (gated, `X-Retention-Key`) in the `tribute-api`
Azure Function. Sources cache independently, so a slow first build resumes from
the finished pieces on retry. When Viv adds a new year-partitioned
"CheckIn/Out Timeliness" customQuery, append its id to `RISE_CHECKIN_CQS` in
`vivProxy.js`.
