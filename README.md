# Can AI Choose the Better Loan Option?

**AI Portfolio — Lab 3: Experiments 1, 2 and 3 (Scenarios S01–S08)**

Author: Keshav Tayal

This project tests whether an AI system can reliably pick the better of two loan options (Option A vs Option B) when given the interest rate, processing fee, expected default and cost of funds. Three prompting strategies are compared against a manually verified answer key.

The full write-up is in [`Lab_3.docx`](Lab_3.docx).

---

## Table of Contents

- [Experiments](#experiments)
- [Decision Formula](#decision-formula)
- [Input Data](#input-data)
- [Designed Test Cases](#designed-test-cases)
- [Verified Answer Key](#verified-answer-key)
- [Prompts Used](#prompts-used)
- [Results](#results)
- [Final Scorecard](#final-scorecard)
- [Recommendation](#recommendation)
- [Limitations](#limitations)
- [Repository Contents](#repository-contents)

---

## Experiments

| Experiment | Strategy |
| --- | --- |
| **1** | Compare interest rates only |
| **2** | Direct decision, no working shown |
| **3** | Step-by-step calculation (Chain-of-Thought) |

## Decision Formula

```
Net Return = Interest Rate + Processing Fee − Expected Default − Cost of Funds
Profit     = Net Return × Loan Budget
```

**Decision rule:** choose the option with the higher profit. If profits are equal, choose the option with the lower expected default.

## Input Data

Excel sheet provided to the AI (Budget in ₹ crore):

| ID | Budget | A Interest | A Fee | A Default | A Cost | B Interest | B Fee | B Default | B Cost |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| S01 | 25 | 24% | 2.00% | 11.00% | 10.00% | 18% | 2.50% | 3.00% | 9.00% |
| S02 | 40 | 28% | 2.50% | 12.00% | 11.00% | 20% | 2.00% | 4.00% | 9.00% |
| S03 | 55 | 26% | 1.50% | 10.50% | 10.00% | 19% | 2.50% | 3.00% | 8.50% |
| S04 | 30 | 20% | 2.00% | 5.00% | 9.00% | 20% | 2.00% | 5.00% | 9.00% |
| S05 | 65 | 16% | 1.50% | 3.00% | 9.00% | 22% | 2.00% | 5.00% | 10.00% |
| S06 | 80 | 18% | 2.50% | 4.00% | 9.50% | 24% | 2.50% | 6.00% | 10.50% |
| S07 | 45 | 15% | 1.00% | 2.00% | 8.50% | 17% | 2.00% | 3.00% | 9.00% |
| S08 | 90 | 21% | 2.00% | 5.00% | 10.00% | 23% | 1.50% | 4.00% | 9.50% |

## Designed Test Cases

- **S03 — High interest, high default:** A has higher interest (26% vs 19%) but also much higher default (10.50% vs 3.00%).
- **S05 — Low default, low interest:** A has lower default (3.00% vs 5.00%) but much lower interest (16% vs 22%).
- **S04 — Exact tie:** A and B are identical on all four variables.

## Verified Answer Key

| ID | A Profit (₹ cr) | B Profit (₹ cr) | Correct Decision |
| --- | --- | --- | --- |
| S01 | 1.250 | 2.125 | B |
| S02 | 3.000 | 3.600 | B |
| S03 | 3.850 | 5.500 | B |
| S04 | 2.400 | 2.400 | **Exact tie** |
| S05 | 3.575 | 5.850 | B |
| S06 | 5.600 | 8.000 | B |
| S07 | 2.475 | 3.150 | B |
| S08 | 7.200 | 9.900 | B |

Option B yields higher profit in 7 of 8 scenarios; S04 is an exact tie at ₹2.40 crore each.

## Prompts Used

<details>
<summary><b>Experiment 1 — interest rates only</b></summary>

> Here is an Excel sheet with 8 loan scenarios, S01 to S08. Each scenario has Option A and Option B with Interest Rate, Processing Fee, Expected Default, and Cost of Funds. Compare only the Interest Rates. Which option has the higher interest rate? Answer only A or B. Do not calculate profit. Do not show working.

</details>

<details>
<summary><b>Experiment 2 — direct decision, no working</b></summary>

> Here is an Excel sheet with 8 loan scenarios, S01 to S08. For each scenario, calculate Profit = (Interest + Fee − Default − Cost) × Budget. Choose the option with the higher profit. Answer only in this table format: ID | Choice | Profit. Do not show working.

</details>

<details>
<summary><b>Experiment 3 — step-by-step (Chain-of-Thought)</b></summary>

> Here is an Excel sheet with 8 loan scenarios, S01 to S08. For each scenario, calculate step by step in 5 steps: Step 1: Calculate Option A's net return. Net = Interest + Fee − Default − Cost. Step 2: Calculate Option A's profit. Profit = Net × Budget. Step 3: Calculate Option B's net return and profit in the same way. Step 4: Compare the profits. Step 5: Final decision — Choose Option A or Option B. If profit is equal, choose the option with the lower expected default. Show all working for all 8 scenarios, then provide a final summary table.

</details>

<details>
<summary><b>Changed-number test</b></summary>

> Change only one number: in S01, Option B's expected default is now 7%. Recalculate S01 step by step using the same 5 steps. Do not change any other number.

</details>

## Results

### Experiment 1 — Interest rates only

**7/8** on the interest-rate question, but only **4/8** when judged against the answer key for the *better loan* (interest alone gets S01–S03 wrong).

| ID | Correct Answer | AI Answer | Mark |
| --- | --- | --- | --- |
| S01 | A (24% > 18%) | A | ✅ |
| S02 | A (28% > 20%) | A | ✅ |
| S03 | A (26% > 19%) | A | ✅ |
| S04 | Tie (20% = 20%) | B | ❌ |
| S05 | B (22% > 16%) | B | ✅ |
| S06 | B (24% > 18%) | B | ✅ |
| S07 | B (17% > 15%) | B | ✅ |
| S08 | B (23% > 21%) | B | ✅ |

In S01–S03, Option A has the higher interest rate but Option B earns more profit, because interest alone ignores fees, default and cost of funds. The prompt said "Answer only A or B", so the AI could not report a tie in S04.

### Experiment 2 — Direct decision, no working

**Profits 8/8, Decisions 7/8.** The S04 tie was not identified.

| ID | Correct Decision | AI Decision | Correct Profit | AI Profit |
| --- | --- | --- | --- | --- |
| S01 | B | B | 2.125 | 2.125 |
| S02 | B | B | 3.600 | 3.600 |
| S03 | B | B | 5.500 | 5.500 |
| S04 | Tie | **A** | 2.400 | 2.400 |
| S05 | B | B | 5.850 | 5.850 |
| S06 | B | B | 8.000 | 8.000 |
| S07 | B | B | 3.150 | 3.150 |
| S08 | B | B | 9.900 | 9.900 |

The numbers were accurate, but with no working the result could not be audited and the tie was hidden.

### Experiment 3 — Step-by-step (Chain-of-Thought)

**Decisions 8/8, Profits 8/8, Designed Test Cases 3/3.**

| ID | A Net | A Profit | B Net | B Profit | AI Decision |
| --- | --- | --- | --- | --- | --- |
| S01 | 5.0% | 1.250 | 8.5% | 2.125 | B |
| S02 | 7.5% | 3.000 | 9.0% | 3.600 | B |
| S03 | 7.0% | 3.850 | 10.0% | 5.500 | B |
| S04 | 8.0% | 2.400 | 8.0% | 2.400 | **Exact tie** |
| S05 | 5.5% | 3.575 | 9.0% | 5.850 | B |
| S06 | 7.0% | 5.600 | 10.0% | 8.000 | B |
| S07 | 5.5% | 2.475 | 7.0% | 3.150 | B |
| S08 | 8.0% | 7.200 | 11.0% | 9.900 | B |

### Changed-number test — **PASS**

S01, Option B's expected default changed from 3.00% to 7.00%:

```
Net    = 18% + 2.50% − 7.00% − 9.00% = 4.5%
Profit = 4.5% × ₹25 crore            = ₹1.125 crore

A ₹1.250 crore > B ₹1.125 crore  →  Decision: A (previously B)
```

The AI recalculated correctly and reversed its decision.

## Final Scorecard

| Experiment | Decisions | Profits | Test Cases | Tie Identified? |
| --- | --- | --- | --- | --- |
| Exp 1 — Interest rates only | 7/8 (interest question); 4/8 vs answer key | Not calculated | N/A | No |
| Exp 2 — Direct, no working | 7/8 | 8/8 | 2/3 | No — hidden |
| **Exp 3 — Step-by-step (CoT)** | **8/8** | **8/8** | **3/3** | **Yes — exact tie** |

## Recommendation

For loan decisions, the **step-by-step (Chain-of-Thought)** method is recommended:

- **Auditable:** every intermediate value can be independently verified.
- **Edge cases are visible:** the exact tie in S04 was concealed in Experiments 1 and 2 but clearly identified in Experiment 3.
- **Responsive to new inputs:** in the changed-number test, the AI recalculated and changed its decision when a single input was altered.

For any financial decision, the AI should not be asked for the final answer alone. It should be required to show its calculation so the result can be checked before it is used.

## Limitations

- Small sample: 8 scenarios, one run per experiment, so results may not generalise.
- The formula assumes fee, default and cost of funds are all percentages of the same loan amount.
- Experiment 2 got every profit figure right without working, so the benefit of step-by-step prompting here is auditability and tie detection, not higher numerical accuracy.

## Repository Contents

| File | Description |
| --- | --- |
| `README.md` | Project overview and results (this file) |
| `Lab_3.docx` | Full portfolio write-up |
