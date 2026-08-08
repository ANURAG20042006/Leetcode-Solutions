# 1. Two Sum

**Difficulty:** Easy  
**Language:** Java

---

## 🧠 Approach

### Core Idea

The submitted solution uses a brute-force approach with two nested loops to check every possible pair of elements in the `nums` array to determine if their sum equals `target`. 

The outer loop iterates through each element as the potential first candidate of the pair, while the inner loop iterates through all subsequent elements as the potential second candidate. Once a pair `(nums[i], nums[j])` is found such that `nums[i] + nums[j] == target`, the algorithm immediately returns an array containing their indices `[i, j]`.

The primary DSA pattern demonstrated here is Brute Force Search / Array Traversal.

### Why This Approach?

When approaching the Two Sum problem for the first time, the direct requirement is to find two distinct indices whose values sum up to a given target. 

The most intuitive way to solve this without additional data structures is to try every combination of two elements. By setting up an outer loop for the first element and an inner loop starting immediately after it for the second element, we guarantee that every unique pair of indices $(i, j)$ with $i < j$ is evaluated.

---

## 💡 How to Think About the Problem

### Step 1: Understand What We Need

We need to find two distinct indices `i` and `j` in the input array `nums` such that `nums[i] + nums[j] == target`. The output must be returned as an array of two integers `[i, j]`.

### Step 2: Identify the Key Observation

Every pair of indices $(i, j)$ where $i \neq j$ produces a sum. By exhaustively checking every unique pair in the array, we are guaranteed to find the solution if one exists.

### Step 3: Recognize the Pattern

This solution fits the Brute Force pattern. When no additional memory can be used or when simplicity is prioritized over time efficiency, checking all possible candidate pairs via nested loops is the default baseline solution.

### Step 4: Decide What Information We Need to Maintain

- `n`: Total length of the input array.
- `i`: Pointer for the first element in the pair (ranges from `0` to `n - 2`).
- `j`: Pointer for the second element in the pair (ranges from `i + 1` to `n - 1`).

### Step 5: Derive the Algorithm

1. Store the length of `nums` in variable `n`.
2. Iterate `i` from `0` to `n - 2`.
3. For each `i`, iterate `j` from `i + 1` to `n - 1`.
4. Check if `nums[i] + nums[j] == target`.
5. If a match is found, return `new int[]{i, j}` immediately.
6. If the loops finish without a match, return an empty array `new int[]{}`.

---

## 🔍 Algorithm

1. Retrieve the array length `n = nums.length`.
2. Start an outer loop with index `i` from `0` up to `n - 2`.
3. Start an inner loop with index `j` from `i + 1` up to `n - 1`.
4. Check if `nums[i] + nums[j]` equals `target`.
5. If true, return a new array containing indices `{i, j}`.
6. If both loops complete without finding a matching pair, return an empty array `{}`.

### Important Implementation Details

- `n` → Stores the length of `nums` to avoid recalculating array length in loop conditions.
- `i` → Tracks the index of the first candidate element.
- `j = i + 1` → Ensures `j` is strictly greater than `i`, preventing the algorithm from using the same element twice and avoiding duplicate checks of swapped pairs.
- `nums[i] + nums[j] == target` → The core condition checking if the two current elements sum to the desired target.

---

## 🧩 Understanding the Code

### Outer Loop (First Element Selection)

```java
for (int i = 0; i < n - 1; i++) {
```

This loop selects the first element `nums[i]`. It stops at `n - 2` because the second element needs at least one position (`n - 1`) remaining in the array to form a pair.

### Inner Loop (Second Element Selection)

```java
for (int j = i + 1; j < n; j++) {
```

This loop selects the second element `nums[j]`. Starting `j` at `i + 1` avoids using `nums[i]` twice and prevents checking symmetric pairs (e.g., checking `(1, 0)` after already checking `(0, 1)`).

### Target Comparison & Return

```java
if (nums[i] + nums[j] == target) {
    return new int[]{i, j};
}
```

If the sum of the elements at indices `i` and `j` equals `target`, the answer is found. The function constructs and returns the result array immediately, terminating further execution.

---

## 🧠 Why This Works

The problem requires finding two indices whose values sum to `target`. By examining every pair $(i, j)$ where $0 \le i < j < n$, the algorithm covers the complete search space of all possible two-element combinations. If a valid answer exists, the nested loops will encounter that specific pair and return its indices.

### Key Invariant

At any given iteration $(i, j)$, all pairs $(a, b)$ that precede $(i, j)$ in lexicographical order have been checked and confirmed not to sum to `target`.

---

## ⏱️ Time Complexity

**Time:** `O(n²)`

### Why?

The outer loop runs $n - 1$ times. For a given index `i`, the inner loop runs $n - 1 - i$ times.
The total number of pair comparisons performed is:

$$ (n - 1) + (n - 2) + \dots + 2 + 1 = \frac{n(n - 1)}{2} = \frac{n^2 - n}{2} $$

Dropping lower-order terms and constants gives a time complexity of `O(n²)`.

---

## 💾 Space Complexity

**Auxiliary Space:** `O(1)`

The algorithm only uses a constant amount of extra memory for loop counters (`i`, `j`) and array length (`n`). No additional data structures (like HashMaps or arrays) are allocated proportional to input size.

---

## 🔄 Alternative Approach

### Alternative Idea

Instead of using two nested loops to find the pair, we can use a Hash Map to store elements and their corresponding indices as we iterate through the array once. 

For each element `nums[i]`, we calculate its complement `complement = target - nums[i]`. We check if `complement` is already present in the map. If it is, we have found our answer: the current index `i` and the stored index `map.get(complement)`. If it is not, we store `nums[i]` and index `i` into the map and continue.

### Complexity

**Time:** `O(n)`  
**Space:** `O(n)`

### Comparison

| Aspect | Submitted Approach | Alternative |
|---|---|---|
| Main Idea | Brute-force checking all index pairs | Hash Map tracking seen complements |
| Time | `O(n²)` | `O(n)` |
| Space | `O(1)` | `O(n)` |
| Advantage | Zero extra memory consumed | Dramatically faster for large inputs |

---

## 📌 Key Takeaways

- **Pattern:** Array / Brute Force
- **Core Observation:** Checking all unique index pairs $(i, j)$ guarantees finding the target pair if it exists.
- **Important Data Structure:** None (uses standard primitive variables).
- **Time:** `O(n²)`
- **Space:** `O(1)`

### Remember

> While nested loops check every pair in $O(n^2)$ time with $O(1)$ space, using a Hash Map reduces the lookup time to $O(n)$ by trading $O(n)$ extra space to store seen values.

---

## 🔗 Problem

[LeetCode Problem](https://leetcode.com/problems/two-sum/)
