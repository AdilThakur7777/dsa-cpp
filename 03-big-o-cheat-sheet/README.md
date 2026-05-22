# 📋 Chapter 03 — Big-O Cheat Sheet

> A quick-reference table of common Big-O complexities, their growth rates, and where they show up. Bookmark this page.

---

## 📊 Common Complexity Classes

From fastest to slowest:

| Big-O | Name | Example |
|---|---|---|
| **O(1)** | Constant | Array access by index |
| **O(log n)** | Logarithmic | Binary Search |
| **O(n)** | Linear | Linear Search, sum of array |
| **O(n log n)** | Linearithmic | Merge Sort, Heap Sort |
| **O(n²)** | Quadratic | Bubble Sort, nested loop |
| **O(n³)** | Cubic | Triple nested loop, naive matrix multiplication |
| **O(2ⁿ)** | Exponential | Naive recursive Fibonacci, subset generation |
| **O(n!)** | Factorial | Permutations, brute-force TSP |

---

## 📈 Operations Comparison

How many operations each complexity requires for different values of `n`:

| Complexity | n = 10 | n = 100 | n = 1,000 | n = 1,000,000 |
|---|---|---|---|---|
| **O(1)** | 1 | 1 | 1 | 1 |
| **O(log n)** | ~3 | ~7 | ~10 | ~20 |
| **O(n)** | 10 | 100 | 1,000 | 1,000,000 |
| **O(n log n)** | ~33 | ~664 | ~10,000 | ~20,000,000 |
| **O(n²)** | 100 | 10,000 | 1,000,000 | 10¹² |
| **O(2ⁿ)** | 1,024 | huge | 🚫 | 🚫 |
| **O(n!)** | 3,628,800 | 🚫 | 🚫 | 🚫 |

> 🚫 = not feasible to compute in a human lifetime

---

## 🗂️ Data Structure Operations

### Array
| Operation | Time |
|---|---|
| Access by index | O(1) |
| Search (unsorted) | O(n) |
| Insert at end | O(1) amortized |
| Insert at index | O(n) |
| Delete at index | O(n) |

### Linked List (Singly)
| Operation | Time |
|---|---|
| Access by index | O(n) |
| Search | O(n) |
| Insert at head | O(1) |
| Insert at tail | O(n) (O(1) with tail pointer) |
| Delete at head | O(1) |

### Hash Table (unordered_map / unordered_set)
| Operation | Avg | Worst |
|---|---|---|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |

### Binary Search Tree (balanced)
| Operation | Avg | Worst |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

### Heap (Min/Max)
| Operation | Time |
|---|---|
| Insert | O(log n) |
| Extract min/max | O(log n) |
| Peek min/max | O(1) |
| Build heap | O(n) |

---

## 🔢 Sorting Algorithms

| Algorithm | Best | Average | Worst | Space |
|---|---|---|---|---|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(k) |
| **Radix Sort** | O(nk) | O(nk) | O(nk) | O(n+k) |

---

## 🔎 Searching Algorithms

| Algorithm | Best | Average | Worst | Notes |
|---|---|---|---|---|
| **Linear Search** | O(1) | O(n) | O(n) | Works on unsorted |
| **Binary Search** | O(1) | O(log n) | O(log n) | Requires sorted array |
| **BFS / DFS** | O(V+E) | O(V+E) | O(V+E) | V = vertices, E = edges |

---

## 🚦 Rules of Thumb (Interview Heuristics)

| Constraint | Acceptable Complexity |
|---|---|
| n ≤ 10 | O(n!) |
| n ≤ 20 | O(2ⁿ) |
| n ≤ 500 | O(n³) |
| n ≤ 5,000 | O(n²) |
| n ≤ 10⁶ | O(n log n) |
| n ≤ 10⁸ | O(n) or O(log n) |
| n > 10⁸ | O(log n) or O(1) |

> Use this table to pick the right algorithm based on input size constraints in a problem.

---

## 🎯 Pattern → Complexity Mapping

| If you see... | Likely complexity |
|---|---|
| Single loop over n | O(n) |
| Nested loop, both over n | O(n²) |
| Halving the problem each step | O(log n) |
| Divide and conquer with merge | O(n log n) |
| Generate all subsets | O(2ⁿ) |
| Generate all permutations | O(n!) |
| Recursion with memoization (DP) | O(states × work per state) |

---

## ✅ Quick Recall Checklist

Before submitting any algorithm, ask yourself:

- [ ] What's the **time complexity**? (worst case)
- [ ] What's the **space complexity**? (extra memory beyond input)
- [ ] Have I **dropped constants** and **lower-order terms**?
- [ ] Does it fit within the **input size constraints**?

---

## 📚 Next Up

→ [Chapter 04 — Arrays](../04-arrays)
