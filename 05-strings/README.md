# 🔤 Chapter 05 — Strings

> A string is just an array of characters with extra rules. Master arrays first, and strings become 80% easy.

---

## 🤔 What is a String?

A string is a **sequence of characters** stored as an **array of integers in memory**.

Wait — integers? Yes. Every character ('A', 'a', '7', '?') is stored as a number. The mapping from numbers to visible characters is called an **encoding**.
"Adil"  →  [65]['d']['i']['l']    (each char = an integer)
'A'  'd'  'i'  'l'

Everything you learned about arrays — contiguous memory, O(1) access, O(n) insertion — applies to strings.

---

## 🔢 ASCII vs Unicode (UTF-8)

Two encodings to know:

### ASCII
- The original, from the 1960s
- **1 byte per character**
- Only **128 characters** — English letters, digits, punctuation
- Examples: `'A' = 65`, `'a' = 97`, `'0' = 48`

### Unicode (encoded as UTF-8)
- Universal standard — covers every language + emojis
- **Variable width**: 1 to 4 bytes per character
- Backward-compatible with ASCII (same byte values for 0–127)

| Character | Bytes in UTF-8 |
|---|---|
| `'A'` | 1 byte |
| `'é'` | 2 bytes |
| `'日'` | 3 bytes |
| `'😀'` | 4 bytes |

> **Trap:** in UTF-8, **1 character ≠ 1 byte**. So `s.length()` may give you byte count, not character count. Mostly safe to ignore for interview prep — but know it exists.

---

## 🏗️ Two Flavors in C++

### C-style string (`char[]`)
The old way, inherited from C.

```cpp
char name[] = "Adil";
```

In memory:
['A'] ['d'] ['i'] ['l'] ['\0']
[0]   [1]   [2]   [3]   [4]

That `'\0'` at the end is the **null terminator**. It marks where the string ends. C-strings don't store their own length — the only way to find length is to walk character-by-character until you hit `'\0'`.

> **`strlen()` is O(n)** because it scans the whole string looking for `'\0'`.

### `std::string` (modern C++)
A proper class. Internally stores:
- Pointer to the character array (heap)
- **Length field** (already computed)
- Capacity field

```cpp
string name = "Adil";
name.length();   // O(1) — just reads the stored length field
```

> **Rule of thumb:** Always use `std::string` in modern C++. Use C-strings only when talking to C libraries.

### The O(1) vs O(n) length difference

| Type | Length operation | Why |
|---|---|---|
| C-string | O(n) | Walk until `'\0'` |
| `std::string` | O(1) | Reads stored length field |

---

## 🔒 Mutability — The Hidden Trap

Strings behave very differently across languages.

| Language | Strings are... |
|---|---|
| **C++** | Mutable — can modify in place |
| **Java, Python, JS, C#** | Immutable — every "change" creates a new string |

### Why this matters: the O(n²) trap

In Python or Java:
```python
result = ""
for word in words:      # n words
    result += word      # each += creates a NEW string
```

Each iteration copies all previous characters into a new string. Total work:
1 + 2 + 3 + ... + n  ≈ n²/2  →  O(n²)

**Fix:** use a mutable buffer.
- Python → build a list, then `"".join(list)`
- Java → use `StringBuilder`
- C++ → no problem, `std::string` is mutable

> **Interview question 99% of the time:** "Why is concatenating strings in a loop slow?" → because of immutability + repeated copying.

---

## 📊 String Operations & Complexity

| Operation | Time | Notes |
|---|---|---|
| Access `s[i]` | O(1) | Array indexing |
| Length (`std::string`) | O(1) | Stored field |
| Length (`strlen`) | O(n) | Walk to `'\0'` |
| Concatenation `s1 + s2` | O(n + m) | Copy both |
| Compare `s1 == s2` | O(min(n, m)) | Char-by-char until mismatch |
| Substring | O(k) | Copies k chars into new string |
| Search substring (naive) | O(n × m) | Try every position |
| Reverse | O(n) | Swap pairs from both ends |

### Why is comparison O(min length)?
"apple" vs "ap"
Step 1: 'a' == 'a' ✓
Step 2: 'p' == 'p' ✓
Step 3: shorter string ran out → different lengths → not equal

You never compare more characters than the shorter string has. Best case: differ at index 0 → O(1). Worst case: equal strings → O(n).

---

## 🧩 Common String Problem Patterns

Just a preview — you'll hit these constantly on LeetCode:

1. **Two pointers** — palindromes, reversing, removing duplicates
2. **Sliding window** — longest substring with some property
3. **Frequency counting** — anagrams, character counts (use `int count[26]` for lowercase letters)
4. **Hashing** — comparing strings fast, detecting duplicates
5. **Pattern matching** — find pattern in text (KMP, Rabin-Karp — much later)

We'll cover each pattern with real problems when we get there.

---

## ✅ Key Takeaways

- A string is **an array of characters**, where characters are integers mapped by an encoding
- **ASCII** = 1 byte, 128 chars. **UTF-8** = variable width, supports everything
- C-strings end with `'\0'` and have no stored length → `strlen` is O(n)
- `std::string` stores its length → `.length()` is O(1)
- In **immutable-string languages (Python, Java)**, concatenating in a loop is O(n²). Use `StringBuilder` / `join`
- String comparison is O(min length), not O(1)
- Most string problems reduce to **two pointers**, **sliding window**, or **frequency counting**

---

## 📚 Next Up

→ [Chapter 06 — Linked Lists](../06-linked-lists)
