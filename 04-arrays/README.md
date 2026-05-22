# 📦 Chapter 04 — Arrays

> The most fundamental data structure. Get this right and half of DSA gets easier.

---

## 🤔 What is an Array?

An array is a **contiguous block of memory** holding elements of the **same type and size**.

Two words doing all the work: **contiguous** (side by side in RAM, no gaps) and **same type** (every slot is the same width in bytes).

These two properties are not random design choices — they are what give arrays their superpower.

---

## 🧠 How Arrays Live in Memory

Think of RAM as numbered lockers. An array claims a stretch of adjacent lockers, all the same size.
Address:  1000   1004   1008   1012   1016
Index:    [0]    [1]    [2]    [3]    [4]
Value:    10     20     30     40     50

Each `int` is 4 bytes, so addresses jump by 4. Indices start at 0, not 1 (we'll see why in a second).

---

## ⚡ The Superpower: O(1) Random Access

The CPU does not walk through the array to find `arr[i]`. It computes the address directly:
address(arr[i]) = base_address + (i × size_of_element)

Example: `arr[3]` in the array above = `1000 + (3 × 4) = 1012`.

One multiply, one add. Done. The array could have 10 elements or 10 billion — access time is the same.

> **Why this works:** because elements are **contiguous** (so addresses are predictable) and **same size** (so the multiplication is valid).

---

## 🔢 Why Index Starts at 0, Not 1

Three real reasons:

1. **Address math is cleaner.** `arr[0]` lives at `base + 0`, which is just `base`. No subtraction needed on every access.
2. **Half-open intervals are cleaner.** `for (i = 0; i < n; i++)` runs exactly n times. Range `[0, n)` covers n elements. No off-by-one bugs.
3. **In C, `arr[i]` is literally `*(arr + i)`.** The index is an *offset from the start*, not a position number. Offset 0 = the start itself.

Languages built for mathematicians (MATLAB, Fortran, R) use 1-indexing. Languages built for systems work (C, C++, Java, Python, Go) use 0-indexing.

---

## 🐢 The Weakness: O(n) Insertion and Deletion in the Middle

The same contiguity that makes access fast makes structural changes expensive.

**Inserting 15 at index 1 in `[10, 20, 30, 40, 50]`:**

You can't just drop 15 into slot 1 — that overwrites 20. You have to shift from the **right end backwards** first:
Step 1: arr[4] (50) → arr[5]    [10, 20, 30, 40, 50, 50]
Step 2: arr[3] (40) → arr[4]    [10, 20, 30, 40, 40, 50]
Step 3: arr[2] (30) → arr[3]    [10, 20, 30, 30, 40, 50]
Step 4: arr[1] (20) → arr[2]    [10, 20, 20, 30, 40, 50]
Step 5: Place 15 at arr[1]      [10, 15, 20, 30, 40, 50]

**Important:** shift from tail to head, never head to tail. Shifting from the head overwrites data.

**General rule:** inserting at index `k` in array of size `n` requires shifting `n - k` elements. Worst case is index 0 — shift everything.

Deletion is the same story in reverse: remove index k, shift everything after it leftward to close the gap.

---

## 🏗️ Static vs Dynamic Arrays

### Static array
```cpp
int arr[5];   // size fixed at compile time
```
- Size is fixed, cannot grow
- Lives on the **stack** (the memory region where function-local data lives)
- Auto-destroyed when the function returns
- Fast, but inflexible

### Dynamic array (`std::vector` in C++)
```cpp
vector<int> arr;
arr.push_back(10);
arr.push_back(20);
```
- Grows automatically
- Lives on the **heap** (the memory region for runtime-allocated data)
- Used in 95% of real code

---

## 🪄 How `vector` Grows — and Amortized O(1)

When a vector hits its capacity and you call `push_back`, it:

1. Allocates a new block, usually **2× the old capacity**
2. Copies all existing elements into the new block
3. Adds the new element
4. Frees the old block

That copy is O(n)! So how can we say `push_back` is O(1)?

### The math

Suppose you start empty and do n push_backs. Capacities go 1 → 2 → 4 → 8 → 16 → ... → n.

Total copy cost across all resizes: `1 + 2 + 4 + 8 + ... + n ≈ 2n`.
Total cheap "drop in next slot" cost: `n`.
Total work for n push_backs: about `3n`.

Average cost per push_back: `3n / n = 3 = O(1)`.

### Why doubling matters

If the vector grew by a fixed amount (say +10) instead of doubling, we would resize every 10 pushes — and each resize still costs O(n) because all existing elements must be copied. Total work would be O(n²).

Doubling makes resizes **exponentially rare** as the vector grows. That's what saves us.

### Credit card analogy

Most months: $1 grocery bill (cheap push).
One month a year: $100 expense (resize).
Average monthly cost: small. Same with push_back.

> **Worst single operation:** O(n) when resize hits.
> **Amortized across a sequence:** O(1).
> Both true at once.

---

## 📊 Full Complexity Table

| Operation | Time | Why |
|---|---|---|
| Access `arr[i]` | O(1) | Direct address formula |
| Search (unsorted) | O(n) | Must scan every element |
| Search (sorted) | O(log n) | Binary search possible |
| Insert at end | O(1) amortized | Drop in next slot (rare resize) |
| Insert at index k | O(n) | Shift `n - k` elements |
| Insert at index 0 | O(n) | Shift all n elements (worst case) |
| Delete at end | O(1) | Decrement size |
| Delete at index k | O(n) | Shift everything after k leftward |
| Update at index | O(1) | Direct address formula |

---

## 🚀 The Hidden Superpower: Cache Locality

Modern CPUs have a **cache** — a tiny, blazing-fast memory layer between RAM and the CPU.

When you access `arr[0]`, the CPU does not load just that one element. It loads a whole **cache line** (typically 64 bytes) of nearby memory into the cache.

Because arrays are contiguous, `arr[1]`, `arr[2]`, ... are already in the cache. Subsequent accesses are nearly free.

This is called **spatial locality**, and it's why arrays often outperform linked lists in real workloads even when Big-O says they should be equal. A cache hit takes ~1 CPU cycle; a cache miss takes 100+. Linked lists scatter nodes across the heap and thrash the cache.

> **Big-O is not the whole story. Memory layout matters.**

---

## ✅ Key Takeaways

- Arrays = contiguous memory + same-size elements
- Access is O(1) because of math, not searching
- Insertion/deletion in the middle is O(n) because of shifting
- Always shift from the **tail backwards** when inserting, never head-first
- Static array → stack, fixed size. Dynamic array (vector) → heap, grows by doubling
- `push_back` is **amortized O(1)** because doubling makes resizes exponentially rare
- Arrays win in real performance partly because of **cache locality**, not just Big-O

---

## 📚 Next Up

→ [Chapter 05 — Strings](../05-strings)
