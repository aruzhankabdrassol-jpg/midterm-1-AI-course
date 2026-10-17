# Midterm 1: answers

GitHub username: aruzhankabdrassol-jpg
Session code: PRACTICE
Fingerprint: d713e2d0

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
| A | 0.650 | 0.733 | 0.800 | 0.701 |
| B | 0.350 | 0.533 | 0.617 | 0.441 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.733 | 0.610 | 0.829 |
| A, k=5 | 0.800 | 0.682 | 0.882 |
| B, k=3 | 0.533 | 0.409 | 0.654 |
| B, k=5 | 0.617 | 0.490 | 0.729 |

Reading of the intervals: the only pair that does not overlap is (A,5) vs (B,3) (0.654 < 0.682), so (A,5) is shown to be better than (B,3). All other pairs overlap, including (A,5) vs (B,5) (0.682 < 0.729) and (B,3) vs (B,5), so I claim no winner for them.

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 8347.81 |
| A, k=5 | 10916.24 |
| B, k=3 | 5745.74 |
| B, k=5 | 6579.46 |

### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

Both A candidates exceed the 7500 budget (A,3: 8348; A,5: 10916), so only B fits. The two B candidates' intervals overlap (B,3: 0.409-0.654; B,5: 0.490-0.729), so I do not claim B,5 is better. I choose it because it costs 6579, within budget, and its recall is higher on every language, most for Russian (0.50 vs 0.35). Recall of B,5 per language: EN 14/20 = 0.70, RU 10/20 = 0.50, KK 13/20 = 0.65. Russian is served worst: half of its questions miss the right chunk in the top 5. With 20 questions per language these figures are rough (RU interval about 0.30-0.70). A does not clearly beat B at equal k (intervals overlap), so the budget costs little proven accuracy.

### 1e. Next measurement (at most 60 words)

Run 100 additional Russian test questions through B,5 and A,3 (120 Russian questions in total). The Russian interval would shrink from about +-20 to +-9 points, showing whether B,5 really lags behind A,3; if A is clearly better, ask finance for the extra 848 a year.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.894 | KK, degraded | 0.833 | 0.664 | 0.927 | 15740.63 |
| L | 0.850 | KK, degraded | 0.700 | 0.521 | 0.833 | 7200 |

Personal-data pages per year sent abroad under H: 620500

### 2b. Memo (at most 300 words)

**Recommendation: choose pipeline L (own server in Kazakhstan), with human review of weak cells.**

**Accuracy.** On 180 test invoices each, H got 161 right (89.4 %, 95 % CI 84.1-93.1 %) and L 153 (85.0 %, CI 79.1-89.5 %). The intervals overlap, so the data do not show H is better overall. That overall figure hides the worst-served group: Kazakh degraded scans in both pipelines. L: 21/30 = 70.0 % (CI 52.1-83.3 %). H: 25/30 = 83.3 % (CI 66.4-92.7 %). These also overlap, and 30 documents per cell is little evidence; L's true rate could be near 50 %.

**Personal data.** 34 % of 5,000 pages/day carry a name and ИИН: 620,500 pages a year. Lecture 5.1 questions:
- H: where does the scan go? A data centre abroad (known). Who stores it? The vendor (unknown). For how long? Unknown. We must obtain written terms; a vendor's statement is not a compliance review under Law No. 94-V.
- L: our own server in Kazakhstan (known), stored by us (known), retention period not yet set: we must define it.

**Safeguards anyone can verify:** (1) block outbound internet from the L server; check firewall logs; (2) automatic deletion of scans after a fixed period, with a deletion log; (3) send every degraded Kazakh and Russian scan to manual check, reporting error rates weekly; (4) log and audit access to ИИН fields.

**Choice.** L costs 7,200 a year against about 15,741 for H, and keeps all 620,500 pages in the country, with no proven accuracy loss. Residual risk: errors on degraded Kazakh scans remain; review reduces but does not remove them. We should test 200 more degraded Kazakh scans.

## Part 3: Explain your own numbers

### Q1

Number: 2.55
Why (at most 40 words): Outputs cost 6x more per token and grow only 1.73x (inputs 3.00x), so they weigh more in the bill and pull the ratio down from 2.90 (token ratio) to 2.55.

### Q2

Number: 473
Why (at most 40 words): The chunk is 526 e5 tokens, but e5-small reads only 512, so the last 14 are cut silently. The sentence ends before about 493, inside the limit, so a matching question can retrieve the chunk. No error appears.

### Q3

Number: 69.0 %
Why (at most 40 words): Top-p cuts the tail (here Monday+Friday reach 84.5 % >= 0.8) and renormalises, discarding unlikely tokens entirely; lower temperature only sharpens all probabilities, keeping every token possible.

### Q4

Number: 31.1 %
Why (at most 40 words): I kept only birthdays and account numbers: arbitrary facts with no rule, where the once-seen share bounds errors. I left out multiplication and sequences: they follow rules. A prompt adds no knowledge, so the bound stays.

### Q5

Number: 15.85
Why (at most 40 words): 317 tokens per parameter against Chinchilla's 20: the model is heavily over-trained relative to compute-optimal. Chinchilla found models like GPT-3 were under-trained: too many parameters for too few tokens.
