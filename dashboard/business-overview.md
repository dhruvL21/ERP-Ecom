# Business Overview: Revised Product & Functional Specification

**Module:** Business Overview (ERP Command Center)  
**System:** OmniCommerce ERP  
**Status:** Revision 2 – Approved Specification  
**Supersedes:** Approved Specification v1 (10-area dashboard)  

---

## 1. What Changed and Why

Version 1 aggregated ten functional areas onto one screen. That was the traditional dashboard model: a wall of cards, charts, and scores that the user had to scan, interpret, and then act on elsewhere. Most of it answered questions nobody asked that day.

**Revision 2 changes the premise.** The Business Overview is no longer a place to passively view data. It is a place to be told what needs a decision, and to act on it immediately. Data is available on demand, never on display by default.

| v1 (Traditional Dashboard) | Revision 2 (Command Center) |
| :--- | :--- |
| Ten functional areas always visible | One verdict, one pace line, one decision queue; everything else on demand |
| Composite 0–100 health score as headline | Plain-language verdict (Steady / Watch / Act); composite score demoted to drill-down |
| Charts displayed for every metric | A chart appears only when it explains a specific decision or gap |
| User hunts for problems across widgets | Problems are ranked and brought to the user by cost of inaction |
| Drill-down to another module for every action | Resolve in place for common actions; drill-down for deep operational workflows |
| Same screen layout for all roles | Same underlying engine, strictly role-specific lens and server-side masking |

---

## 2. Design Principles

1. **Silence is a feature:** If nothing needs attention, the screen says so and stays nearly empty. An empty queue is the success state.
2. **Decisions over data:** Every element on screen must lead to a decision or an action. Reporting-only metrics live in Ask and Reports, not on the default surface.
3. **Cost of inaction, not just status:** An alert states what happens, and how much it costs, if nothing is done within a defined window.
4. **Ask, don't scan:** Any governed KPI is one question away. It does not need a permanent card.
5. **Explainable and governed:** All numbers resolve to the KPI Catalogue (Section 8): deterministic formulas, FIFO landed cost, traceable to source records.
6. **Role-aware by default:** RBAC applies to every element, including what the verdict sentence is permitted to state.
7. **Resolve in place:** Where safe, common operational actions are performed directly from the Overview itself.

### The Admission Test
A piece of information earns a place on the default surface **only if it passes all four checks**:
- **Threshold-driven:** It is not always shown; it appears only when it crosses a critical operational line.
- **Clear Owner:** A specific role is responsible for acting on it.
- **Clear Next Action:** It provides a direct button/action, not just an informational chart.
- **Measurable Cost:** Ignoring it has a quantifiable financial or SLA cost within a defined time window.

---

## 3. Roles & Access Matrix (RBAC)

| Role | Primary Responsibility | Permissions Scope | Financial Visibility | Default Lens |
| :--- | :--- | :--- | :--- | :--- |
| **Business Owner / CEO** | Direction, net profit, cash flow | Global / Full Access | Full (Net Profit, COGS, Margins) | Profit and cash risk first |
| **E-commerce Manager** | Channel sales, marketing pace, fulfillment | Operational Management | Limited (Gross Sales, Discounts, Blended Margin) | Pace, channel, and stock decisions |
| **Commerce Admin** | Integrations, API sync, data health | Integrations & Data Config | Restricted (No Profit/Cost data) | Sync health, tokens, and data trust |
| **Commerce Viewer** | Monitoring and read-only reporting | Read-Only | Read-Only Summary | Verdict and read-only brief |

*Role masking is applied server-side. Restricted values are never transmitted to the client, and the verdict sentence is dynamically regenerated per role so it never leaks masked figures.*

---

## 4. The Surface: Layout Model

The screen is a **single vertical flow**, not a fragmented card grid. Reading order corresponds directly to priority order:

```
+-----------------------------------------------------------------------+
|  THE VERDICT        (one plain-language sentence + state badge)       |
+-----------------------------------------------------------------------+
|  THE PACE LINE      (realized net revenue vs target, one clean line)  |
+-----------------------------------------------------------------------+
|  DECISION QUEUE     (ranked, actionable items by cost of inaction)   |
|    1. ... [Act / Resolve]  [Snooze]  [Delegate]  [Dismiss]            |
|    2. ...                                                             |
+-----------------------------------------------------------------------+
|  CHANNEL RAIL       (4 quiet health dots; expands only on anomaly)    |
+-----------------------------------------------------------------------+
|  ASK BAR & WATCHES  (Command palette: "What did Amazon returns cost?")|
+-----------------------------------------------------------------------+
```

---

## 5. Core Features

### 5.1 The Verdict
- A single sentence paired with a three-state indicator:
  - **Steady:** No immediate action needed.
  - **Watch:** Trending toward a problem; monitor trajectory.
  - **Act:** Money, inventory, or fulfillment SLAs at risk now.
- *Example:* `"Steady. Sales are ahead of pace by ₹42k; one stockout risk needs a PO today."`
- *Under the hood:* Retains the deterministic 5-factor calculation (Financial 25%, Sales 20%, Fulfillment 20%, Inventory 20%, Customer 15%), accessible only via drill-down modal along with WoW/MoM historical comparisons.
- *Strict Rule:* The verdict sentence may only cite facts that exist in the active Decision Queue or Pace Line.

### 5.2 The Pace Line
- Replaces the entire static sales performance section with one interactive progress line.
- Displays realized net revenue vs. target pace for the active horizon (Day, Week, or Month), clearly indicating ahead/behind status and projected landing.
- Clicking the line reveals the **three explanatory drivers only**: Channel Mix, Discounting Rate, and Order Volume.

### 5.3 The Decision Queue
The core operational heart of the Command Center. Items requiring human decisions are ranked dynamically:
$$\text{Priority} = \frac{\text{Value at Risk} \times \text{Urgency (Time to Impact)}}{\text{Effort to Resolve}}$$

Each queue item contains:
- **Situation:** One clean line describing the core event.
- **Cost of Inaction:** Quantified business impact if unaddressed within the window.
- **Suggested Action:** Concrete operational solution.
- **Controls:** `[Act / Resolve in Place]`, `[Snooze (with reason)]`, `[Delegate]`, `[Dismiss]`.
- **Ranking Transparency:** "Why is this #1?" formula breakdown on click.
- **Queue Discipline:** Maximum 7 items visible. Low-priority overflow is collapsed. Duplicate alerts sharing a single root cause are automatically merged.

### 5.4 The Ask Bar & Command Palette (`Ctrl + K`)
- Natural-language query interface mapped strictly to the Governed KPI Catalogue.
- Always displays the exact formula, date period, and source module alongside the answer.
- Answers can be temporarily pinned or converted into automated **Threshold Watches**.

### 5.5 The Channel Rail
- Four quiet health indicators: **Shopify (D2C)**, **Amazon (FBA/FBM)**, **Retail POS**, and **Wholesale B2B**.
- Displays only health states (`Healthy`, `Degraded`, `Broken`).
- Automatically expands into metrics **only when an anomaly occurs** (e.g., sync token expiration, sudden return spike, or fee deviation).

### 5.6 Collision Course (Inventory Engine)
- Surrounds only SKUs whose remaining runway (days of supply) is shorter than supplier lead time plus buffer.
- Identifies locked capital in dead/excess stock for liquidation or promotional bundling.

### 5.7 Leak Finder (Margin Engine)
- Watches for operational cost drains exceeding thresholds (e.g., international interchange fee creep, shipping surcharge spikes, promo code stacking).
- Surfaces findings directly into the Decision Queue with weekly cost impact and 1-click remediation.

### 5.8 Since You Last Looked (Change Feed)
- Summary catch-up feed displayed on return after absence.
- Lists only meaningful changes: new decisions, resolved items, expired snoozes, and key milestones.

### 5.9 What-If Simulation Sandbox
- Deterministic sandbox allowing operators to test actions before execution (e.g., *"What if I reallocate 200 units to Amazon FBA?"* or *"What if I pause discount code WINTER20?"*).

---

## 6. Governed KPI Catalogue

| KPI Display Name | Formula | Primary Source | Inclusion / Exclusion Rules |
| :--- | :--- | :--- | :--- |
| **Gross Sales** | `SUM(line_price * qty) + shipping + tax` | Orders Module | Includes tax and shipping. Excludes cancelled prior to payment. |
| **Net Sales** | `Gross Sales - (Discounts + Returns)` | Orders + Reconciliation | Excludes sales tax remittance and unearned deposits. |
| **Total Orders** | `COUNT(DISTINCT order_id)` | Order Operations | Excludes abandoned checkouts and fraudulent attempts. |
| **Average Order Value (AOV)** | `Net Sales / Total Orders` | Analytics Aggregates | Paid orders only. |
| **Gross Margin (%)** | `((Net Sales - Landed FIFO COGS) / Net Sales) * 100` | Finance Module | Landed product cost; excludes marketing SG&A. |
| **Inventory Runway (Days)** | `Available Units / Average Daily Burn Rate` | Inventory Module | Excludes allocated/reserved stock. |
| **Dispatch SLA Compliance** | `(Orders Shipped within SLA / Total Shipped) * 100` | Warehouse Module | Measured against channel promised ship dates. |
| **Return Rate (%)** | `(RMA Units / Total Units Sold) * 100` | Returns Engine | Calculated on cohort delivery date. |

---

## 7. Module Integration & Action Routing

```
                 BUSINESS OVERVIEW (COMMAND CENTER)
   Verdict | Pace Line | Decision Queue | Channel Rail | Ask Bar
        |            |              |               |
        v            v              v               v
  Order Operations  Warehouse    Inventory &     Finance &
  - Held / delayed  - Pick/Pack  Replenishment   Reconciliation
  - Release / hold  - Dispatch   - Draft PO      - FIFO COGS
                      lag        - Stock move    - Fees, net margin
```

- Every queue item maps to exactly one owning module and one primary action.
- Resolve-in-place actions invoke the owning module's backend API, ensuring permissions, validation, and audit logging are strictly enforced.
- Every action (resolve, snooze, delegate, dismiss) writes to an immutable audit ledger.
