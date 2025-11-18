# Automated Data Purge System Using Azure Function App & Logic Apps

This repository contains an automated data purge solution built on **Azure Functions**, **Azure Database for PostgreSQL**, **Azure Storage (Blob)**, and **Azure Logic Apps**.

The system:

1. Identifies and **purges old records** from a PostgreSQL database based on configurable retention rules.  
2. Generates a **purge summary report** and stores it in an Azure Storage Blob container.  
3. Triggers an **Azure Logic App** whenever a new purge report is created/updated and sends an **email via Outlook 365** with the report attached.  
4. Runs automatically on a **weekly schedule (every Friday at 10 PM CST)** for hands-free operations.

---

## Table of Contents

- [Architecture Overview](#architecture-overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Repository Structure](#repository-structure)
---

## Architecture Overview

**High-level flow:**

1. **Timer-triggered Azure Function**
   - Runs every **Friday at 10 PM CST**.
   - Connects to **Azure Database for PostgreSQL (Flexible Server)**.
   - Identifies records older than a configured threshold (e.g., **> 90 days**).
   - Purges eligible records from one or more tables.
   - Generates a **summary report** (JSON/CSV/text) capturing:
     - Tables processed  
     - Row counts before and after purge  
     - Number of rows deleted  
     - Execution start/end time  
     - Status (Success/Failure, Error details if any)
   - Writes the report to an **Azure Blob Storage** container.

2. **Azure Logic App (Blob Trigger)**
   - Triggered whenever a new blob is **added or modified** in the purge report container.
   - Retrieves the purge report.
   - Sends an **email via Outlook 365**, attaching the report and including a summary in the email body.

---

## Features

- ✅ Configurable data retention (e.g., delete records older than N days)
- ✅ Supports multiple tables and schemas
- ✅ Automated **weekly schedule** (no manual intervention)
- ✅ Detailed **purge summary report** stored in Blob Storage
- ✅ Email notification with **attached report** via Logic App + Outlook 365
- ✅ Centralized logging and monitoring via **Application Insights**
- ✅ Secure configuration using **Azure Key Vault** (recommended)

---

## Technology Stack

- **Azure Function App**
  - Trigger: Timer
  - Language: `C#` / `Python` / `Node.js` (choose as per your implementation)
- **Azure Database for PostgreSQL**
  - Flexible Server (recommended) or Single Server
- **Azure Storage Account**
  - Blob Container for purge reports
- **Azure Logic Apps**
  - Trigger: When a blob is added or modified (Blob Storage)
  - Action: Outlook 365 – Send email with attachment
- **Azure Key Vault** (optional but recommended for secrets)
- **Application Insights** for monitoring and logging
- **Azure DevOps / GitHub Actions** (optional) for CI/CD

---

## Repository Structure

