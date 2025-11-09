# Stock Portfolio Tracker

## Problem Description

You are tasked with building an **advanced stock portfolio simulator** that can dynamically manage a trader’s share inventory using two different accounting systems:

- **FIFO (First In, First Out)** — managed using a **queue**
- **LIFO (Last In, First Out)** — managed using a **stack**

Each transaction affects the current inventory and the trader’s total profit.  
Your goal is to process all transactions correctly according to the rules and return the **final inventory** and **total profit**.


## Input Format

You are given a list of transactions.  
Each transaction is represented as a tuple: (operation, shares, price, mode)


Where:
- `operation`: `"buy"` or `"sell"`
- `shares`: positive integer — the number of shares
- `price`: price per share (float)
- `mode`: `"FIFO"` or `"LIFO"` — the accounting mode at the time of the transaction


## Rules

### 1. Buy Operation
- Adds shares to your portfolio.
- Under **FIFO**, shares are added to the **end of the queue**.
- Under **LIFO**, shares are pushed onto the **top of the stack**.

### 2. Sell Operation
- Sells shares according to the current accounting mode:
  - **FIFO**: sell from the **oldest batch first** (front of the queue).
  - **LIFO**: sell from the **most recent batch first** (top of the stack).
- The sale price applies to the number of shares sold.
- Profit is calculated as such:
  - If the sale requires multiple batches, continue removing until all shares are sold.
  - If a sale requests more shares than available, sell all remaining shares and stop processing further sales.

### 3. Mode Switching
- The `mode` applies **only to that transaction**.
- Switching modes does not change how previous shares are stored — you apply the new operation using the current mode.


## Output

Return two values:
1. **remaining_positions** — a list of `(shares, price)` still in inventory after all transactions are processed  
2. **total_profit** — the total accumulated profit (float)

---

## Test Case 1

### Input
transactions = [
("buy", 100, 10.0, "FIFO"),
("buy", 50, 12.0, "FIFO"),
("sell", 80, 15.0, "FIFO"),
("buy", 30, 14.0, "LIFO"),
("sell", 40, 16.0, "LIFO"),
("buy", 20, 11.0, "FIFO"),
("sell", 20, 13.0, "FIFO")
]

### Step-by-Step Execution

**Initial State:**  
inventory = []  
profit = 0  

1. **Buy 100 @ $10 (FIFO)**  
 → queue = [(100, 10)]

2. **Buy 50 @ $12 (FIFO)**  
 → queue = [(100, 10), (50, 12)]

3. **Sell 80 @ $15 (FIFO)**  
 → Sell 80 from (100, 10)  
 → Profit = (15 - 10) * 80 = 400  
 → Remaining = [(20, 10), (50, 12)]

4. **Buy 30 @ $14 (LIFO)**  
 → stack push → [(20, 10), (50, 12), (30, 14)]

5. **Sell 40 @ $16 (LIFO)**  
 → Sell 30 @ 14 → Profit = (16 - 14) * 30 = 60  
 → Sell 10 @ 12 → Profit += (16 - 12) * 10 = 40  
 → Total profit so far = 500  
 → Remaining = [(20, 10), (40, 12)]

6. **Buy 20 @ $11 (FIFO)**  
 → queue append → [(20, 10), (40, 12), (20, 11)]

7. **Sell 20 @ $13 (FIFO)**  
 → Sell 20 @ 10 → Profit = (13 - 10) * 20 = 60  
 → Total profit = 560  
 → Remaining = [(40, 12), (20, 11)]


### Output
- remaining_positions = [(40, 12), (20, 11)]  
- total_profit = 560.0

---

### Test Case 2

### Input
transactions = [
("buy", 50, 10.0, "FIFO"),
("buy", 60, 10.0, "FIFO"),
("sell", 100, 5.0, "FIFO")
]

**Step-by-Step:**
1. Buy 50 @ $10 → inventory = [(50, 10)]  
2. Buy 60 @ $10 → inventory = [(50, 10), (60, 10)]  
3. Sell 100 @ $5 → sell in FIFO order:
   - Sell 50 @ 10 → Profit = (5 - 10) × 50 = **-250**
   - Sell 50 @ 10 → Profit = (5 - 10) × 50 = **-250**
   - Total sold = 100 shares  
   - Remaining = [(10, 10)]

**Result:**
remaining_positions = [(10, 10)]
total_profit = -500.0

## Constraints

- 1 ≤ number of transactions ≤ 100,000  
- shares > 0  
- price > 0  
- mode ∈ {"FIFO", "LIFO"}  

---

## Function

```python
def simulate_portfolio(transactions: list[tuple[str, int, float, str]]) -> tuple[list[tuple[int, float]], float]:
  """
  Simulates a stock trading system with FIFO and LIFO modes.
  Returns the remaining inventory and total profit.
  """


