# Midterm 1: answers

GitHub username: aruzhankabdrassol-jpg
Session code: KETTLE-62
Fingerprint: e85a766f

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used Claude and Google Colab.

- Claude: He was used as a consultant to check the syntax of the code, verify mathematical formulas, calculate numbers, an auxiliary tool for consulting on formulations and checking the correctness of calculations.
- Colab:It was used for dataset generation and data processing. I ran the data generator and the analysis code on my own dataset.

I studied the assignment and the heading on my own, and developed the solution logic.I did not share my data or answers with other students.
## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.467 | 0.533 | 0.633 | 0.523 |
| B | 0.400 | 0.533 | 0.783 | 0.523 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.533 | 0.409 | 0.654 |
| A, k=5 | 0.633 | 0.507 | 0.744 |
| B, k=3 | 0.533 | 0.409 | 0.654 |
| B, k=5 | 0.783 | 0.664 | 0.869 |

Reading of the intervals: (B,5) does not overlap with (A,3) or (B,3) (their upper end 0.654 < 0.664), so (B,5) is shown to be better than both; the gap is small. (B,5) overlaps with (A,5) (0.664 < 0.744), so no winner between them. (A,3) and (B,3) are identical, and (A,5) overlaps with both of them, so no other winner is claimed.

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 19394.38 |
| A, k=5 | 25484.64 |
| B, k=3 | 12717.02 |
| B, k=5 | 14355.71 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

Three candidates fit the budget of 22400: A,3 (19394), B,3 (12717) and B,5 (14356); A,5 costs 25485 and is out. B,5 has the highest recall, 47/60 = 0.783 (CI 0.664-0.869). Its interval does not overlap with A,3 or B,3 (both 0.409-0.654; the gap is small, 0.654 vs 0.664), so it is shown to be better than both, and it is cheaper than A,3. It overlaps with A,5 (0.507-0.744), but A,5 is unaffordable anyway. Recall of B,5 per language: EN 16/20 = 0.80, RU 15/20 = 0.75, KK 16/20 = 0.80. Russian is served worst, but the gap is one question and n = 20 per language, so no language is shown to be worse.

### 1e. Next measurement (at most 60 words)

Add 60 more test questions (20 per language) and re-run B,5, B,3 and A,3. B,5's lead over them rests on a 0.01 gap between intervals; with 120 questions the intervals narrow from about +-0.10 to +-0.07, which confirms or overturns the choice and shows whether Russian really lags.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.883 | RU, degraded | 0.767 | 0.591 | 0.882 | 42969.63 |
| L | 0.861 | RU, degraded | 0.767 | 0.591 | 0.882 | 27000 |

Personal-data pages per year sent abroad under H: 372300

### 2b. Memo (at most 300 words)

**Recommendation: choose pipeline L (own server in Kazakhstan), with human review of degraded scans.**

**Accuracy.** On 180 test invoices each, H got 159 right (0.883, 95 % CI 0.828-0.922) and L 155 (0.861, CI 0.803-0.904). The intervals overlap, so the data do not show H is better. The overall figure hides the worst-served group: degraded Russian scans, 23/30 = 0.767 (CI 0.591-0.882) in both pipelines. More generally, degraded scans reach only 0.81 (H) and 0.80 (L), against 0.96 and 0.92 for clean ones. With 30 documents per cell, the true rate in the worst cell could be near 0.6.

**Personal data.** 12 % of 8,500 pages/day carry a name and ИИН: 372,300 pages a year. Lecture 5.1 questions:
- H: where does the scan go? A data centre abroad (known). Who stores it? The vendor (unknown). For how long? Unknown. We must obtain written terms; a vendor's statement is not a compliance review under Law No. 94-V.
- L: our own server in Kazakhstan (known), stored by us (known), retention period not yet set: we must define it.

**Safeguards anyone can verify:** (1) block outbound internet from the L server; check firewall logs; (2) automatic deletion of scans after a fixed period, with a deletion log; (3) send every degraded scan to manual check, reporting error rates weekly; (4) log and audit access to ИИН fields.

**Choice.** L costs 27,000 a year against about 42,970 for H, and keeps all 372,300 pages in the country, with no proven accuracy loss. Residual risk: errors on degraded Russian scans remain; review reduces but does not remove them. We should test 200 more degraded Russian scans.

## Part 3: Explain your own numbers

### Q1

Number: 0.86
Why (at most 40 words): Tokens grow 1.96x, bytes 2.28x: the ratio 0.86 is below 1, so most of the premium is already in the UTF-8 bytes (Cyrillic letters take 2 bytes); the tokenizer merges bytes and recovers only 14 %.

### Q2

Number: 44.4 %
Why (at most 40 words): Only English is cut: 920.5 e5 tokens vs the 512 limit; RU (448) and KK (266) fit. The cut 44.4 % is silently dropped, never embedded, so questions about it cannot retrieve the chunk, and no error appears.

### Q3

Number: 73.7 %
Why (at most 40 words): Top-p keeps the smallest set whose probabilities reach 0.9: Monday+Friday give only 89.6 %, so Sunday is also kept; the rest is cut and renormalised. Lower temperature instead sharpens all probabilities, leaving every token possible.

### Q4

Number: 19.2 %
Why (at most 40 words): I kept account numbers and birthdays: arbitrary facts with no rule, where the once-seen share bounds errors. I left out arithmetic sequences and multiplication: they follow learnable rules. A prompt adds no knowledge, so the bound stays.

### Q5

Number: 11.90
Why (at most 40 words): 238 tokens per parameter against Chinchilla's 20: the model is heavily over-trained relative to compute-optimal. Chinchilla found models like GPT-3 under-trained: too many parameters for too few tokens.
