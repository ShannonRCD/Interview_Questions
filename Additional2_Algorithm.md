
## Additional Question 2: Minimum Sum Problem – 10 Minutes

### Problem Statement

Given an `m x n` grid filled with non-negative numbers, find a path from the **top-left** to the **bottom-right** corner, which **minimizes the sum** of all numbers along its path.

**Note:** You can only move **either down or right** at any point in time.

---

### Example

**Grid:**

```
1  3  1
1  5  1
4  2  1
```

**Input:**

```python
grid = [[1, 3, 1],
        [1, 5, 1],
        [4, 2, 1]]
```

**Output:**

```
7, [(0,0), (0,1), (0,2), (1,2), (2,2)]
```

**Explanation:**

The path `1 → 3 → 1 → 1 → 1` minimizes the sum. The total is `7`, and the corresponding path is given by the cell indices.

---

### Task

Write a **pseudocode solution** to find the **minimized sum** given an `m x n` grid as input.

Your function signature should look like:

```python
def minPathSum(grid):
    return sum_grid_path, path
```

Where:
- `sum_grid_path` is the minimum sum to reach the bottom-right corner.
- `path` is a list of coordinates (row, col) representing the chosen path.
