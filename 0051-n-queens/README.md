# 51. N-Queens

**Difficulty:** Hard  
**Language:** Python3

---

## 🧠 Approach

### Core Idea

The submitted solution places $N$ queens on an $N \times N$ chessboard such that no two queens attack each other. Because two queens cannot occupy the same row, the algorithm places exactly one queen per row, moving sequentially from row `0` to row `n - 1`.

To guarantee that no two queens attack each other across columns or diagonals, the solution maintains three hash sets during exploration:
1. `placedCol` tracks columns that already contain a queen.
2. `placedPos` tracks positive diagonals (anti-diagonals running from bottom-left to top-right), identified by the invariant expression $r + c$.
3. `placedNeg` tracks negative diagonals (main diagonals running from top-left to bottom-right), identified by the invariant expression $r - c$.

This approach uses **Backtracking** (a Depth-First Search over the implicit state-space tree). When a valid row configuration is found, the algorithm recursively proceeds to the next row. If a placement violates any constraint, that branch is pruned immediately. Once all $N$ queens are placed (reaching row $n$), the current board configuration is saved.

### Why This Approach?

A naive brute-force attempt would place $N$ queens across all $N^2$ cells indiscriminately, generating $\binom{N^2}{N}$ combinations. For $N = 8$, this results in over 4.4 billion combinations to check.

By recognizing that each row must contain exactly one queen, we reduce the search space to $N^N$ choices (picking 1 column out of $N$ for each of the $N$ rows). Furthermore, using hash sets allows us to check column and diagonal conflicts in $O(1)$ time, allowing us to prune invalid configurations immediately before going deeper. This prunes the search space down to $O(N!)$, making the computation efficient and feasible.

---

## 💡 How to Think About the Problem

### Step 1: Understand What We Need

We need to generate all unique board layouts of size $N \times N$ containing $N$ queens where no two queens share the same row, column, or diagonal.

### Step 2: Identify the Key Observation

Instead of treating the board as $N^2$ isolated cells, exploit mathematical invariants for chessboard lines:
- **Rows:** Solved by iterating through rows $r = 0, 1, \dots, n-1$ one step at a time.
- **Columns:** Solved by ensuring column $c$ is not placed twice.
- **Positive Diagonals ($/$)**: Every cell along a bottom-left to top-right diagonal shares the same sum of row and column indices ($r + c = \text{constant}$).
- **Negative Diagonals ($\backslash$)**: Every cell along a top-left to bottom-right diagonal shares the same difference of row and column indices ($r - c = \text{constant}$).

### Step 3: Recognize the Pattern

This problem fits the **Backtracking** paradigm:
- **Choose:** Pick an available column $c$ in the current row $r$.
- **Explore:** Mark constraints in sets and recurse to row $r + 1$.
- **Unchoose (Backtrack):** Remove the queen and unmark constraints to attempt other column choices for row $r$.

### Step 4: Decide What Information We Need to Maintain

- `board`: A 2D grid representing the current placement state of queens (`"Q"`) and empty spaces (`"."`).
- `placedCol`: Hash set storing occupied column indices.
- `placedPos`: Hash set storing occupied positive diagonal sums ($r + c$).
- `placedNeg`: Hash set storing occupied negative diagonal differences ($r - c$).
- `r`: An integer tracking the current row index being evaluated.

### Step 5: Derive the Algorithm

Start at row $0$. Iterate through column $c$ from $0$ to $n - 1$. If placing a queen at $(r, c)$ does not conflict with `placedCol`, `placedPos`, or `placedNeg`, update the state and recursively proceed to row $r + 1$. Undo state updates after returning from the recursive call to test remaining options.

---

## 🔍 Algorithm

1. Initialize `board` as an $N \times N$ matrix filled with `"."`.
2. Initialize three empty sets (`placedCol`, `placedPos`, `placedNeg`) and a solution list `ans`.
3. Define helper function `backtrack(r)`:
   - **Base Case:** If `r == n`, convert each row of `board` from character arrays to strings, add the solution to `ans`, and return.
   - **Branching:** Iterate column $c$ from $0$ to $n - 1$:
     - Check if $c \in \text{placedCol}$, $r + c \in \text{placedPos}$, or $r - c \in \text{placedNeg}$. If any are true, skip $c$.
     - Place queen: Set `board[r][c] = "Q"` and add $c$, $r + c$, and $r - c$ to their respective sets.
     - Recurse: Call `backtrack(r + 1)`.
     - Backtrack: Set `board[r][c] = "."` and remove $c$, $r + c$, and $r - c$ from their respective sets.
4. Invoke `backtrack(0)` and return `ans`.

### Important Implementation Details

- `placedCol` → Set tracking columns containing a queen.
- `placedPos` → Set tracking anti-diagonals via formula $r + c$.
- `placedNeg` → Set tracking main diagonals via formula $r - c$.
- `c in placedCol or r + c in placedPos or r - c in placedNeg` → Pruning condition evaluated in $O(1)$ time to prevent invalid subtrees.

---

## 🧩 Understanding the Code

### Initialization Phase

```python
board = [["."] * n for _ in range(n)]

placedCol = set()
placedPos = set()
placedNeg = set()
ans = []
```

This sets up the 2D matrix representing the board along with global state sets to keep track of occupied attack lines in $O(1)$ time.

### Base Case Construction

```python
if r == n:
    copy = board[:]
    sol = []
    for c in copy:
        sol.append("".join(c[:]))
    ans.append(sol)
    return
```

When $r$ reaches $n$, $N$ queens have been safely placed. The board representation is converted from a 2D list of single characters into a list of strings matching the required output format and appended to `ans`.

### Search and Backtracking Phase

```python
for c in range(n):
    if c in placedCol or r + c in placedPos or r - c in placedNeg:
        continue

    board[r][c] = "Q"
    placedCol.add(c)
    placedPos.add(r + c)
    placedNeg.add(r - c)

    backtrack(r + 1)

    board[r][c] = "."
    placedCol.remove(c)
    placedPos.remove(r + c)
    placedNeg.remove(r - c)
```

For the current row `r`, this block tries every column `c`. If a conflict exists, it skips placing the queen. Otherwise, it updates the board and attack sets, moves to row `r + 1`, and unmarks all changes afterward to restore state for subsequent column choices.

---

## 🧠 Why This Works

### Key Invariant

At the start of `backtrack(r)`, exactly `r` queens have been validly placed in rows $0$ through $r - 1$ without attacking each other. The hash sets `placedCol`, `placedPos`, and `placedNeg` accurately maintain all column and diagonal trajectories occupied by these first `r` queens.

---

## ⏱️ Time Complexity

**Time:** `O(N!)`

### Why?

In the first row ($r = 0$), there are $N$ placement choices. In the second row ($r = 1$), there are at most $N - 2$ valid column choices due to column and diagonal restrictions. In the third row, at most $N - 4$ choices remain. This recursive structure yields at most $O(N!)$ total search branches. At each leaf node ($r = n$), converting the matrix to strings takes $O(N^2)$ work, making the overall bound $O(N!)$.

---

## 💾 Space Complexity

**Auxiliary Space:** `O(N^2)`

### Why?

- The call stack reaches a maximum depth of $O(N)$ recursive frames.
- The state sets `placedCol`, `placedPos`, and `placedNeg` store at most $O(N)$ elements each.
- The 2D matrix `board` requires $O(N^2)$ space during execution.
*(Note: Output storage for valid solutions inside `ans` is excluded from auxiliary space per standard complexity conventions).*

---

## 🔄 Alternative Approach

### Alternative Idea

Instead of using Python `set` instances for tracking occupied vectors, we can use integer bitmasks (`cols`, `pos_diag`, `neg_diag`). Bitwise operations allow $O(1)$ identification of all valid positions using mask operations (`available_positions = ~(cols | pos_diag | neg_diag)`), and isolating the lowest set bit using `bit = available & -available`.

### Complexity

**Time:** `O(N!)`  
**Space:** `O(N)`

### Comparison

| Aspect | Submitted Approach | Alternative |
|---|---|---|
| Main Idea | Set-based constraint tracking with 2D matrix state | Bitmask constraint tracking with bitwise logic |
| Time | `O(N!)` | `O(N!)` |
| Space | `O(N^2)` | `O(N)` |
| Advantage | Highly readable and intuitive representation | Lower memory usage and smaller constant factor execution |

---

## 📌 Key Takeaways

- **Pattern:** Backtracking
- **Core Observation:** Placing queens row by row reduces the problem to column selection, where diagonal invariants ($r + c$ and $r - c$) allow $O(1)$ safety checks.
- **Important Data Structure:** Hash Sets (`set`)
- **Time:** `O(N!)`
- **Space:** `O(N^2)`

### Remember

> On grid problems with diagonal constraints, use $r + c$ for anti-diagonals (bottom-left to top-right) and $r - c$ for main diagonals (top-left to bottom-right) to convert geometric line checks into $O(1)$ hash set lookups.

---

## 🔗 Problem

[LeetCode Problem](https://leetcode.com/problems/n-queens/)
