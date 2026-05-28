# Portfolio Deployment Dashboard — €300,000

An interactive offline dashboard for monitoring and executing your 18-month staged portfolio deployment plan.

---

## Quick Start

1. Open `portfolio-dashboard.html` in any modern browser (Chrome, Firefox, Safari, Edge).
2. No internet required after initial load — the page uses CDN libraries but works offline once cached.
3. Your data is saved automatically in browser `localStorage`. It persists between sessions in the same browser.

---

## Two Views

**Simple View** (default) — for daily execution:
- Summary cards showing invested, staging reserve, drawdown, and next action
- Next-action checklist in priority order
- Drawdown trigger ladder
- Allocation bars
- 18-month timeline progress

**Detailed View** — for audit and scenario planning:
- All Simple View sections
- Capital flow diagram
- Scenario simulator (4 scenarios)
- Full holdings vs target table
- Guardrail warnings
- Transaction log
- Settings panel
- Export / import panel

---

## First-Time Setup

1. Switch to **Detailed View → Settings**.
2. Set **Current month** to `0` (or your actual month if you have already started).
3. Set your **Start date** (Day 1 of the plan).
4. Set your **VWCE start price** (the price on Day 1, used as the drawdown reference) — or use **Manual drawdown %** instead.
5. Click **Mark Month 0 complete** when you have executed the initial €150,000 investment.

---

## Day-to-Day Use

### Every month
- Increase **Current month** by 1 in Settings (or in the timeline input).
- Click **Mark Month N DCA complete** when you have executed that month's ~€2,999 DCA buy.

### When markets fall
- Update the **current VWCE price** in Settings (or update the **manual drawdown %**).
- The trigger ladder will show any fired triggers in amber with an **"ACT NOW"** badge.
- Click **Mark executed** on each trigger after you have placed the buy.

### Deep-Crash Pivot (≥ −30% drawdown)
- A red banner appears automatically.
- Pause DCA for IBCI, ERNX, CMOE, SGLD.
- Redirect all remaining unallocated capital toward VWCE/XDW0.
- The banner remains until the drawdown recovers above −30%.

### By Month 18
- All €150,000 of temporary staging reserve must be deployed.
- A warning appears from Month 13 onwards showing the required monthly deployment rate.
- A breach warning appears if you pass Month 18 with staging reserve remaining.

---

## Key Distinction: Permanent Cash vs Staging Reserve

| | Permanent Cash | Temporary Staging Reserve |
|---|---|---|
| Amount | €15,000 | €150,000 (at start) |
| Final state | Always in portfolio | Zero by Month 18 |
| Part of target | Yes (5% of base target) | No — temporary only |
| Counted as invested? | Yes, once Month 0 is marked complete | No — it's the undeployed remainder |

The dashboard explicitly calls this out in the Capital Flow section and in the holdings table.

---

## Scenario Simulator

In Detailed View, the **Scenario Simulator** models four cases:

| Scenario | Drawdown | Triggers | Pivot |
|---|---|---|---|
| No Correction | 0% | None | No |
| 2022-style Bear | −22% | −10%, −20% | No |
| COVID / 1987 Crash | −33% | −10%, −20%, −30% | Yes |
| Dot-com / GFC Deep | −50% | All five | Yes |

The simulator is read-only — it does not affect your live state.

---

## Export / Import

In Detailed View → **Export panel**:
- **Download Status** — exports full current state + config as `portfolio-status.json`
- **Download Config** — exports plan config only as `portfolio-config.json`
- **⬆ Import** (top-right header button) — imports a previously exported JSON file to restore state

---

## Guardrail Warnings

The dashboard will warn you if:
1. A trigger has fired but you have not marked it as executed
2. You are past Month 18 with staging reserve remaining
3. Total deployed exceeds €300,000
4. Deep-crash pivot is active but you are still running non-equity DCA
5. Any asset is over its base target (outside of permitted crash-pivot overweight)

---

## Files

| File | Purpose |
|---|---|
| `portfolio-dashboard.html` | The main interactive dashboard — open this in a browser |
| `portfolio-config.json` | Plan configuration (can be imported into the dashboard) |
| `portfolio-state-sample.json` | Sample state at Month 4 with −20% trigger fired — shows what a live state looks like |
| `portfolio-dashboard-README.md` | This file |

---

## Test Checklist

See the separate test checklist section below.

---

## Test Checklist

### No-correction scenario
- [ ] Set drawdown to 0% (or leave unset)
- [ ] Set current month to 18
- [ ] Mark all DCA months 1–18 as complete
- [ ] Mark Month 0 as complete
- [ ] Expected: Staging reserve = €0, no triggers fired, "Fully allocated" state

### −20% scenario (2022-style)
- [ ] Set manual drawdown to −10%, check trigger badge turns amber ("FIRED")
- [ ] Mark −10% trigger as executed, check it turns green ("EXECUTED")
- [ ] Set manual drawdown to −22%, check −20% trigger turns amber
- [ ] Verify holdings increase correctly when each trigger is executed
- [ ] Verify staging reserve decreases by correct amount (€10,000 then €14,000)
- [ ] Verify deep-crash banner does NOT appear

### −33% scenario (COVID / 1987)
- [ ] Set manual drawdown to −33%
- [ ] Check that −10%, −20%, −30% triggers all show as fired
- [ ] Check that red "Deep-Crash Pivot Active" banner appears
- [ ] Check that checklist item 1 is the unfired trigger
- [ ] Mark all three triggers as executed
- [ ] Verify holdings updated correctly (total trigger spend = €39,000)

### −48% scenario (Dot-com / GFC)
- [ ] Set manual drawdown to −50%
- [ ] All five triggers should show as fired
- [ ] Banner: "Deep-Crash Pivot Active"
- [ ] Mark all triggers as executed
- [ ] Verify total trigger capital = €81,000
- [ ] Verify VWCE holding substantially exceeds base target (permitted under crash pivot)
- [ ] Verify no guardrail warning fires for VWCE overweight in deep-crash mode

### Month 18 full-allocation rule
- [ ] Set current month to 15, leave some staging reserve
- [ ] Check that "Month 18 approaching" warning appears with required monthly amount
- [ ] Set current month to 19 with staging reserve > 0
- [ ] Check "Plan breach" warning and banner appear

### Deep-crash pivot activates at −30%
- [ ] Set drawdown to −29%, check banner does NOT appear
- [ ] Set drawdown to −30%, check red banner DOES appear
- [ ] Check guardrail warning appears: "Pause IBCI/ERNX/CMOE/SGLD DCA"

### Permanent Cash vs temporary staging reserve
- [ ] Mark Month 0 as complete
- [ ] Verify €15,000 Cash appears in holdings table under "Defensive (Permanent Cash)"
- [ ] Verify staging reserve drops by €150,000 (not by €135,000)
- [ ] Verify Capital Flow section calls out the distinction explicitly
- [ ] In holdings table, verify Cash is not shown as part of staging reserve

---

## Known Rounding Note

The monthly DCA assigns €333 to each of IBCI, CMOE, and SGLD. Three × €333 = €999, making the actual monthly total **€2,999** (not €3,000). Over 18 months the shortfall is **€18**. This is displayed as a note in Detailed View and is absorbed into the completion reserve. No action needed.
