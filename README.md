# n8n Automations

Workflow automations built on n8n exploring how TAM operational tasks can be automated at scale.

---

## Customer Account Health Monitor

### What it does

Simulates a daily TAM health check digest across a customer portfolio. Fetches account data via REST API, assigns a health score to each account, routes at-risk accounts through a dedicated escalation branch, and outputs a structured digest with prioritised actions per customer.

### Business context

One of the highest-value TAM activities is proactive risk identification — catching accounts drifting toward churn before they escalate. In practice this often means manually reviewing adoption patterns, support ticket volume, and engagement signals across a portfolio. This workflow automates that triage layer: pulling account data, applying a scoring model, and surfacing the right action for each tier without manual review.

### Workflow structure

| Node | Type | Purpose |
|---|---|---|
| Run health check | Manual Trigger | Initiates the workflow (production version uses Schedule trigger — weekdays 8am) |
| Fetch customer accounts | HTTP Request | Pulls account records from external source via REST API (GET) |
| Add health scores | Code (JavaScript) | Assigns a 0–100 health score and risk tier to each account |
| Route by risk level | IF | Branches on score < 60 (at-risk) vs ≥ 60 (healthy) |
| At Risk | Edit Fields | Flags account: riskLevel = At-Risk, priority = HIGH, action = Schedule urgent check-in |
| Healthy | Edit Fields | Flags account: riskLevel = Healthy, priority = LOW, action = Monthly check-in |
| Merge | Merge (Append) | Recombines both branches into a single item stream |
| Format digest output | Edit Fields | Appends reportType and generatedAt timestamp to each record |

### How to import

1. Download `Customer Account Health Monitor.json` from this repo
2. In n8n, go to **Workflows → Import from file**
3. Select the downloaded JSON
4. Hit **Test workflow** to run

### Extending this workflow

The current version uses [JSONPlaceholder](https://jsonplaceholder.typicode.com) as a mock data source. To productionise:
- Replace the HTTP Request URL with a real CRM endpoint (Salesforce, HubSpot, Gainsight)
- Replace the random health score in the Code node with real signals: days since last login, open P1 ticket count, feature adoption rate, NPS score
- Replace the final Edit Fields node with a Slack or email node to deliver the digest automatically
