# ⚙️ Chapter 02 — Algorithm Complexity

> Asymptotic notation gives us the language. Algorithm complexity is what we actually **measure** with that language.

---

## 🤔 What is Algorithm Complexity?

**Algorithm complexity** is a measure of the resources an algorithm consumes as the input size grows. There are two kinds we care about:

1. **Time Complexity** — how long the algorithm takes
2. **Space Complexity** — how much memory the algorithm uses

Both are expressed using asymptotic notation (usually Big-O).

---

## ⏱️ Time Complexity

Time complexity counts the **number of basic operations** an algorithm performs as a function of input size `n`.

A "basic operation" is anything that takes constant time: an arithmetic operation, a comparison, an array access, an assignment.

We don't count in seconds because seconds depend on hardware. We count operations because that's a property of the algorithm itself.

### Example: Sum of an array
```cpp
int sum = 0;                          // 1 operation
for (int i = 0; i < n; i++) {         // loop runs n times
    sum += arr[i];                    // 1 operation per iteration
}
```

Total: `1 + n` operations → **O(n)** time complexity.

---

## 💾 Space Complexity

Space complexity counts the **extra memory** an algorithm needs as a function of input size `n`. This usually means memory **beyond the input itself**.

### Example 1: O(1) space
```cpp
int sum = 0;
for (int i = 0; i < n; i++) {
    sum += arr[i];
}
```
Only one extra variable (`sum`), regardless of n. → **O(1) space**.

### Example 2: O(n) space
```cpp
int* copy = new int[n];               // n extra slots
for (int i = 0; i < n; i++) {
    copy[i] = arr[i];
}
```
Allocates n extra slots. → **O(n) space**.

---

## 🎭 Best, Worst, and Average Case

For the same algorithm, complexity can vary based on the input. Take **Linear Search** — finding a target in an array:

| Case | Scenario | Complexity |
|---|---|---|
| **Best** | Target is the first element | O(1) |
| **Worst** | Target is the last element (or absent) | O(n) |
| **Average** | Target is somewhere in the middle | O(n) |

By default, when we say "the complexity is X", we mean the **worst case**, unless explicitly stated otherwise.

---

## 📐 The Three Rules of Complexity Analysis

### Rule 1 — Drop the constants
`O(2n) → O(n)`, `O(500) → O(1)`

As n grows huge, multiplying by a constant doesn't change the growth shape.

### Rule 2 — Drop the lower-order terms
`O(n² + n + 100) → O(n²)`

The biggest term dominates. For n = 1,000,000, n² is a trillion while n is only a million — the smaller term becomes invisible.

### Rule 3 — Worst case is the default
"O(n)" means worst case unless otherwise stated.

---

## 🔍 Worked Example: Step-by-step analysis

```cpp
void analyze(int arr[], int n) {
    int sum = 0;                          // O(1)

    for (int i = 0; i < n; i++) {         // O(n)
        sum += arr[i];
    }

    for (int i = 0; i < n; i++) {         // O(n²)
        for (int j = 0; j < n; j++) {
            cout << arr[i] * arr[j];
        }
    }

    cout << sum;                          // O(1)
}
```

**Step 1**: Add up all parts → O(1) + O(n) + O(n²) + O(1)
**Step 2**: Drop lower-order terms → **O(n²)**

---

## 🧠 The Time-Space Tradeoff

Often, you can make an algorithm **faster** by using **more memory**, or **save memory** by accepting **more time**. This is one of the most important tradeoffs in CS.

### Example: Checking for duplicates in an array

**Approach A — Save space, spend time**
Nested loop comparing every pair.
- Time: O(n²)
- Space: O(1)

**Approach B — Spend space, save time**
Use a hash set to track seen elements.
- Time: O(n)
- Space: O(n)

Both solve the same problem. The "right" choice depends on the constraints.

---

## ✅ Key Takeaways

- Complexity has two dimensions: **time** and **space**
- Always assume **worst case** unless told otherwise
- Apply the three rules: drop constants, drop lower terms, focus on dominant growth
- The **time-space tradeoff** is a recurring pattern — be aware of both axes

---

## 📚 Next Up

→ [Chapter 03 — Big-O Cheat Sheet](../03-big-o-cheat-sheet)
