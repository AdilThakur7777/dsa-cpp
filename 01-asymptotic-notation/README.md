# 📐 Chapter 01 — Asymptotic Notation

> Before we measure algorithms, we need a language to describe how they behave as input grows. That language is **asymptotic notation**.

---

## 🤔 What does "asymptotic" mean?

The word **asymptotic** describes the behavior of a function as its input approaches a limit — usually infinity.

In DSA, we don't care how an algorithm performs on 5 items. We care how it scales when the input gets huge — 1 million, 1 billion items. Asymptotic notation gives us a precise way to talk about that scaling behavior, while ignoring irrelevant details like:

- The exact hardware
- The programming language
- Constant factors and lower-order terms

---

## 💡 Why do we need it?

Imagine two algorithms solving the same problem:

- **Algorithm A** takes `5n + 100` operations
- **Algorithm B** takes `n² + 2` operations

For n = 10:
- A → 150 operations
- B → 102 operations ← faster!

For n = 1,000,000:
- A → 5,000,100 operations
- B → 1,000,000,000,002 operations ← unusably slow

Asymptotic notation lets us say "A is **linear**, B is **quadratic**" — capturing the long-term truth without getting lost in small-n details.

---

## 🎯 The Three Asymptotic Notations

There are three formal notations, each describing a different kind of bound on an algorithm's growth.

### 1. Big-O (O) — Upper Bound
> "The algorithm grows **at most** this fast."

Describes the **worst-case** behavior. Most common in practice.

**Example**: Linear Search is O(n) — in the worst case, you scan all n elements.

### 2. Big-Omega (Ω) — Lower Bound
> "The algorithm grows **at least** this fast."

Describes the **best-case** behavior — the floor below which the algorithm cannot go.

**Example**: Linear Search is Ω(1) — in the best case, the target is the first element.

### 3. Big-Theta (Θ) — Tight Bound
> "The algorithm grows **exactly** this fast."

Used when the upper and lower bounds match — the algorithm has a single, well-defined growth rate.

**Example**: Iterating through every element of an array is Θ(n) — always exactly linear, no matter the input.

---

## 📊 Visual Intuition
---

## 🆚 Quick Comparison

| Notation | Meaning | Use Case | Example |
|---|---|---|---|
| **O(f(n))** | Upper bound | Worst case | Linear Search → O(n) |
| **Ω(f(n))** | Lower bound | Best case | Linear Search → Ω(1) |
| **Θ(f(n))** | Tight bound | Average / exact | Array traversal → Θ(n) |

---

## 🔑 Why Big-O dominates in practice

In interviews and engineering, you'll hear "Big-O" 95% of the time. Why?

1. **We care about the worst case** — we need guarantees that our system won't blow up under bad inputs.
2. **It's the easiest to reason about** — finding an upper bound is usually simpler than proving a tight bound.
3. **It's enough for comparison** — if Algorithm A is O(n) and B is O(n²), we know which to pick.

> When engineers say "Big-O", they often mean Big-Θ (tight bound) — but use the term "Big-O" out of habit.

---

## ✅ Key Takeaways

- Asymptotic notation describes growth **as n → ∞**, not exact running time
- There are **three** notations: O (upper), Ω (lower), Θ (tight)
- Big-O is the most used because we care about worst-case guarantees
- Ignore constants, ignore lower-order terms — focus on the dominant growth term

---

## 📚 Next Up

→ [Chapter 02 — Algorithm Complexity](../02-algorithm-complexity)
