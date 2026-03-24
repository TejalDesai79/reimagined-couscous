# Screen 10: Accounting & Forecasting Dashboard

## Purpose
Provide CFO and finance team with cash and accrual financial reporting, budget vs. actual analysis, and multi-scenario forecasting (revenue, cash, collections, capacity). Serves as the financial command center complementing the operational views on Screens 2 and 9.

## Primary Users
CFO (primary), CLO (read access to revenue/capacity forecasts), CEO/Managing Partner (read access), COO (read access)

## When This Screen Is Used (Workflow Stage)
Ongoing financial management. Monthly close, quarterly reviews, annual budgeting, ad-hoc financial analysis.

---

## LAYOUT (TEXTUAL WIREFRAME)

```
┌─────────────────────────────────────────────────────────────────────────┐
│ [Global Nav Shell — see Screen 1]                                       │
├─────────────────────────────────────────────────────────────────────────┤
│ ACCOUNTING HEADER                                                       │
│ Accounting & Forecasting  [Basis: Accrual ▾] [Period: Mar 2026 ▾] [⟳] │
│ [P&L] [Balance Sheet] [Cash Flow] [Budget vs Actual] [Forecasting]     │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│ TAB: P&L (default)                                                      │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Profit & Loss Statement — March 2026 (Accrual Basis)              │  │
│ │                                                                   │  │
│ │                          │ MTD      │ QTD      │ YTD              │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ REVENUE                                                           │  │
│ │   Legal fees earned      │ $285,000 │ $810,000 │ $810,000        │  │
│ │   Consultation fees      │ $12,500  │ $35,000  │ $35,000         │  │
│ │   Other income           │ $2,000   │ $5,500   │ $5,500          │  │
│ │ Total Revenue            │ $299,500 │ $850,500 │ $850,500        │  │
│ │                                                                   │  │
│ │ EXPENSES                                                          │  │
│ │   Attorney compensation  │ $142,000 │ $405,000 │ $405,000        │  │
│ │   Staff compensation     │ $58,000  │ $168,000 │ $168,000        │  │
│ │   Office & occupancy     │ $22,000  │ $66,000  │ $66,000         │  │
│ │   Technology             │ $8,500   │ $25,500  │ $25,500         │  │
│ │   Marketing              │ $12,000  │ $34,000  │ $34,000         │  │
│ │   Other operating        │ $15,000  │ $44,000  │ $44,000         │  │
│ │ Total Expenses           │ $257,500 │ $742,500 │ $742,500        │  │
│ │                                                                   │  │
│ │ NET INCOME               │ $42,000  │ $108,000 │ $108,000        │  │
│ │                                                                   │  │
│ │ [Drill down by office] [By practice area] [Export PDF] [Export XLS]│  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: BUDGET VS ACTUAL                                                   │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │                          │ Actual   │ Budget   │ Variance │ %    │  │
│ │ ─────────────────────────────────────────────────────────────────│  │
│ │ Revenue                  │ $299,500 │ $310,000 │ ($10,500)│ -3%  │  │
│ │   Estate Planning        │ $145,000 │ $140,000 │ $5,000   │ +4%  │  │
│ │   Estate Admin           │ $82,000  │ $90,000  │ ($8,000) │ -9%  │  │
│ │   Elder Law              │ $38,000  │ $42,000  │ ($4,000) │ -10% │  │
│ │   Tax Planning           │ $28,500  │ $30,000  │ ($1,500) │ -5%  │  │
│ │   Litigation             │ $6,000   │ $8,000   │ ($2,000) │ -25% │  │
│ │                                                                   │  │
│ │ Expenses                 │ $257,500 │ $265,000 │ $7,500   │ +3%  │  │
│ │                                                                   │  │
│ │ Net Income               │ $42,000  │ $45,000  │ ($3,000) │ -7%  │  │
│ │                                                                   │  │
│ │ 🔴 Variance flag: Estate Admin revenue -9% vs budget             │  │
│ │ 🟡 Variance flag: Elder Law revenue -10% vs budget               │  │
│ │                                                                   │  │
│ │ [Configure variance thresholds →]                                 │  │
│ └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│ ─────────────────────────────────────────────────────────────────────  │
│                                                                         │
│ TAB: FORECASTING                                                        │
│ ┌───────────────────────────────────────────────────────────────────┐  │
│ │ Forecast Model: [Revenue ▾]  Horizon: [90 days ▾]                │  │
│ │ Scenario: [Base ▾]  Compare: [☑ Budget] [☑ Optimistic]          │  │
│ │                                                                   │  │
│ │ ┌─────────────────────────────────────────────────────────────┐  │  │
│ │ │               FORECAST CHART                                │  │  │
│ │ │                                                             │  │  │
│ │ │  $400K ┤                                                    │  │  │
│ │ │        │            ╭──── Optimistic                       │  │  │
│ │ │  $350K ┤         ╭──┤                                      │  │  │
│ │ │        │      ╭──┤  ╰── Base forecast                     │  │  │
│ │ │  $300K ┤───●──┤  ╰──── Budget                             │  │  │
│ │ │     Actual     │                                            │  │  │
│ │ │  $250K ┤       ╰──── Conservative                         │  │  │
│ │ │        ├────┬────┬────┬────┬────┬────                      │  │  │
│ │ │        Mar  Apr  May  Jun  Jul                              │  │  │
│ │ └─────────────────────────────────────────────────────────────┘  │  │
│ │                                                                   │  │
│ │ Forecast drivers:                                                 │  │
│ │ • Pipeline: 87 qualified leads (conversion est: 31%)             │  │
│ │ • Capacity: 85% utilized (headroom for 8 more matters)          │  │
│ │ • Collections: 91% rate (trailing 90-day)                        │  │
│ │ • Avg fee: $4,800 (trailing 90-day)                              │  │
│ │                                                                   │  │
│ │ Forecast types: [Revenue] [Cash] [Collections] [Capacity]       │  │
│ │ [Adjust assumptions →] [Export] [Save scenario]                  │  │
│ └───────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## KEY COMPONENTS

### 1. Financial Statements (P&L, Balance Sheet, Cash Flow)
- **Data displayed:** Standard financial statement format with MTD/QTD/YTD columns; toggleable between cash and accrual basis
- **Actions:** Drill down by office/practice/attorney; Export PDF/XLS; Toggle basis; Compare periods

### 2. Budget vs. Actual
- **Data displayed:** Line-by-line comparison with variance ($ and %); variance flags for items exceeding thresholds
- **Actions:** Configure variance thresholds; Drill down; Export
- **Variance flags:** 🔴 >10% negative variance, 🟡 5–10% negative variance

### 3. Forecasting Engine
- **Data displayed:** Multi-scenario forecast charts (Optimistic/Base/Conservative) overlaid with budget and actual; forecast drivers summary
- **Forecast types per FSD:** Revenue, Cash, Collections, Capacity
- **Actions:** Adjust assumptions, switch forecast type, compare scenarios, export, save scenarios
- **CFO can adjust:** Financial forecast assumptions (conversion rate, avg fee, collections rate, expense growth)
- **CLO can adjust:** Capacity forecast inputs only (see Screen 6)

---

## INTERACTIONS & STATES

### Default State
P&L tab, accrual basis, current month. All data populated.

### Empty State
"Financial data will populate as matters generate billing activity."

### Loading State
Statement tables show skeleton rows. Charts show axis with shimmer.

### Error State
"Unable to load financial data. [Retry] [Contact finance admin]"

### Success State
- Assumption change: forecast chart updates with animation; toast "Forecast updated"
- Export: toast "Report exported"

---

## AUTOMATIONS & AI ASSISTS

- **Anomaly detection:** AI flags unusual variances with explanation
- **Forecast confidence bands:** Shown as shaded areas on chart
- **Trend analysis:** AI notes multi-month patterns (e.g., "Elder Law revenue declining 3 consecutive months")
- All insights are advisory — no automated financial actions

---

## ROLE-BASED VISIBILITY

| Feature | CFO | CLO | CEO | COO |
|---------|-----|-----|-----|-----|
| P&L / Balance Sheet / Cash Flow | Full R/W | Read-only (revenue lines) | Read-only | Read-only |
| Budget vs. Actual | Full R/W | Read-only | Read-only | Read-only |
| Forecasting (Financial) | Full R/W (adjust assumptions) | Read-only | Read-only | Read-only |
| Forecasting (Capacity) | Read-only | R/W (adjust inputs) | Read-only | Read-only |
| Variance threshold config | Full R/W | — | — | — |
| Export | ✅ | ✅ | ✅ | ✅ |

All other roles: **No access.**

---

## EDGE CASES & GUARDRAILS

- **Budget not set:** "Budget not configured for this period. [Set budget →]" — actuals still display
- **Accrual/cash discrepancy:** Banner: "Note: Significant timing differences between cash and accrual views. [View reconciliation]"
- **Trust balances:** Clearly excluded from P&L/operating accounts. Labeled "Trust balances are reported on Screen 11."
- **Forecast divergence:** If base forecast diverges >20% from budget: prominent alert "Forecast significantly deviates from budget. Review assumptions."
