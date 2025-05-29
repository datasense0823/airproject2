# 🧠 AI Project Challenge 2: Human-in-the-Loop Document Summarizer

## 🎯 Objective

Build a **Human-in-the-Loop** document summarization system where:

1. A real financial document is summarized using an LLM.
2. The summary is scored based on defined criteria.
3. A human reviewer approves or rejects the summary.
4. Based on the human decision:
   - ✅ Approved: Store the summary in a PostgreSQL table.
   - ❌ Rejected: Ask for feedback and regenerate the summary using that feedback.


## Input Document (Provided)

Use the file `investment_strategy.pdf` as your source document. It contains approximately 500 words about long-term wealth building via:

- Asset Allocation  
- SIPs (Systematic Investment Plans)  
- Tax-saving strategies  
- Portfolio Rebalancing  
- Emotional discipline in investing

You must parse and summarize this document.


---

## 🧪 Scoring Logic

Each summary must be evaluated and scored before being shown to the human.

| Metric           | Max Score | Description |
|------------------|-----------|-------------|
| Coverage         | 4         | Captures all key ideas from the input document |
| Clarity          | 3         | Easy-to-read, professional tone |
| Language Quality | 3         | Proper grammar, structure, and flow |

**Total Score:** 10

### Additional Flags

- `flagged_uncertain`: Set to `TRUE` if summary includes vague words like `"maybe"`, `"probably"`, `"I think"`.
- `flagged_too_short`: Set to `TRUE` if summary has less than 25 words.

---

## 👩‍⚖️ Human Review Process

After generating and scoring the summary:

### ✅ If approved:


## 👩‍⚖️ Human Review Process

After generating and scoring the summary:

### ✅ If approved:

Do you approve the following summary? (Type 'approve' or 'reject')

Summary:
"<Generated summary here>"

shell
Copy
Edit

### ❌ If rejected:

Please describe what needs improvement in the summary:
(e.g., "It missed the importance of tax efficiency and portfolio rebalancing.")

pgsql
Copy
Edit

Your feedback will be used to regenerate the summary.

---

## 🗃️ PostgreSQL Table Schema

### Table 1: `approved_summaries`

```sql
CREATE TABLE approved_summaries (
    id UUID PRIMARY KEY,
    original_text TEXT NOT NULL,
    summary TEXT NOT NULL,
    score INTEGER NOT NULL CHECK (score BETWEEN 0 AND 10),
    flagged_uncertain BOOLEAN DEFAULT FALSE,
    flagged_too_short BOOLEAN DEFAULT FALSE,
    approved_by TEXT NOT NULL,
    feedback TEXT,
    approved_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Table 2: rejected_summaries
sql
Copy
Edit
CREATE TABLE rejected_summaries (
    id UUID PRIMARY KEY,
    original_text TEXT NOT NULL,
    rejected_summary TEXT NOT NULL,
    score INTEGER NOT NULL,
    flagged_uncertain BOOLEAN DEFAULT FALSE,
    flagged_too_short BOOLEAN DEFAULT FALSE,
    feedback TEXT NOT NULL,
    rejected_by TEXT NOT NULL,
    rejected_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
✅ Deliverables
investment_strategy.docx: Your input document

main.py: Full summarization + review pipeline

score_logic.py: Summary scoring and flag checker

schema.sql: SQL for both PostgreSQL tables

.env: Contains OPENAI_API_KEY, POSTGRES_URL

README.md: This file

🛠 Suggested Tools
Python 3.10+

OpenAI or Claude API

PostgreSQL

psycopg2 or asyncpg

uuid, datetime, dotenv

💡 Bonus Points
Store human feedback in the DB

Allow up to 2 rejection-regeneration cycles

Show top 3 summaries with highest scores

🧠 Real-World Relevance
This project simulates:

Editorial review pipelines

Human-in-the-loop GenAI systems

Enterprise content validation

RAG-based summarization + moderation flow




