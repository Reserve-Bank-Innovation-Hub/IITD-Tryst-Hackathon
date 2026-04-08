# Phase 1 — Submission Guidelines

## What You Need to Submit

A single **EDA report** (Jupyter Notebook(in .md format) or PDF) documenting your exploration and analysis of the dataset.

**You do NOT need to submit predictions in this phase.** Phase 1 is purely about demonstrating your understanding of the data. Selected candidates will advance to Phase 2, where predictions on `test_accounts.csv` will be required.

---

## Report Expectations

Your report should walk us through your analysis of the dataset — what you explored, what you found, and what you think it means. We want to see how you think, not how well you can make charts look.

Specifically, your report should cover:

1. **Data Understanding** — How well do you understand the structure of the dataset? Show that you've explored the tables, understood the relationships between them, and identified what each field represents in a banking context.

2. **Statistical Analysis** — Distributions, correlations, class-wise comparisons, handling of class imbalance in your analysis. Are your comparisons valid? Did you avoid misleading aggregations?

3. **Pattern Identification** — What distinguishes mule accounts from legitimate ones? Refer to the known mule behavior patterns listed in `README.md` — did you find evidence of any of them in the data? Did you discover anything we didn't list?

4. **Feature Engineering Plan (Required)** — You must include a concrete list of features you plan to build for Phase 2 modelling. For each proposed feature, explain:
   - What it captures and how it would be computed
   - Why you believe it would help distinguish mule accounts from legitimate ones (back this with evidence from your EDA)
   - Which tables/columns it draws from

   This is a mandatory part of your report. You may add more features in Phase 2, but your Phase 1 report must show that you've thought about the feature engineering pipeline and have a clear plan going in.

5. **Data Quality Observations** — Missing values, noise in labels, anomalies, potential data leakage concerns. Did you notice anything that would affect modelling decisions?

6. **Critical Reasoning** — Do you question your own findings? Do you consider alternative explanations? Do you note limitations?

---

## What We Care About (and What We Don't)

**Matters:**
- Logical soundness of your analysis
- Depth of exploration — did you go beyond surface-level summary statistics?
- Evidence-backed observations — claims supported by numbers or plots
- Clear reasoning — can we follow your thought process?
- Awareness of pitfalls (leakage, class imbalance, noisy labels)

**Does NOT matter:**
- Visual polish — a clean matplotlib plot is just as good as a styled Plotly dashboard
- Length — a focused 15-page report beats a padded 50-page one
- Fancy libraries or tools — use whatever you're comfortable with
- Perfect grammar — clarity matters, not prose quality

---

## Evaluation Rubric

| Criteria | Weight | What We Look For |
|---|---|---|
| Data Understanding | 15% | Correct joins, awareness of schema, data types, relationships between tables |
| Analytical Depth | 20% | Going beyond `.describe()` — meaningful comparisons, proper handling of imbalance, statistical validity |
| Pattern Identification | 20% | Identifying behavioral differences between mule and legitimate accounts, evidence of known patterns in the data |
| Feature Engineering Plan | 20% | A concrete, well-reasoned list of features you intend to build — backed by EDA findings, not just guesswork |
| Critical Thinking | 15% | Questioning assumptions, noting anomalies, considering edge cases, awareness of leakage |
| Clarity of Communication | 10% | Can we follow your reasoning? Are findings presented in a logical flow? |

---

## Phase 2 Preview — Prediction Format

This is for your reference only. You will submit predictions in Phase 2 if selected.

```csv
account_id,is_mule,suspicious_start,suspicious_end
ACCT_000000,0.02,,
ACCT_000003,0.87,2023-11-15T09:30:00,2024-02-20T16:45:00
```

One row per account in `test_accounts.csv`:
- **`is_mule`**: Probability between 0 and 1
- **`suspicious_start`**: ISO timestamp marking the beginning of suspected mule activity (leave empty if predicted legitimate)
- **`suspicious_end`**: ISO timestamp marking the end of suspected mule activity (leave empty if predicted legitimate)

Primary metric: **AUC-ROC** on `is_mule`. Time window accuracy is a separate bonus metric scored via temporal IoU.

---

## Submission Instructions

- Submit your report as a **Jupyter Notebook (in .md (markdown) format)** or **PDF**
- If using a notebook, ensure all cells are executed and outputs are visible
- Name your file: `<team_name>_eda_report.md` or `<team_name>_eda_report.pdf`
- Submit via the form shared on the event page before the deadline
- For link - https://forms.gle/wqAeindJJYp1hXct5

---

Good luck. Focus on understanding the data — the models come later.
