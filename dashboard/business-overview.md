# Business Overview

## Product & Functional Specification

**Module:** Business Overview (ERP Command Center)  
**System:** OmniCommerce ERP  
**Status:** Approved Specification  

---

## 1. Executive Summary & Purpose

The **Business Overview** module serves as the central command center for the OmniCommerce ERP system. It provides executives, e-commerce managers, and operations leaders with a consolidated, real-time pulse of omnichannel performance, flags operational exceptions, and provides direct drill-down links to operational modules where work is executed.

### Core Principles
- **Decision-Oriented:** Prioritizes actionable metrics and immediate business impact over vanity data.
- **Explainable & Governed:** All KPIs adhere to traceable accounting formulas, cost bases (FIFO), and deterministic calculations.
- **Actionable Routing:** Every anomaly or exception directly links to the responsible operational module (Order Operations, Warehouse, Inventory, Finance, or POS).
- **Role-Aware Security:** Respects role-based access control (RBAC); sensitive financial margins or customer data are restricted by role.
- **Multi-Channel Scope:** Natively aggregates across Direct-to-Consumer (Shopify), Marketplaces (Amazon), Retail Store (POS), and Wholesale (B2B).

---

## 2. Roles & Access Matrix (RBAC)

| Role | Primary Responsibility | Permissions Scope | Financial Visibility |
| :--- | :--- | :--- | :--- |
| **Business Owner / CEO** | Strategic direction, net profit, cash flow | Global / Full Access | Full (Net Profit, COGS, Margins) |
| **E-commerce Manager** | Channel sales, marketing efficiency, fulfillment | Operational Management | Limited (Gross Sales, Discounts, Blended Margin) |
| **Commerce Admin** | System integrations, API sync, data health | Integrations & Data Config | Restricted (No Profit/Cost data) |
| **Commerce Viewer** | Monitoring and executive reporting | Read-Only | Read-Only Summary |

---

## 3. Information Architecture: 10 Functional Areas

The Business Overview aggregates data into 10 cohesive areas:

### 3.1 Business Health Engine
- **Composite Score (0–100):** Weighted index reflecting organizational performance across:
  - Financial Health (25%)
  - Sales Performance (20%)
  - Order Fulfillment (20%)
  - Inventory Efficiency (20%)
  - Customer Performance (15%)
- **Period Comparisons:** Current period vs. prior equivalent period (e.g., WoW, MoM, YoY).

### 3.2 Sales Performance
- **Core Volume Metrics:** Gross Sales, Net Sales (Recognized), Order Count, Average Order Value (AOV).
- **Pacing & Targets:** Realized revenue vs. daily/monthly target benchmark.
- **Sales Velocity:** Daily run-rate and peak volume patterns.

### 3.3 Order Intelligence
- **Order Status Distribution:** Completed/Delivered, In Transit, Packing Wave, Pending, and Cancelled.
- **Aging & Exceptions:** Identifies orders exceeding SLA thresholds (>24h unfulfilled).
- **At-Risk Value:** Total monetary value tied up in blocked or delayed orders.

### 3.4 Financial Intelligence & Unit Economics
- **Revenue Recognition:** Strict GAAP/IFRS separation between gross sales, customer discounts, landed FIFO COGS, shipping expenses, and gateway merchant fees.
- **Contribution Margin:** True net profit after deducting channel cuts, return allowances, and direct variable costs.
- **Margin Leakage Diagnostics:** Automated tracking of shipping surcharges, excessive discounting, and return losses.

### 3.5 Inventory Intelligence & Runway
- **Capital Allocation:** Total inventory valuation, unit counts across fulfillment centers.
- **Stock Depletion Forecaster:** Days of supply remaining (Runway) calculated against 30-day sales velocity vs. supplier lead times.
- **Four Capital Efficiency Quadrants:**
  1. *High Demand / Healthy Stock:* Core profit drivers.
  2. *High Demand / Low Stock:* Urgent PO trigger (stockout risk).
  3. *Low Demand / Excess Stock:* Capital locked; liquidation/bundle candidates.
  4. *Low Demand / Healthy Stock:* Long-tail specialty items.

### 3.6 Customer Intelligence
- **Cohort Mix:** New vs. returning customer revenue split.
- **Repeat Purchase Rate:** 30/60/90-day repurchase frequency.
- **Unit Economics:** Customer Acquisition Cost (CAC) vs. Lifetime Value (LTV) ratio.

### 3.7 Omnichannel Channel Performance
- **Active Channel Breakdowns:**
  - *Shopify Online Store (D2C)*
  - *Amazon Marketplace (FBA / FBM)*
  - *Retail Store POS*
  - *Wholesale B2B Portal*
- **Channel Economics:** Gross volume, take rates, return rates, and live API synchronization health.

### 3.8 Operational Health & Fulfillment
- **Dispatch SLAs:** Percentage of orders dispatched within guaranteed shipping window (Target &ge; 95%).
- **Cycle Time:** Average time from order placement to carrier barcode scan.
- **Reverse Logistics:** RMA return processing turnaround and inspection status.

### 3.9 Priority Alerts & Exception Handling
- **Categorization:**
  - **Critical:** Immediate financial or SLA risk (e.g., token expiration, stockout within 48h).
  - **Attention:** Action required within 24 hours (e.g., reorder point reached, return rate spike).
  - **Informational:** Milestone reached or batch sync completed.

### 3.10 Strategic Opportunities & Downside Risks
- **Restock Allocation:** Shifting idle regional inventory to fast-depleting channels (e.g., FBA).
- **Dead Stock Liquidation:** Bundling slow-moving accessories with hero SKUs.
- **Supplier Lead Time Delays:** Proactive warnings for vendor production backlogs.

---

## 4. Governed KPI Catalogue

| KPI Display Name | Mathematical Formula | Primary Source | Inclusion / Exclusion Rules |
| :--- | :--- | :--- | :--- |
| **Gross Sales** | `SUM(line_price * qty) + shipping + tax` | Orders Module | Includes tax and shipping. Excludes cancelled prior to payment. |
| **Net Sales** | `Gross Sales - (Discounts + Returns)` | Orders + Reconciliation | Excludes sales tax remit and unearned deposits. |
| **Total Orders** | `COUNT(DISTINCT order_id)` | Order Operations | Excludes abandoned checkouts and fraudulent attempts. |
| **Average Order Value (AOV)** | `Net Sales / Total Orders` | Analytics Aggregates | Calculated on paid orders only. |
| **Gross Margin (%)** | `((Net Sales - Landed FIFO COGS) / Net Sales) * 100` | Finance Module | Based on landed product cost; excludes marketing SG&A. |
| **Inventory Runway (Days)** | `Available Units / Average Daily Burn Rate` | Inventory Module | Excludes allocated/reserved stock. |
| **Dispatch SLA Compliance** | `(Orders Shipped within SLA / Total Shipped) * 100` | Warehouse Module | Measured against channel-specific promised ship dates. |
| **Return Rate (%)** | `(RMA Units / Total Units Sold) * 100` | Returns Engine | Calculated on cohort delivery date. |

---

## 5. Module Integration Points

```
+-----------------------------------------------------------------------+
|                       BUSINESS OVERVIEW (DASHBOARD)                   |
+-----------------------------------------------------------------------+
        |                 |                  |                 |
        v                 v                  v                 v
+---------------+ +----------------+ +---------------+ +---------------+
|     Order     | |  Warehouse &   | |  Inventory &  | |   Finance &   |
|  Operations   | |  Fulfilment    | | Replenishment | |Reconciliation |
+---------------+ +----------------+ +---------------+ +---------------+
  - Status queue    - Pick/Pack SLA    - Stock runway    - FIFO COGS
  - Unfulfilled     - Carrier scans    - Supplier POs    - Merchant fees
  - Delayed >24h    - Dispatch lag     - Reorder points  - Net margin
```

---

## 6. Implementation Guidelines

1. **Deterministic Aggregations:** Summary KPIs must be precalculated via scheduled batch rollups (e.g., hourly/daily pipelines) rather than running intensive runtime queries over raw transactional tables.
2. **Graceful State Handling:** The UI must display explicit states for:
   - *Live vs. Cached:* Distinguish real-time webhooks from scheduled aggregates.
   - *Estimated Data:* Clearly tag accruals, estimated freight, or pending settlement figures.
   - *Role Masking:* Dynamically blur or hide restricted financial metrics based on the active user session.
3. **Action Triggers:** Every card or alert must offer a 1-click drill-down to the corresponding workflow module.
