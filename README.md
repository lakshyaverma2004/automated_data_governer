# Automated Data Governor (AI-Powered Data Quality Automation)

An **AI-driven data governance workflow built with n8n** that automatically audits PostgreSQL databases, detects data quality issues, proposes SQL fixes, and generates a shareable health report.

The system uses **AI agents, automated approvals, SQL execution, and reporting** to create an end-to-end **data quality management pipeline**.

---

# Architecture Overview

The workflow is divided into **three intelligent stages**:

### 1. Data Investigation

The system scans the PostgreSQL database to detect data quality issues.

Checks include:

* Missing / NULL values
* Placeholder values (N/A, UNKNOWN, etc.)
* Inconsistent formats
* Data type mismatches
* Duplicate rows
* Outliers
* Referential integrity violations
* Timestamp anomalies
* Schema drift

An **AI Data Investigator Agent** analyzes the database schema and returns structured issues.

Output example:

```json
{
 "issue_id": "ISSUE-001",
 "category": "Fixable with SQL",
 "description": "Negative values found in order_amount",
 "sample_values": ["-200", "-45"],
 "suggestion": "Replace negative values with NULL or correct source logic"
}
```

---

# 2. Automated Issue Resolution

Issues classified as **Fixable with SQL** are sent for automated fixing.

Workflow process:

1. Send **email approval request**
2. Wait for human approval
3. AI **Data Issue Fixer Agent** generates SQL
4. SQL executed on PostgreSQL
5. Execution result captured

Example AI output:

```json
{
 "issue_id": "ISSUE-001",
 "status": "resolved",
 "message": "Updated 3 rows with corrected values",
 "executed_sql": "UPDATE orders SET amount = NULL WHERE amount < 0;"
}
```

This ensures **human-in-the-loop governance** before database modification.

---

# 3. Automated Reporting

After all issues are processed:

The system automatically:

1. Generates a **Database Health Report**
2. Creates a **Google Docs report**
3. Shares the document publicly
4. Emails the report link

Report sections include:

* Executive Summary
* Fixed Issues
* Discussion Items
* SQL executed

---

# Workflow Architecture

Main workflow components:

```
Manual Trigger
     │
Master Data Investigator Agent
     │
Issue Classification
     │
Human Approval via Gmail
     │
Data Issue Fixer Agent
     │
SQL Execution (PostgreSQL)
     │
Result Aggregation
     │
Report Generation
     │
Google Docs Report
     │
Email Notification
```

---

# Technologies Used

* **n8n** – workflow automation
* **PostgreSQL** – database system
* **LLM Agents** – AI analysis & SQL generation
* **Groq (Kimi K2)** – investigation agent
* **Google Gemini** – fixing & reporting agents
* **Google Docs API** – automated reporting
* **Gmail API** – approval workflow

---

# Key Features

AI-powered **data quality detection**

Human-in-the-loop **approval system**

Automated **SQL issue fixing**

Structured **JSON validation pipelines**

Automated **Google Docs reporting**

Email-based **data governance workflow**

---

# Project Structure

```
project/
│
├── workflow/
│   └── automated-data-governor.json
│
├── docs/
│   └── architecture.png
│
└── README.md
```

---

# How to Run

### 1. Import Workflow

Import the workflow JSON into **n8n**.

```
Settings → Import Workflow
```

---

### 2. Configure Credentials

Add credentials for:

* PostgreSQL
* Gmail OAuth
* Google Docs API
* Google Drive API
* Groq API
* Gemini API

---

### 3. Configure Database Access

Ensure PostgreSQL allows schema inspection.

The workflow runs this query:

```sql
SELECT
table_name,
column_name,
data_type,
is_nullable
FROM information_schema.columns
WHERE table_schema = 'public';
```

---

### 4. Execute Workflow

Click:

```
Execute Workflow
```

The pipeline will:

1. Audit the database
2. Send approval requests
3. Apply SQL fixes
4. Generate a health report

---

# Example Output

Final email includes:

```
Database Health Check Report

Total Issues: 12
Resolved: 8
Needs Discussion: 4

Report Link:
https://docs.google.com/...
```

---

# Future Improvements

* Slack / Teams approval integration
* Data lineage tracking
* ML-based anomaly detection
* Dashboard for governance metrics
* Support for multiple databases

---

# Author

**Lakshya Verma**

Computer Science (AI) Student

