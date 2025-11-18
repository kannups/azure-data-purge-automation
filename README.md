🚀 Automated Data Purge System Using Azure Function App & Logic Apps

This repository contains a fully automated and serverless data purge workflow built on Microsoft Azure, designed to purge stale records from a PostgreSQL database and automatically send purge reports by email via Outlook 365.

The entire process is fully automated, secure, and scheduled to run weekly.

🌟 Architecture Summary
Azure Function App (Timer Trigger)
        ↓
Connects to PostgreSQL & executes purge logic
        ↓
Generates purge summary (counts, stats, metadata)
        ↓
Uploads report to Azure Blob Storage
        ↓
Blob-triggered Azure Logic App
        ↓
Sends email through Outlook 365 with report attached

🧩 Components Used
🔹 Azure Function App

Timer trigger executes every Friday at 10 PM CST

Connects to PostgreSQL using Key Vault secrets or Managed Identity

Performs:

Data scan

Purge based on business rules

Before/after row counts

Logging & validation

Uploads purge summary to Blob Storage

🔹 PostgreSQL Database

Stores operational data

Purge criteria (examples):

Records older than X days

Records matching business flags

Designed for safe deletion (batch delete)

🔹 Azure Blob Storage

Stores purge summary reports (CSV/JSON/TXT)

Acts as historical audit trail

Blob event triggers the Logic App

🔹 Azure Logic Apps

Trigger: “When a blob is added or modified”

Reads the uploaded report

Sends an email to Outlook 365 with:

Report attached

Execution metadata in email body

🔹 Outlook 365

Receives automated purge summary

Used by:

Operations team

Business stakeholders

Compliance & audit teams

⏱️ Scheduling Details

Weekly purge:
🗓 Friday
⏰ 10:00 PM CST
⚙️ Function App Timer (CRON):

0 0 22 ? * FRI *

📊 Purge Report Includes

Table name

Total rows scanned

Eligible rows

Deleted rows

Timestamp start/end

Execution duration

Error logs (if any)

🔐 Security Highlights

No credentials in code

Azure Key Vault secrets or Managed Identity

PostgreSQL protected with Private Endpoints

RBAC-enabled access

Application Insights for telemetry

🏆 Benefits

Fully automated, no manual intervention

Reduces DB size and improves performance

Ensures compliance with data retention policies

Provides a clear audit trail via storage + email

Scales efficiently and cost-effectively
