# Data Sense Presents: AI Project Challenge 2: Human-in-the-Loop Document Summarizer

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


## Scoring Logic

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


## AFTER LLM CALL, USER SHOULD BE PROMPTED BELOW QUESTION

**Do you approve the following summary? (Type 'approve' or 'reject')**

Summary:
"<Generated summary here>"

### ✅ If approved: Store Information in a Postrges SQL Table


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


##  ❌ If rejected: Ask for a Question and Store Rejected summaries in POSTGRES Table

Please describe what needs improvement in the summary:
(e.g., "It missed the importance of tax efficiency and portfolio rebalancing.")


Your feedback will be used to regenerate the summary.

**Table 2: rejected_summaries**

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

#  Deliverables
1. investment_strategy.docx: Your input document
2. main.py: Full summarization + review pipeline
3. score_logic.py: Summary scoring and flag checker
4. schema.sql: SQL for both PostgreSQL tables
5. Snapshot of Table Storage
6. A Short video showing the whole Process
7. README.md: This file


# Deadline for Submission: Sunday June 7 EOD




