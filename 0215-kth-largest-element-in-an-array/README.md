# 215. Kth Largest Element in an Array

**Difficulty:** Medium  
**Language:** Python3

---

## 🧠 Approach

### Core Idea

The submitted solution relies on full array sorting to find the $k$-th largest element. By sorting the array in descending order, all elements are arranged from largest to smallest.

Once the array is sorted in descending order, the $1$-st largest element is at index `0`, the $2$-nd largest is at index `1`, and in general, the $k$-th largest element is located precisely at 0-based index `k - 1`.

The fundamental technique used here is the **Sorting** pattern applied to array order statistics.

### Why This Approach?

When tasked with finding the $k$-th largest element, sorting the collection is the most intuitive baseline approach. If an array is completely sorted, order statistics (such as minimum, maximum, median, or $k$-th largest) become directly accessible via constant-time index lookups.

A naive approach without sorting might involve scanning the array $k$ times to repeatedly extract the maximum element, requiring $O(k \cdot n)$ time, which degrades severely when $k$ is large. Sorting structures all elements at once, transforming the search into a single index lookup.

---

## 💡 How to Think About the Problem

### Step 1: Understand What We Need

We need to find the value that corresponds to the $k$-th rank when all elements of `nums` are arranged in non-increasing order. Note that $k$ is 1-indexed (e.g., $k = 1$ refers to the maximum element) and duplicates are counted as distinct occurrences.

### Step 2: Identify the Key Observation

In a descending sorted array $S$, the element at index `0` is the $1$-st largest element, the element at index `1` is the $2$-nd largest element, and by extension, the element at index `k - 1` is the $k$-th largest element.

### Step 3: Recognize the Pattern

This approach belongs to the **Sorting** pattern. Sorting organizes unordered data into a sequence where positional indices directly correspond to relative element ranks.

### Step 4: Decide What Information We Need to Maintain

Since Python's built-in sorting function processes the array in a single operation, no complex state tracking or custom data structures are needed during iteration. We only need the final sorted array to retrieve the value at offset `k - 1`.

### Step 5: Derive the Algorithm

1. Perform a descending sort on `nums` using Python's `sorted(nums, reverse=True)`.
2. Access the value at index `k - 1`.
3. Return the retrieved value.

---

## 🔍 Algorithm

1. Pass `nums` to `sorted()` with `reverse=True` to create a new list sorted in descending order.
2. Index into the sorted list at position `k - 1`.
3. Return the resulting element.

### Important Implementation Details

- `sorted(nums, reverse=True)` → Creates a new list containing all elements of `nums` ordered from largest to smallest.
- `[k - 1]` → Maps the 1-based rank ($k$-th largest) to Python's 0-based list indexing.

---

## 🧩 Understanding the Code

### Phase 1: Sorting and Indexing

```python
return sorted(nums, reverse=True)[k-1]
```

This single expression executes two distinct operations:
1. `sorted(nums, reverse=True)` builds a new list containing all elements of `nums` ordered from highest to lowest value.
2. `[k - 1]` accesses the element at offset `k - 1` in $O(1)$ time and returns it as the $k$-th largest value.

---

## 🧠 Why This Works

Sorting establishes a total ordering over the array elements. When `reverse=True` is specified, the resulting array $S = [s_0, s_1, \dots, s_{n-1}]$ guarantees that $s_0 \ge s_1 \ge \dots \ge s_{n-1}$. 

Because there are exactly $k - 1$ elements before position $k - 1$ that are greater than or equal to $s_{k-1}$, the element $s_{k-1}$ is by definition the $k$-th largest element in the collection.

### Key Invariant

- **Descending Order Invariant:** For the sorted list $S = \text{sorted}(nums, \text{reverse=True})$, $S[i] \ge S[j]$ holds for all indices $0 \le i < j < n$.

---

## ⏱️ Time Complexity

**Time:** `O(n log n)`

### Why?

Python's built-in `sorted()` function uses Timsort, a hybrid stable sorting algorithm based on Merge Sort and Insertion Sort. Sorting an array of length $n$ takes $O(n \log n)$ comparisons in both average and worst cases. The subsequent array access `[k - 1]` is an $O(1)$ operation. Thus, total time is dominated by the sorting step: $O(n \log n)$.

---

## 💾 Space Complexity

**Auxiliary Space:** `O(n)`

Creating a new sorted list via `sorted(nums, ...)` allocates memory proportional to the size of the input array $n$. Additionally, Timsort requires up to $O(n)$ temporary working space for merging runs.

---

## 🔄 Alternative Approach

### Alternative Idea

Instead of sorting the entire array, we can maintain a **Min-Heap** of size $k$. As we iterate through each element in `nums`, we push it onto the heap. If the size of the heap exceeds $k$, we pop the smallest element. After processing all numbers, the heap will contain the $k$ largest elements of the array, and the root (smallest element in the heap) will be the $k$-th largest element overall.

### Complexity

**Time:** `O(n log k)`  
**Space:** `O(k)`

### Comparison

| Aspect | Submitted Approach | Alternative (Min-Heap) |
|---|---|---|
| Main Idea | Full descending sort | Min-heap of size $k$ |
| Time | `O(n log n)` | `O(n log k)` |
| Space | `O(n)` | `O(k)` |
| Advantage | One-line implementation, fast for small $n$ | Scalable when $k \ll n$ |

---

## 📌 Key Takeaways

- **Pattern:** Sorting / Array
- **Core Observation:** Sorting an array in descending order maps the $k$-th largest element directly to 0-based index `k - 1`.
- **Important Data Structure:** Array / List
- **Time:** `O(n log n)`
- **Space:** `O(n)`

### Remember

> Full sorting provides a quick and clean solution for order statistics, but using a Min-Heap of size $k$ ($O(n \log k)$) or Quickselect ($O(n)$ average time) provides better efficiency when $k$ is much smaller than $n$.

---

## 🔗 Problem

[LeetCode Problem](https://leetcode.com/problems/kth-largest-element-in-an-array/)
