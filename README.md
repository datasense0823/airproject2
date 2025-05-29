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

## ✅ Human Review Prompt

After the LLM generates and scores the summary, the user should be shown the following prompt:

```text
Do you approve the following summary? (Type 'approve' or 'reject')

Summary:
"<Generated summary here>"

🟢 If Approved
The summary, along with its metadata, should be stored in a PostgreSQL table named approved_summaries.

🗃️ PostgreSQL Table Schema — approved_summaries

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

🔴 If Rejected
Prompt the reviewer for feedback:

Please describe what needs improvement in the summary:
(e.g., "It missed the importance of tax efficiency and portfolio rebalancing.")


Store this feedback and the rejected summary in a separate PostgreSQL table called rejected_summaries.

🗃️ PostgreSQL Table Schema — rejected_summaries

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
Use the collected feedback to regenerate a better summary via the LLM.


📦 Deliverables

1. investment_strategy.docx: Your input document

2. main.py: Full summarization + human review pipeline

3. score_logic.py: Summary scoring and flag checker logic

4. schema.sql: SQL schema for both PostgreSQL tables

5. Snapshot of table data stored in the database (approved & rejected)

6. A short screen recording (1–2 minutes) showing the full flow

7. README.md: This instruction file

📅 Deadline for Submission
🗓 Sunday, June 7 — End of Day (EOD)




