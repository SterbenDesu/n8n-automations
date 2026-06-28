Customer Account Health Monitor

An n8n workflow that simulates a daily TAM health check digest. Fetches customer account data via HTTP Request, scores each account (0–100) using a Code node, routes at-risk accounts (score < 60) through a dedicated branch using conditional IF logic, merges both branches, and formats a structured output digest. Built to explore how recurring TAM operational tasks — account monitoring, risk flagging, prioritisation — can be automated at scale.
