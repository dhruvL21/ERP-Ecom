# Order Operations — Product & Functional Specification

**Product:** OmniCommerce ERP  
**Service:** E-commerce  
**Module:** Order Operations (Unified Order Management / OMS)  
**Document Type:** Product, UX, and Functional Specification  
**Status:** Proposed Design Baseline  
**Preceding Module:** [Business Overview](file:///c:/Users/DHRUV/OneDrive/Desktop/ERP-Ecom/dashboard/business-overview.md)

---

## 1. Dashboard Overview

**Order Operations** is the live operational command centre for every order entering the business across D2C, B2C, and B2B channels. Instead of a traditional wall of KPI cards, charts, and giant tables, it behaves like a **live control room** focused on moving orders forward.

### What It Helps Teams Answer in Seconds
1. Which orders are moving normally? *(Quiet by default)*
2. Which orders are stuck, delayed, or need a decision right now?
3. What happens to the customer or business if we don't act?
4. What is the next best action, and who owns it?
5. Can we resolve it right here, or does it belong to Warehouse, Inventory, or Finance?

### Core Design Approach
- **Work, not metrics:** Prioritize actionable exceptions over vanity charts.
- **Journey-first:** Show the live order lifecycle as a connected flow, not isolated status labels.
- **Resolve in context:** Click an order to open its side workspace and act without losing your place in the queue.
- **Role-aware by design:** Order Managers, Ops Specialists, and Customer Service each see the layout, data, and actions suited to their role.

### Main Workspaces
1. **Command Centre (Default):** Flow Pulse, Live Order Journey, Attention Queue, Workload & Ownership, Channel Signals, and Ask Order Operations.
2. **Order Explorer:** Searchable unified inbox for deep order investigation.
3. **Returns & Exceptions:** Dedicated coordination for cancellations, returns, exchanges, and delivery failures.
4. **Rules & SLAs *(Manager Only)*:** Configurable dispatch clocks, priority weights, and approval thresholds.

### Command Centre Layout at a Glance

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ ORDER OPERATIONS                                      Search order / Ask AI  │
│ All channels  ·  All stores  ·  Today  ·  Updated 30 sec ago                │
├──────────────────────────────────────────────────────────────────────────────┤
│ FLOW PULSE                                                                    │
│ Dispatch is the current bottleneck. 18 paid orders are waiting for picking.  │
│ 5 are at risk of missing their promised dispatch time. [See affected orders] │
├──────────────────────────────────────────────────────────────────────────────┤
│ LIVE ORDER JOURNEY                                                            │
│                                                                              │
│ Received ─ Verification ─ Paid ─ Reserved ─ Picking ─ Packing ─ Dispatched   │
│   126          9           84       71        42        28         93         │
│                2 risk       3       18 risk    4         2          5         │
│                                                                              │
│ Delivered: 214       On Hold: 7       Delivery Exception: 6       Returns: 9 │
├───────────────────────────────────────────────┬──────────────────────────────┤
│ ATTENTION QUEUE                                │ WORKLOAD & OWNERSHIP         │
│                                               │                              │
│ HIGH  #ORD-10482  Reservation failed          │ Unassigned exceptions     8  │
│       Waiting 42m · Customer promise at risk   │ Near SLA deadline         12 │
│       [Open] [Assign] [Escalate]               │ Overdue tasks              4 │
│                                               │                              │
│ HIGH  #ORD-10491  Carrier pickup not confirmed│ Team workload               │
│       Dispatch SLA in 18m                      │ A. Shah       7 open         │
│       [Open] [Retry] [Escalate]                │ R. Mehta      5 open         │
│                                               │ S. Khan       9 open         │
│ MED   #B2B-2081   PO approval pending          │ [Balance workload]           │
├───────────────────────────────────────────────┴──────────────────────────────┤
│ CHANNEL SIGNALS                                                               │
│ Website  ● Healthy   Marketplace  ! 3 sync issues   POS  ● Healthy            │
│ B2B      ! 4 approvals pending    EDI/API  ! 2 failed messages                 │
├──────────────────────────────────────────────────────────────────────────────┤
│ ASK ORDER OPERATIONS                                                         │
│ “Show orders that may miss dispatch today...”                      [Ask →]   │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Overall Features at a Glance

| Area | Feature | What It Does |
| :--- | :--- | :--- |
| **Live Monitoring** | **Flow Pulse** | Summarizes current bottlenecks in one plain-language sentence linked to affected orders. |
| | **Live Order Journey Map** | Shows real-time order counts and bottlenecks across every stage from *Received* to *Delivered*. |
| | **Order Stagnation Detector** | Flags orders stuck in a stage longer than expected. |
| | **Channel Signals Rail** | Compact health strip for Website, Marketplace, POS, B2B, and EDI/API — surfaces sync issues only. |
| **Prioritization & Action** | **Attention Queue** | Ranked list of blocked or at-risk orders with SLA countdowns and direct action buttons. |
| | **Exception Intelligence & Promise Risk Radar** | Automatically detects stock failures, payment mismatches, carrier timeouts, and delivery risks. |
| | **Order Detail Workspace** | Slide-over panel showing dependency blockers, customer promise, items, timeline, and notes. |
| | **Safe Action Preview** | Shows impact, downstream notifications, and requires an audit reason before risky changes. |
| | **Bulk Operations** | Safely assign, hold/release, retry, or export multiple orders with eligibility previews. |
| **Collaboration & Service** | **Workload & One-Click Handoff** | Tracks unassigned/overdue work and transfers orders across teams with full context attached. |
| | **Customer Service Resolution Desk** | Search-first lookup with a customer-safe timeline and verified reply drafting. |
| **Specialized Workflows** | **Unified Order Inbox & Explorer** | Consolidates, deduplicates, and searches orders across B2C, D2C, B2B, and POS. |
| | **B2B Order & PO Workflows** | Handles PO documents, credit checks, MOQ validation, and multi-level approvals. |
| | **Returns, Exchanges & Cancellations** | Coordinates reverse logistics, warehouse pickup/QC, cancellation cutoffs, and finance refunds. |
| **Governance & Intelligence** | **Rules, SLAs & Notifications** | Configurable SLA clocks, escalation ladders, and deduplicated alerts. |
| | **Ask Order Operations (AI)** | Natural-language search, order summaries, and reply drafts strictly bounded by RBAC. |
| | **Operational Insights** | Root-cause bottleneck analysis (ageing stages, hold reasons, delivery failures). |

---

## 3. Feature-by-Feature Guide (Simple Explanations)

### 3.1 Flow Pulse
- **What it is:** A single sentence at the very top of the dashboard telling you the health of your order flow right now (e.g., *"Dispatch is the current bottleneck. 18 paid orders have not entered picking"*).
- **Why it helps:** You don't have to scan 10 charts to know what's wrong. Clicking the link in the sentence filters straight to those affected orders.

### 3.2 Live Order Journey Map & Stagnation Detector
- **What it is:** A visual pipeline of your order stages (*Received → Verification → Paid → Reserved → Picking → Packing → Dispatched → Delivered*) plus side paths (*On Hold*, *Stock Unavailable*, *Delivery Exception*, *Returns*).
- **Why it helps:** Normal orders stay quiet, while stages with bottlenecks visually stand out. The **Stagnation Detector** flags orders sitting in one stage longer than normal (e.g., >2x expected time) so nothing silently gets forgotten.

### 3.3 Attention Queue
- **What it is:** The main to-do list of Order Operations. Instead of showing all 600+ orders, it shows only the orders that need a human decision, ranked automatically by **SLA urgency, customer promise impact, order value, and downstream blockers**.
- **Why it helps:** Operators always know what to work on first, why it's urgent, and what button to click next (*Open Workspace, Assign, Retry, Approve, Escalate,* or *Snooze*).

### 3.4 Order Detail Workspace (Dependency Chain, Promise View & Order Replay)
- **What it is:** A slide-over panel that opens when you click any order, keeping the main queue visible behind it. Inside, you get:
  - **Dependency Chain:** Shows the exact blocker (e.g., `Order Confirmed → Payment Paid → Stock Reservation FAILED → Warehouse Blocked`).
  - **Customer Promise View:** Shows what delivery/dispatch date was promised and whether it is still achievable.
  - **Order Replay (Event Timeline):** A step-by-step chronological history of every event from checkout to delivery, great for resolving disputes or integration errors.
  - **Items, Payment Snapshot, Documents & Notes:** Everything needed to understand the order in one place.

### 3.5 Unified Order Inbox & Search (Order Explorer)
- **What it is:** A single normalized inbox that collects orders from your website, mobile app, Amazon/Flipkart, POS stores, B2B portal, and EDI/API.
- **Why it helps:** Duplicate imports are automatically caught, original channel IDs are preserved, and teams can search instantly by Order ID, tracking number, SKU, PO number, or customer details.

### 3.6 Exception Intelligence & Promise Risk Radar
- **What it is:** Background monitoring that catches problems early—such as payment captured without confirmation, failed stock reservations, overdue picking, carrier API timeouts, or failed delivery attempts.
- **Why it helps:** Every exception gets a clear reason code, severity, owner, and SLA clock, while the **Promise Risk Radar** warns you before a customer's promised delivery date is missed.

### 3.7 Order Actions, Amendments & Safe Action Preview
- **What it is:** Controlled actions to hold/release orders, fix addresses before dispatch, split/merge orders, cancel orders, or retry failed syncs.
- **Why it helps:** Before any sensitive change runs, **Safe Action Preview** shows what will change, which systems (Warehouse, Inventory, Finance) will be notified, and requires a reason for the permanent audit log.

### 3.8 B2B Order & PO Workflows
- **What it is:** Dedicated support for wholesale/B2B orders—handling attached Purchase Orders (POs), negotiated contract pricing, minimum order quantities (MOQ), Net-30/45 credit term checks, and multi-level manager approvals.
- **Why it helps:** B2B orders have different rules than consumer orders. Managers can review terms and approve POs cleanly without mixing them into standard B2C flows.

### 3.9 Returns, Exchanges & Cancellations
- **What it is:** A coordinated workflow for reverse logistics. It checks return eligibility, enforces cancellation cutoffs (e.g., asking Warehouse to stop picking if packing hasn't finished), tracks return pickups, and sends approved refund requests to Finance.
- **Why it helps:** Keeps Order Ops, Warehouse (inspection), and Finance (refund posting) synchronized without duplicating each other's jobs.

### 3.10 Customer Service Resolution Desk
- **What it is:** A specialized, search-first view for Customer Service agents. It hides internal profit margins and B2B cost rules, showing a **customer-safe timeline** and plain-language status updates.
- **Why it helps:** Agents can answer *"Where is my order?"* in seconds, send verified status replies, and escalate issues to Ops without needing to decode warehouse codes.

### 3.11 Workload, Ownership & One-Click Handoffs
- **What it is:** A live view of unassigned exceptions, overdue tasks, and how many open items each team member has.
- **Why it helps:** Managers can rebalance overloaded queues in one click. Specialists can use **One-Click Handoff** to pass an order to Inventory, Warehouse, or Customer Service with the full timeline and reason already attached.

### 3.12 Channel Signals Rail
- **What it is:** A compact status bar for all sales channels (Website/D2C, Marketplace, POS, B2B Portal, EDI/API).
- **Why it helps:** Instead of cluttering the screen with channel scorecards, channels stay quiet unless there is an issue (like an Amazon sync delay or pending B2B approvals). Clicking a channel filters the dashboard to it.

### 3.13 Bulk Operations
- **What it is:** Tools for authorized roles to assign, hold/release, retry integration errors, tag, export (with sensitive fields masked), or cancel multiple orders at once.
- **Why it helps:** Shows exactly how many selected orders are eligible vs. excluded (and why) before running large batches safely in the background.

### 3.14 Rules, SLAs & Notifications
- **What it is:** Configurable SLA timers per channel and shipping tier (e.g., 4h Marketplace dispatch vs. 24h B2B), escalation ladders, and grouped alerts.
- **Why it helps:** Prevents alert fatigue while ensuring critical SLA warnings always reach the right owner on time.

### 3.15 Ask Order Operations (AI Assistance)
- **What it is:** A `Ctrl+K` command bar where you can ask plain-English questions like *"Why is #ORD-10482 on hold?"* or *"Draft a customer reply for #ORD-10510"*.
- **Why it helps:** Answers are strictly grounded in verified order events and respect user permissions. If you ask AI to take an action, it always shows a confirmation preview first.

### 3.16 Operational Insights
- **What it is:** Root-cause reporting focused on operational bottlenecks—where orders spend the most time, top hold reasons, repeated integration failures, and delivery failure patterns.
- **Why it helps:** Every insight links straight back to the underlying orders so teams can fix systemic problems.

---

## 4. Roles & Role-Based Access Control (RBAC)

Order Operations shares one unified engine, while adapting the landing view, visible fields, and allowed actions per role.

### 4.1 Role Intent

| Role | Primary Responsibility | Default Experience |
| :--- | :--- | :--- |
| **Order Manager** | Own overall order performance, workload, escalations, policies, and operational outcomes. | **Operations Command Centre:** flow health, risk, workload rebalancing, SLA config, and approvals. |
| **Order Ops Specialist** | Process orders, investigate exceptions, coordinate fulfilment, and execute permitted operational actions. | **Action Workbench:** assigned work, dependency-blocked orders, next steps, and handoffs. |
| **Customer Service** | Resolve customer questions and order-related service cases. | **Customer Resolution Desk:** search-first lookup, customer-safe timeline, and verified communications. |

### 4.2 Permission Matrix

*Legend: **Full** = within assigned scope; **Limited** = specified actions/fields only; **View** = read-only; **No** = unavailable.*

| Capability | Order Manager | Order Ops Specialist | Customer Service |
| :--- | :---: | :---: | :---: |
| View Command Centre & all orders in scope | Full | Team/assigned scope | Service scope |
| View order lifecycle & timeline | Full | Full | Customer-safe view |
| View customer contact details | Limited by policy | Limited by policy | Full for service |
| View payment state | Full status | Status only / limited | Status only (no card data) |
| View & add internal notes | Full | Full | Service-scoped notes |
| Assign / reassign work | Yes | Only if delegated | No |
| Hold / release / amend / split / merge | Policy-controlled | Allowed actions/fields | Request / escalate only |
| Cancel an order | Approve or execute | Within policy | Request; execute if authorized |
| Initiate return / exchange | Yes | Yes | Yes (CS policy) |
| Approve or execute refund | Approve by finance policy; Finance executes | No | No |
| Bulk actions | Yes (with safeguards) | Limited approved actions | No |
| Configure SLA & priority rules | Yes | No | No |
| Export data & view audit history | Full (field-masked) | Limited / relevant orders | Restricted / service history |

### 4.3 Security Guardrails
- Enforce RBAC **server-side** on every read, search, export, and action.
- Mask internal margins, supplier costs, confidential B2B pricing, and card credentials from unauthorized roles.
- Log all sensitive actions (holds, cancellations, overrides, approvals) in an immutable audit trail with actor and reason.

---

## 5. End-to-End Key Workflows

1. **Normal D2C Order:** Order arrives → normalized & deduplicated → payment verified → stock reserved → warehouse picks & packs → carrier dispatches & delivers → closed quietly.
2. **Stock Reservation Failure:** Inventory returns a shortfall code → Attention Queue surfaces it with SLA countdown → Specialist coordinates stock transfer or substitution → lifecycle resumes once Inventory confirms.
3. **Customer Asks “Where Is My Order?”:** CS looks up order → reads customer-safe timeline → sends AI-drafted reply grounded in verified carrier events → creates Ops handoff if delayed.
4. **B2B PO Awaiting Approval:** B2B order arrives with PO & credit terms → validation routes high-value/Net-term PO to Order Manager → Manager reviews & approves → proceeds to reservation and fulfilment.
5. **Cancellation After Picking Starts:** CS logs cancellation request → Order Ops checks fulfilment stage → sends warehouse stop request if picking started, or routes to return/refusal policy if already shipped → Finance handles refund posting.

---

## 6. Scope & Boundaries with Other ERP Modules

Order Operations owns **order lifecycle orchestration**. It coordinates with—but never replaces—the other ERP services:

| Service | Owns | Order Operations Interaction |
| :--- | :--- | :--- |
| **Inventory & Replenishment** | Stock levels, reservations, replenishment | Requests/checks stock reservation; displays stock blockers. |
| **Warehouse & Fulfilment** | Picking, packing, dispatch handover | Creates fulfilment tasks; displays progress and warehouse delays. |
| **Shipping / Logistics** | Carrier booking, tracking, delivery | Displays tracking & delivery events; retries carrier handovers. |
| **Finance & Reconciliation** | Invoices, ledger, settlements, refunds | Displays payment/refund status; sends approved refund requests. |
| **Customer / CRM** | Customer profile & communication consent | Displays relevant customer context and links to CRM record. |
| **Business Overview** | Executive health, P&L, and top risks | Receives summarized order & SLA risk signals. |

---

## 7. Technical & Implementation Reference

### 7.1 Core Data Model Entities
- **Core Records:** `Order`, `Order Line`, `Order Source`, `Order Status Mapping`
- **Events & Exceptions:** `Order Event` *(immutable, separate event vs. ingestion time, idempotent)*, `Order Exception`, `Work Item`, `Assignment`, `Order Note`
- **Fulfilment & Finance Links:** `Shipment`, `Return / Exchange`, `Approval`, `SLA Policy`, `Audit Record`, `Integration Message`

### 7.2 Integrations & Safeguards
- **Inbound Events:** `OrderCreated`, `PaymentCaptured/Failed`, `ReservationSucceeded/Failed`, `PickingStarted`, `Packed`, `Dispatched`, `DeliveryAttemptFailed`, `ReturnRequested`, `RefundCompleted`, `ChannelSyncFailed`.
- **Outbound Commands:** `CreateOrder`, `RequestReservation`, `CreateFulfilmentTask`, `RequestOrderHold/Release`, `RequestCancellation`, `RequestAddressCorrection`, `CreateReturnRequest`, `RequestRefund` *(Finance-executed)*.
- **Safeguards:** Idempotent APIs with correlation IDs, optimistic concurrency, strict state-transition validation, async bulk processing, dead-letter queues for failed syncs, and graceful degraded mode if a downstream service is unreachable.

### 7.3 Phased Delivery Roadmap
1. **Phase 1 — Order Foundation:** Unified order model, idempotent channel ingestion, normalized lifecycle, Order Explorer search, Detail Workspace, timeline, and core RBAC.
2. **Phase 2 — Command Centre:** Flow Pulse, Live Order Journey, Attention Queue, Workload & Handoffs, SLA clocks, Channel Signals, and role-specific views.
3. **Phase 3 — Operational Actions:** Safe Action Preview, holds/amendments, cancellations, warehouse/inventory coordination, returns/exchanges, and bulk operations.
4. **Phase 4 — Intelligence:** Stagnation Detector, Promise Risk Radar, root-cause insights, and Ask Order Operations AI assistant.

### 7.4 Success Measures & Open Decisions
- **Measure Outcomes:** Dispatch SLA %, delivery promise %, mean time to resolve exceptions, unassigned exception age, manual touches per order, and duplicate import rate.
- **Validate Before Build:** First-release channels, mandatory vs. channel-specific lifecycle stages, SLA targets per tier, specialist auto-approval limits, B2B credit/pricing permissions, and retention policies.
