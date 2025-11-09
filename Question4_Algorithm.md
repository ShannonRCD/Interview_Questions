## Question 4: Dynamic Decision – 30 Minutes

## Problem Description

You are modeling a **financial decision process** that evolves over time based on a set of given rules.

You are provided three arrays of equal length `n`:

- `instructions`: an array of strings, each either `"add"` or `"skip"`.
- `years`: an array of integers.
- `financial_income`: an array of integers (which may be **positive or negative**).

Your goal is to determine the **maximum achievable final financial income value** by optimally deciding when to **continue**, **skip**, or **stop** the process.



## Simulation Rules

You start at index `i = 0` with:

- `year = 2025`
- `financial_income_value = 0`

At each step `i`, depending on `instructions[i]`, you can make one of the following moves:

### 1. `"add"` instruction
- You **must**:
  - Increase `year` by `years[i]`.
  - Add `financial_income[i]` to `financial_income_value`.
  - Move to `i + 1`.

### 2. `"skip"` instruction
- You have **two options**:
  1. Treat it as an `"add"` — same as above.
  2. **Skip ahead** by `years[i]` indices (`i + years[i]`), and multiply your **current** `financial_income_value` by `3/4`.

At any point, you may **choose to stop** to lock in your current income.

The process also **ends automatically** when:
- You go **out of bounds** (`i < 0` or `i >= n`), or  
- You **revisit** an index (cycles are not allowed).



## Objective

Return the **the maximum possible financial income value and year** that be obtained by following the optimised sequence of moves and stopping decisions.

---

## Test Case 1

- instructions = ["add", "skip", "add", "add"]
- years = [1, 2, 1, 3]
- financial_income = [100, 200, 50, 400]

(625.0, 2031)

**Initial State:**  
`year = 2025`, `income = 0`, `i = 0`

**i = 0 → "add"**
- Add `years[0] = 1` → `year = 2026`  
- Add `financial_income[0] = 100` → `income = 100`  
- Move to `i = 1`



**i = 1 → "skip"**
- **Option 1:** Treat as `"add"` → `income = 100 + 200 = 300`, move to `i = 2`  
- **Option 2:** Skip ahead `i + years[i] = 3`, multiply income by `0.75` → `100 * 0.75 = 75`  
- **Decision:** Continuing from `i = 2` yields more potential value → choose **Option 1**


**i = 2 → "add"**
- Add `years[2] = 1` → `year = 2027`  
- Add `financial_income[2] = 50` → `income = 350`  
- Move to `i = 3`


**i = 3 → "add"**
- Add `years[3] = 3` → `year = 2030`  
- Add `financial_income[3] = 400` → `income = 750`  
- Process ends at `i = 4` (**out of bounds**)


**Final:**  
- Raw simulated income = `750`  
- The **optimal DP path** (accounting for the potential skip at step 2) yields a **maximum achievable income** of `625.0`.  
- The process stops at **year = 2031**.

**Expected Output:**

(625.0, 2031)

---

## Test Case 2

### Input
- instructions = ["add", "skip", "add"]  
- years = [1, 5, 1]  
- financial_income = [100, -500, 200]


### Step-by-Step Execution

**Initial State:**  
year = 2025, income = 0, i = 0

**i = 0 → "add"**  
- Add years[0] = 1 → year = 2026  
- Add financial_income[0] = 100 → income = 100  
- Move to i = 1

**i = 1 → "skip"**  
- Option 1: Treat as "add" → add -500 → income = -400, move to i = 2  
- Option 2: Skip ahead i + years[i] = 6 (out of bounds)  
  - Multiply income by 0.75 → 100 × 0.75 = 75  
- Decision: Skipping gives higher income (75 > -400) → choose Option 2  
- Process ends (out of bounds)


**Final Result:**  
income = 75.0  
year = 2026  

**Expected Output:**  
(75.0, 2026)

