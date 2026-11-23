# Business Overview
# E-commerce ERP --- Business Overview

## Product and Functional Specification

**Document type:** Product planning and prototype specification\
**Module:** Business Overview\
**Status:** Initial proposal; requires stakeholder validation

------------------------------------------------------------------------

## 1. Purpose and product principles

The Business Overview is the central business-intelligence entry point
for an e-commerce ERP. It helps authorized users understand business
performance, identify issues, evaluate opportunities, and navigate to
the operational modules where work is completed.

It summarizes information from other ERP modules; it does not replace
Order Operations, Warehouse and Fulfilment, Inventory and Replenishment,
Finance and Reconciliation, or Store / POS.

### Product principles

-   **Decision-oriented:** prioritize information users need to
    understand or decide.
-   **Contextual:** explain why a metric or alert matters.
-   **Actionable:** link insights to the correct ERP workflow.
-   **Role-aware:** adapt information and actions to permissions.
-   **Explainable:** expose KPI definitions, sources, periods, and
    limitations.
-   **Reliable:** distinguish actual, estimated, forecast, incomplete,
    and stale data.
-   **Configurable:** support different business models, stores,
    channels, and preferences.

The dashboard should answer: 1. How is the business performing? 2. What
changed during the selected period? 3. What needs attention now? 4.
Where might the business be losing money or efficiency? 5. What risks
and opportunities should be considered? 6. Where can an authorized user
investigate or take action?

------------------------------------------------------------------------

## 2. Roles in scope

The company role structure identifies four roles served by Business
Overview.

  -----------------------------------------------------------------------
  Role                    Primary responsibility  Main information need
  ----------------------- ----------------------- -----------------------
  Business Owner          Overall business        Is the business healthy
                          performance and         and performing well?
                          direction               

  E-commerce Manager      E-commerce performance  What is happening
                          and operational         across e-commerce, and
                          coordination            what needs improvement?

  Commerce Admin          Commerce environment,   Are commerce systems,
                          configurations,         connections, and data
                          integrations, and data  functioning correctly?
                          operations              

  Commerce Viewer         Read-only business      What is happening in
                          monitoring              the business?
  -----------------------------------------------------------------------

These are proposed responsibility interpretations. The organization
should confirm exact job boundaries, especially the distinction between
E-commerce Manager and Commerce Admin.

**Security principle:** administrative access must not automatically
grant access to confidential financial or customer information. System
administration and data confidentiality are separate permissions.

------------------------------------------------------------------------

## 3. Business Overview information architecture

The proposed overview contains ten interconnected functional areas.

  -----------------------------------------------------------------------
  \#                      Functional area         Purpose
  ----------------------- ----------------------- -----------------------
  1                       Business Health         Summarize the condition
                                                  of the business

  2                       Sales Performance       Show sales, revenue,
                                                  order, and growth
                                                  trends

  3                       Order Intelligence      Summarize order status,
                                                  exceptions, and trends

  4                       Financial Intelligence  Present revenue,
                                                  profitability, costs,
                                                  and cash-related
                                                  information

  5                       Inventory Intelligence  Explain stock health,
                                                  inventory value, and
                                                  capital efficiency

  6                       Customer Intelligence   Show customer mix and
                                                  purchasing behaviour

  7                       Channel Performance     Compare connected
                                                  commerce channels

  8                       Operational Health      Summarize fulfilment,
                                                  delivery, and returns
                                                  performance

  9                       Business Alerts         Surface important
                                                  exceptions and
                                                  notifications

  10                      Business Intelligence   Present opportunities,
                                                  risks, forecasts, and
                                                  AI-assisted
                                                  explanations
  -----------------------------------------------------------------------

These are sections of one Business Overview experience, not necessarily
ten separate pages. Detailed investigation should open the relevant
module or a contextual detail view.

------------------------------------------------------------------------

## 4. Functional area specifications

### 4.1 Business Health

**Purpose:** Give users a concise view of overall business condition.

Potential content: - Overall business health indicator. - Growth and
performance signals. - Financial, sales, operational, and inventory
signals. - Positive developments and areas requiring attention. -
Comparison with a previous period. - Explanation of factors contributing
to the status.

A composite health score should only be introduced after its dimensions,
weights, thresholds, and missing-data behaviour are defined and
validated.

Illustrative scoring framework:

  Dimension                Example weight
  ---------------------- ----------------
  Financial health                    25%
  Sales performance                   20%
  Order operations                    20%
  Inventory efficiency                20%
  Customer performance                15%

These weights are examples, not approved business rules. The score
should be deterministic and explainable, not generated directly by an
LLM. If data is incomplete, show the limitation rather than a misleading
score.

### 4.2 Sales Performance

Potential metrics and views: - Gross sales and net sales. - Order count
and average order value (AOV). - Sales target and achievement, if
configured. - Daily, weekly, monthly, and custom-period trends. -
Current period versus previous comparable period. - Revenue by product,
category, store, and channel where available. - Sales growth and changes
in order volume.

Explanations should identify drivers where supported by data. Revenue
growth accompanied by declining margin should be presented as a mixed
result, not an unqualified success.

### 4.3 Order Intelligence

The overview summarizes order activity without duplicating the full
Order Operations workflow.

Potential content: - Total orders and orders by status. - Pending or
unprocessed orders. - Cancelled orders and cancellation trends. -
Delayed orders. - Orders awaiting fulfilment or dispatch. - Order
exceptions requiring attention. - Value associated with affected orders,
where its calculation is defined.

Every exception should link to a filtered order list in Order
Operations. Complex order processing remains in that module.

### 4.4 Financial Intelligence

Financial concepts must remain distinct. Sales revenue, recognized
revenue, profit, cash received, receivables, and cash availability are
not interchangeable.

Potential content: - Gross sales and net sales. - Discounts, refunds,
and returns. - Cost of goods sold (COGS). - Gross profit and gross
margin, when cost data is reliable. - Operating expenses, when connected
and classified. - Payments received. - Outstanding or unsettled amounts,
where supported. - Estimated capital tied up in inventory. - Potential
cost or margin leakage.

Potential money-leakage signals: - Excessive discounting. - High return
rates. - Increased shipping costs. - Payment gateway charges. - Margin
deterioration. - Slow-moving inventory. - Unusual expense changes.

Each figure must identify whether it is actual, estimated, accrued,
forecast, or incomplete. Financial formulas and recognition rules
require approval from finance/business stakeholders.

### 4.5 Inventory Intelligence

Connect inventory status to availability, demand, and capital rather
than showing only stock totals.

Potential content: - Inventory quantity and value. - Low-stock products
and stockout risk. - Fast-moving and slow-moving products. - Dead stock
using a configurable inactivity period. - Excess stock. - Inventory
turnover and sell-through rate. - Replenishment signals. - Estimated
capital tied up in slow-moving stock.

Possible capital-efficiency groups:

1.  High demand and healthy stock.
2.  High demand and low stock.
3.  Low demand and excess stock.
4.  Low demand and healthy stock.

Inventory value requires a defined cost basis. Dead-stock thresholds
should be configurable and visible.

### 4.6 Customer Intelligence

Potential content: - New and returning customers. - Repeat purchase
rate. - Average order value by customer segment. - Purchase frequency. -
Customer trends over time. - Retention or reactivation signals, where
sufficient history exists.

Customer-level personal information should not appear in aggregate
overview widgets unless the user's permissions allow it and the detail
is necessary.

### 4.7 Channel Performance

Potential content: - Revenue and orders by channel. - Channel growth or
decline. - Returns and cancellations by channel. - Channel contribution
to total sales. - Channel profitability, only when channel costs and
fees are available. - Connection and data-freshness status.

The exact channel list depends on integrations enabled for the
organization.

### 4.8 Operational Health

Potential content: - Order processing performance. - Fulfilment
progress. - Dispatch delays. - Delivery performance and failed-delivery
signals. - Return volume and rate. - Pending returns or refunds. -
Operational bottlenecks.

Show the business impact and route users to the responsible operational
module.

### 4.9 Business Alerts

Alerts should be prioritized and contextual, not an unfiltered
notification feed.

Suggested severity levels: - **Critical:** requires prompt attention. -
**Attention:** needs review or follow-up. - **Informational:** useful
status or change without immediate action.

Each alert should contain, where applicable: - Clear title and
description. - Severity and timestamp. - Affected store, channel,
product, or process. - Evidence or source records. - Estimated business
impact, if reliably calculated. - Suggested next step. - Link to the
relevant ERP module. - Owner or assignee, if alert assignment is
implemented.

### 4.10 Business Intelligence: Opportunities, Risks, and AI Advisor

Potential opportunities: - Products with increasing demand and
insufficient stock. - Products with high sales but low margins. -
Potential product bundles. - Categories showing growth. - Customer
segments with changing purchase behaviour.

Potential risks: - Increasing cancellations or returns. - Declining
profitability. - Stockout exposure. - Excess inventory. - Fulfilment
delays. - Supplier or channel concentration, if relevant data exists.

Each recommendation should show its reason, supporting data,
assumptions, estimated impact, evidence quality, and next permitted
action. Consequential actions require authorization and, where
appropriate, approval.

#### AI Business Advisor

The AI Advisor may answer questions such as: - Why did profit or margin
change during this period? - Which products contribute most to sales? -
Which products have low stock and high demand? - What is contributing to
delayed orders? - How does this period compare with the previous period?

It must use authorized, current business data and enforce the same
organization, store, role, and data-scope restrictions as the dashboard.
Where feasible, expose the underlying metrics or records used.

------------------------------------------------------------------------

## 5. Role-specific dashboard experiences

Use **one Business Overview engine with role-adaptive layouts**, rather
than four independently maintained dashboards.

### 5.1 Business Owner

**Objective:** Understand overall business performance and make business
decisions.

Recommended priorities: 1. Business Health. 2. Revenue and
profitability. 3. Business growth trajectory. 4. Capital tied up in
inventory. 5. Cross-store and channel performance. 6. Major operational
risks. 7. Strategic opportunities and AI explanations.

Potential access across all ten areas, subject to organization scope and
explicit permissions.

### 5.2 E-commerce Manager

**Objective:** Monitor e-commerce performance and coordinate
improvements.

Recommended priorities: 1. Sales performance and targets. 2. Order
trends and exceptions. 3. Product and channel performance. 4. Inventory
and stockout risks. 5. Customer trends. 6. Fulfilment and returns
performance. 7. Discount and return impact. 8. Actionable performance
insights.

Financial details may be restricted independently from sales summaries.
This role should not automatically receive full accounting or sensitive
financial access.

### 5.3 Commerce Admin

**Objective:** Maintain the commerce environment, integrations,
configurations, and data operations.

Recommended priorities: 1. Channel connectivity. 2. Data synchronization
status. 3. Import and processing status. 4. Data quality and validation
errors. 5. Missing or inconsistent records. 6. Integration and system
alerts. 7. Configuration status. 8. User or permission administration,
only if separately granted.

Commerce Admin should see business metrics only to the extent required
and authorized. Technical administration does not imply unrestricted
access to profit, payment, or personal customer data.

### 5.4 Commerce Viewer

**Objective:** Monitor business performance without changing business
data.

Recommended priorities: 1. Business performance summary. 2. Sales
trends. 3. Order trends and status distribution. 4. Inventory summary.
5. Channel performance. 6. Operational summaries. 7. Published business
insights.

Viewer access is read-only by default. Users may interact with permitted
filters, charts, and detail views. They cannot perform write actions,
configure integrations, manage permissions, or export restricted data
unless separately authorized.

------------------------------------------------------------------------

## 6. Initial role-permission matrix

Legend: **F** = Full, **V** = View, **L** = Limited, **N** = No access.

  -------------------------------------------------------------------------------
  Business        Business Owner    E-commerce    Commerce Admin  Commerce Viewer
  Overview                            Manager                     
  feature                                                         
  --------------- --------------- --------------- --------------- ---------------
  Business Health        F               F               L               V

  Sales                  F               F               L               V
  Performance                                                     

  Order                  F               F               L               V
  Intelligence                                                    

  Financial              F               L               L               L
  Intelligence                                                    

  Inventory              F               F               L               V
  Intelligence                                                    

  Customer               F               F               L               V
  Intelligence                                                    

  Channel                F               F               F               V
  Performance                                                     

  Operational            F               F               F               V
  Health                                                          

  Business Alerts        F               F               F               V

  Business               F               F               L               V
  Intelligence                                                    

  AI Business            F               F               L               L
  Advisor                                                         

  Integration            V               V               F               N
  Monitoring                                                      

  Dashboard              F               L               F               N
  Configuration                                                   

  User and               F               N          Conditional          N
  Permission                                                      
  Management                                                      
  -------------------------------------------------------------------------------

This is a proposed starting point, not a final authorization policy.
Financial visibility, customer-data visibility, exports, and Commerce
Admin's user-management authority require explicit approval.

### Permission dimensions

1.  **Functional permission:** what feature or resource the user can
    access.
2.  **Data scope:** which organization, store, region, channel, or
    records the user can access.
3.  **Action permission:** whether the user can view, create, update,
    approve, export, or delete.

Example: an E-commerce Manager may view sales across the organization
but manage only assigned stores.

------------------------------------------------------------------------

## 7. RBAC and ABAC implementation

Combine:

-   **RBAC (Role-Based Access Control):** baseline permissions based on
    a role.
-   **ABAC (Attribute-Based Access Control):** conditions such as
    organization, store, region, department, record ownership, and data
    sensitivity.

Illustrative permission object:

``` json
{
  "role": "ecommerce_manager",
  "permissions": {
    "business_overview": { "view": true },
    "sales_performance": { "view": true, "export": true },
    "financial_intelligence": {
      "view": true,
      "view_profit": false
    },
    "inventory_intelligence": { "view": true },
    "integration_monitoring": { "view": false }
  },
  "data_scope": {
    "organization_id": "org_001",
    "store_ids": ["store_01", "store_02"]
  }
}
```

This is illustrative, not a production-ready authorization
implementation.

### Security requirements

-   Enforce authorization on the server/API and data-access layer, not
    only in the UI.
-   Deny access by default.
-   Validate organization and store scope on every relevant request.
-   Do not trust client-supplied role names or organization IDs.
-   Apply field-level restrictions to sensitive financial and customer
    data.
-   Restrict exports separately from on-screen viewing.
-   Record permission changes and sensitive actions in audit logs.
-   Support approval workflows for consequential actions.
-   Ensure AI retrieval and tool calls enforce the same access controls.
-   Test cross-tenant and cross-store access isolation.

------------------------------------------------------------------------

## 8. Business-model-aware metrics

Support configurable metrics based on the organization's business model.
Avoid hardcoding a separate dashboard for every model.

  -----------------------------------------------------------------------
  Business model                      Potential additional metrics
  ----------------------------------- -----------------------------------
  D2C                                 Customer acquisition cost (CAC),
                                      lifetime value (LTV), repeat
                                      purchases, conversion

  B2C                                 Customer demand, order frequency,
                                      sales growth

  B2B                                 Outstanding receivables, bulk
                                      orders, credit exposure

  Marketplace seller                  Marketplace fees, commissions,
                                      returns, channel contribution

  Multi-store                         Store-level revenue, inventory
                                      distribution, store comparisons
  -----------------------------------------------------------------------

Only show a metric when its required data is connected and sufficiently
complete. For example, CAC requires reliable acquisition-cost and
attribution data; LTV requires an agreed calculation and sufficient
customer history.

------------------------------------------------------------------------

## 9. Dashboard interactions and navigation

  -----------------------------------------------------------------------
  Interaction                         Expected behaviour
  ----------------------------------- -----------------------------------
  Select Business Health              Open contributing dimensions and
                                      data quality

  Select Revenue                      Open detailed sales analysis

  Select Delayed Orders               Open Order Operations with relevant
                                      filters

  Select Low Stock                    Open affected products in Inventory
                                      and Replenishment

  Select Dead Stock                   Open the slow-moving inventory list

  Select Financial Alert              Open permitted financial detail

  Select Opportunity                  Show evidence, assumptions, impact,
                                      and recommendation

  Select Recommended Action           Open the relevant workflow, subject
                                      to permissions

  Change date range                   Refresh applicable metrics
                                      consistently

  Change store                        Refresh data within permitted store
                                      scope

  Open AI Advisor                     Answer using authorized business
                                      context

  Open KPI definition                 Show formula, source, period, and
                                      limitations
  -----------------------------------------------------------------------

Every important metric should have a traceable definition and a route to
supporting detail when permitted.

------------------------------------------------------------------------

## 10. KPI governance and metric definitions

Maintain a KPI catalogue. Each KPI should define: - Display name and
description. - Business meaning. - Formula and inclusion/exclusion
rules. - Source module and source fields. - Currency and timezone
rules. - Date basis (order date, payment date, fulfilment date, or
another approved event). - Aggregation level. - Refresh frequency. -
Required permissions. - Treatment of refunds, cancellations, taxes,
shipping, and discounts. - Behaviour when source data is missing or
stale. - Whether the value is actual, estimated, or forecast.

### Initial KPI catalogue

  -----------------------------------------------------------------------
  KPI                     Likely source           Decisions required
  ----------------------- ----------------------- -----------------------
  Gross Sales             Orders / sales records  Treatment of tax,
                                                  shipping, cancellations

  Net Sales               Orders, discounts,      Agreed deductions and
                          returns                 recognition rules

  Order Count             Orders                  Included statuses and
                                                  date basis

  AOV                     Orders and sales        Numerator and
                          metrics                 eligible-order
                                                  denominator

  Gross Profit            Net sales and COGS      Reliable product cost
                                                  data

  Gross Margin            Gross profit and net    Zero or negative net
                          sales                   sales handling

  Refund Amount           Refund / return records Initiated versus
                                                  completed refunds

  Cash Received           Payment records         Captured, settled, and
                                                  paid-out distinctions

  Inventory Value         Inventory and cost      Valuation method and
                          records                 stock-location scope

  Dead Stock Value        Inventory and sales     Inactivity period and
                          history                 valuation basis

  Return Rate             Orders and returns      Cohort and denominator

  Repeat Purchase Rate    Customer and order      Customer identity and
                          history                 observation window
  -----------------------------------------------------------------------

Ambiguous financial formulas must be agreed with finance and business
stakeholders before implementation.

------------------------------------------------------------------------

## 11. Data and technical architecture

The overview should consume governed metrics rather than repeatedly
querying large operational datasets from the browser.

``` text
DATA SOURCES
  Shopify / marketplaces / CSV / Excel / manual entry
                 |
                 v
          DATA INGESTION
                 |
                 v
       VALIDATION AND NORMALIZATION
                 |
                 v
       OPERATIONAL SOURCE OF TRUTH
       Orders / Products / Inventory /
       Customers / Payments / Returns
                 |
                 v
          ANALYTICS PIPELINE
       KPIs / Aggregates / Events
                 |
                 v
          OVERVIEW API LAYER
                 |
                 v
       SERVER-SIDE AUTHORIZATION
        Role + Scope + Permissions
                 |
                 v
          ROLE-ADAPTIVE UI
                 |
                 v
       AI / INSIGHT SERVICES
```

### Suggested implementation approach

For the prototype, use the existing application stack and database where
practical. If the current system uses Next.js and Firebase, retain them
unless a measured requirement justifies a change.

Recommended services: 1. **KPI calculation service:** deterministic,
tested metric calculations. 2. **Analytics summary service:**
precomputed aggregates by period, store, and channel. 3. **Business
Overview API:** returns data required by the requested view. 4.
**Authorization service:** checks role, data scope, and action
permissions. 5. **Alert and insight service:** creates prioritized,
evidence-backed signals. 6. **AI context service:** retrieves authorized
metrics and supporting records.

### Illustrative data organization

``` text
organizations/{orgId}/
  users/{userId}
  stores/{storeId}
  analytics/
    overview
    daily_metrics
    monthly_metrics
    financial_summary
    inventory_summary
    operational_summary
  business_health/
    current_score
    historical_scores
  insights/{insightId}
  permissions/{roleId}
  audit_logs/{logId}
```

The actual schema must align with the existing ERP data model.
Operational collections remain the source of truth; analytics documents
are derived summaries and should be rebuildable.

### Performance and reliability

-   Avoid loading all orders or inventory records into the browser to
    calculate overview metrics.
-   Use pagination for detail views.
-   Precompute frequently used aggregates.
-   Use incremental updates or scheduled recalculation where
    appropriate.
-   Debounce user-driven filters.
-   Make metric refresh times visible.
-   Handle duplicate events and retries safely.
-   Ensure large imports do not block dashboard rendering.
-   Monitor API latency, calculation failures, and stale aggregates.

------------------------------------------------------------------------

## 12. AI and model architecture

AI supports analysis and explanation; it does not replace deterministic
business logic.

  -----------------------------------------------------------------------
  Component                           Responsibility
  ----------------------------------- -----------------------------------
  Deterministic Analytics Engine      Calculate KPIs and business metrics

  Forecasting Model                   Forecast demand, sales, or stockout
                                      risk when data is sufficient

  Anomaly Detection                   Identify unusual changes in
                                      business activity

  RAG                                 Retrieve relevant authorized
                                      business context and records

  LLM                                 Explain findings and answer
                                      natural-language questions

  Agent Orchestrator                  Coordinate permitted multi-step
                                      workflows
  -----------------------------------------------------------------------

### Safeguards

-   Never use an LLM as the source of truth for arithmetic or financial
    totals.
-   Supply validated metrics and their definitions to the LLM.
-   Enforce permissions before retrieval, not only after generation.
-   Show the period, source, and freshness of the data used.
-   Distinguish observed facts from estimates and predictions.
-   Include confidence or evidence quality where meaningful.
-   Require approval for consequential actions.
-   Log AI recommendations and approved actions.
-   Avoid predictions when history is insufficient or data quality is
    poor.

------------------------------------------------------------------------

## 13. Prototype scope and implementation sequence

The prototype should demonstrate a coherent experience using realistic
demo data. It must not imply that unimplemented forecasting or
integrations are production-ready.

### Core prototype scope

  -----------------------------------------------------------------------
  Area                    Prototype deliverable   Priority
  ----------------------- ----------------------- -----------------------
  Business Health         Health summary and      Must have
                          explainable signals     

  Sales Performance       Sales, orders, AOV,     Must have
                          period comparisons      

  Order Intelligence      Status summaries and    Must have
                          exception drill-down    

  Financial Intelligence  Clearly labelled        Must have
                          metrics and assumptions 

  Inventory Intelligence  Low stock, slow-moving  Must have
                          stock, inventory value  

  Customer Intelligence   New/returning customer  Must have
                          summaries               

  Channel Performance     Channel comparison      Must have
                          using demo data         

  Operational Health      Fulfilment, delivery,   Must have
                          and return summaries    

  Business Alerts         Severity, evidence, and Must have
                          navigation              

  Business Intelligence   Example opportunities   Must have
                          and risks               

  Role-adaptive views     Four role experiences   Must have

  Permissions             Feature, data-scope,    Must have
                          and action checks       

  AI Advisor              Contextual demo Q&A,    Prototype
                          clearly labelled        

  Advanced forecasting    Foundation or clearly   Later phase
                          labelled mock           

  Custom widgets          Configuration           Later phase
                          foundation              
  -----------------------------------------------------------------------

### Suggested implementation sequence

1.  Define KPI catalogue, data assumptions, and role permissions.
2.  Establish design system, navigation, header, filters, and page
    structure.
3.  Build Business Health and Sales Performance.
4.  Build Order Intelligence, Financial Intelligence, and Inventory
    Intelligence.
5.  Build Customer Intelligence, Channel Performance, and Operational
    Health.
6.  Build Alerts, Opportunities, and AI Advisor prototype.
7.  Implement role-adaptive layouts and server-side authorization.
8.  Add loading, empty, error, stale-data, and partial-data states.
9.  Test roles, scopes, filters, and drill-down interactions.
10. Review usability and refine the visual hierarchy.

------------------------------------------------------------------------

## 14. Required prototype test scenarios

Test at least these situations:

1.  Organization with no sales data.
2.  Organization with a small number of orders.
3.  Organization with a large order history.
4.  Organization with multiple stores.
5.  Organization with delayed orders.
6.  Organization with cancellations and returns.
7.  Organization with missing product cost data.
8.  Organization with substantial slow-moving inventory.
9.  User with restricted financial permissions.
10. User assigned to only one store.
11. Commerce Admin with a failed integration.
12. Commerce Viewer attempting a write action.
13. User attempting to access another organization's data.
14. Stale or partially synchronized source data.
15. AI question requesting information outside the user's permissions.

The prototype should demonstrate loading, empty, error, stale-data,
partial-data, permission-denied, and responsive states.

------------------------------------------------------------------------

## 15. Open decisions requiring stakeholder confirmation

1.  Exact responsibilities and authority of E-commerce Manager versus
    Commerce Admin.
2.  Whether Business Overview supports multiple legal entities,
    organizations, stores, and currencies.
3.  Which commerce channels and integrations are in scope.
4.  Approved definitions for gross sales, net sales, recognized revenue,
    profit, refunds, and cash received.
5.  Which roles can view profit, expenses, payment information, and
    customer-level data.
6.  Whether Commerce Admin can manage users and permissions, and under
    what delegation.
7.  Which actions can be initiated from Business Overview versus only
    from operational modules.
8.  Business Health score dimensions, weights, and thresholds.
9.  Alert severity rules, ownership, and escalation.
10. Data refresh expectations and acceptable staleness.
11. Business-model configuration and supported KPI variants.
12. AI provider, data handling, retention, and audit requirements.

------------------------------------------------------------------------

## 16. Recommended product decisions

  -----------------------------------------------------------------------
  Area                                Recommendation
  ----------------------------------- -----------------------------------
  Product experience                  Business Intelligence Command
                                      Center

  Dashboard architecture              One engine with role-adaptive
                                      layouts

  Functional structure                Ten interconnected overview areas

  Access control                      RBAC + ABAC + action-level
                                      permissions

  Data handling                       Governed metrics and precomputed
                                      aggregates

  Source of truth                     Existing operational ERP data

  AI approach                         Analytics + anomaly detection +
                                      forecasting + RAG + LLM

  Business support                    Configurable D2C, B2C, B2B,
                                      marketplace, and multi-store
                                      metrics

  Navigation                          Summary in Business Overview;
                                      detailed workflows in their own
                                      modules

  Reliability                         Explainable KPIs, freshness
                                      indicators, and explicit
                                      incomplete-data states
  -----------------------------------------------------------------------

### Three foundational capabilities

1.  **Business Health Engine:** provides a consistent and explainable
    view of business condition.
2.  **Business Impact Engine:** explains the operational or financial
    significance of an issue.
3.  **Action Intelligence Engine:** turns evidence-backed insights into
    permission-controlled next steps.

The dashboard should be judged not by the number of widgets it contains,
but by whether each authorized user can quickly understand the business,
identify what matters, trust the information, and reach the correct
workflow.

------------------------------------------------------------------------

**Document note:** This is an initial product specification based on the
Business Overview discussion. Role mappings, financial definitions,
security policy, and implementation details remain proposals until
validated by the company's product, business, finance, and engineering
stakeholders.
