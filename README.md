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



