# 🔗 Chapter 06 — Linked Lists

> Where DSA gets meaningfully harder. Arrays you can picture as boxes in a row — linked lists require thinking in pointers.

---

## 🤔 Why Linked Lists Exist

Arrays have a problem: **insertion and deletion in the middle is O(n)** because of shifting. Inserting at the head of a million-element array means shifting all million elements.

Linked lists fix this by **giving up contiguous memory**. Instead of one big block, elements are scattered across the heap, and each element holds a **pointer** to where the next one lives.

**The trade-off:**
- ❌ Lose O(1) random access
- ✅ Gain O(1) insertion/deletion *if you already have a pointer to the spot*

This is the single most important trade-off in this chapter — **contiguity vs flexibility**.

---

## 🧱 The Node

The building block of a linked list is a **node**. Each node holds two things:

1. **Data** — the value
2. **A pointer (`next`)** — the address of the next node

```cpp
struct Node {
    int data;
    Node* next;
};
```

`Node* next` does not contain the next node — it contains the **memory address** where the next node lives.

### Visual
[10 | •] ─────► [20 | •] ─────► [30 | NULL]
addr 800        addr 1500       addr 2200

Key observations:
- Nodes live at **scattered memory addresses** (800, 1500, 2200) — not adjacent
- Each `next` holds the address of the next node
- The last node's `next` is **NULL** — meaning "end of list"

You also need a **head pointer** — a variable holding the address of the first node. Without it, you have no way to find the list.
head ────► [10 | •] ─────► [20 | •] ─────► [30 | NULL]

> **Lose the head pointer → lose the entire list.** Memory leak.

---

## 🆚 Linked List vs Array — The Core Contrast

| Aspect | Array | Linked List |
|---|---|---|
| Memory layout | Contiguous block | Scattered nodes + pointers |
| Access `[i]` | O(1) — address math | O(n) — walk from head |
| Insert at head | O(n) — shift all | O(1) — update pointer |
| Insert at tail | O(1) amortized | O(n) (or O(1) with tail pointer) |
| Insert in middle | O(n) — shift | O(1) **if you have the previous node** |
| Memory overhead | Just the data | Data + pointer per node |
| Cache friendliness | Excellent | Poor |

**The "if you have the previous node" catch:**
Getting to that node is O(n) (you have to walk). So "delete the 5th node" = O(5) to walk + O(1) to delete = **O(n) total**. The O(1) advantage only applies when you're already holding the pointer (e.g., processing nodes in a loop).

---

## 🌀 Three Flavors

### Singly Linked List
Each node points only to the next.
head ────► [10 | •] ─────► [20 | •] ─────► [30 | NULL]

- ✅ Simple, low memory overhead
- ❌ Can only traverse forward

### Doubly Linked List
Each node has **two pointers** — `next` and `prev`.
NULL ◄──── [10 | •] ◄────► [20 | •] ◄────► [30 | •] ────► NULL

- ✅ Bidirectional traversal
- ✅ Easy deletion when you only have a pointer to the node (use `prev` to find previous)
- ❌ Extra memory per node

C++'s `std::list` is doubly linked.

### Circular Linked List
The last node's `next` points back to the first node.
head ────► [10 | •] ─────► [20 | •] ─────► [30 | •]
▲                                  │
└──────────────────────────────────┘

Used for round-robin schedulers, music playlists in loop mode.

---

## ⚙️ Core Operations

### Insert at Head — O(1)

Before:
head ────► [10 | •] ────► [20 | NULL]

Insert `5` at head:
head ────► [5 | •] ────► [10 | •] ────► [20 | NULL]

Steps:
1. Create new node with data = 5
2. New node's `next` = current head
3. Update `head` to point to new node

```cpp
Node* newNode = new Node{5, head};
head = newNode;
```

Two operations regardless of list size → **O(1)**.

This is exactly where linked lists beat arrays. Array insert-at-head is O(n).

### Insert at Tail — O(n) without tail pointer

```cpp
Node* curr = head;
while (curr->next != nullptr) {
    curr = curr->next;
}
curr->next = new Node{value, nullptr};
```

Walk to end (O(n)), then attach (O(1)). Maintain a `tail` pointer to make this O(1).

### Delete a Node — O(1) if you have the previous node

To remove the node with 20:
head ────► [10 | •] ────► [20 | •] ────► [30 | NULL]

Make 10's `next` skip over 20:
head ────► [10 | •] ─────────────────────► [30 | NULL]

```cpp
Node* toDelete = prev->next;
prev->next = prev->next->next;
delete toDelete;
```

In a **singly** linked list, finding `prev` is O(n) from the head. In a **doubly** linked list, you already have `prev` → true O(1).

### Search — O(n)

```cpp
Node* curr = head;
while (curr != nullptr) {
    if (curr->data == target) return curr;
    curr = curr->next;
}
return nullptr;
```

No shortcut. Cannot binary search a linked list — jumping to the middle is itself O(n).

---

## 📊 Complexity Table — Singly Linked List

| Operation | Time | Notes |
|---|---|---|
| Access `[i]` | O(n) | Walk from head |
| Search | O(n) | Walk and compare |
| Insert at head | O(1) | Update head |
| Insert at tail | O(n) / O(1) | O(1) with tail pointer |
| Insert at index k | O(k) | Walk + insert |
| Delete head | O(1) | Update head |
| Delete tail | O(n) | Walk to second-last |
| Delete at index k | O(k) | Walk + delete |

---

## 🐢 Why Arrays Often Win in Practice

Even when Big-O says linked lists are faster, arrays usually win on real hardware. Three reasons:

### 1. Cache Locality
CPUs fetch **cache lines** (64 bytes) at a time. Arrays are contiguous, so accessing `arr[0]` brings `arr[1]`, `arr[2]`, etc. into cache for free. Linked list nodes are scattered, so each `next` jump can cause a **cache miss** — 100x slower than a cache hit.

### 2. Heap Allocation Cost
Every `new Node{...}` is a heap allocation — 30-100 CPU cycles per node. An array of 1 million ints does **one** allocation. A linked list of 1 million nodes does **a million** allocations.

### 3. Memory Overhead
Each node carries:
- Data (e.g., 4 bytes for an int)
- Pointer (8 bytes on 64-bit systems)
- Allocator bookkeeping (16-24 bytes hidden per allocation)
- Padding for alignment

A linked list can use **5-10× more memory** than an equivalent array for small data types.

> **Big-O measures number of operations, not cost per operation.** Memory layout often matters more than asymptotic complexity for real performance.

---

## 🎯 When to Use Each

**Use arrays / `std::vector` when:**
- Need fast random access
- Most operations happen at the end
- Cache performance matters (almost always)

**Use linked lists when:**
- Constant insertion/deletion at head or mid-list, *and* you already have pointers there
- Building blocks for stacks, queues, or hash table chaining
- Random access isn't needed

**Honest reality:** In production code, arrays/vectors beat linked lists 90% of the time. Linked lists matter more as a **conceptual tool** — they're the foundation for trees, graphs, and many advanced structures.

---

## 🧩 Common Linked List Problem Patterns

You'll see these constantly on LeetCode:

1. **Two pointers (fast & slow)** — detect cycles, find middle node
2. **Reverse a linked list** — classic interview question
3. **Merge two sorted lists** — building block for merge sort
4. **Remove nth node from end** — two-pointer technique
5. **Detect cycle (Floyd's algorithm)** — tortoise and hare

We'll solve these with code in upcoming sessions.

---

## ✅ Key Takeaways

- A linked list is a chain of nodes connected by pointers — no contiguous memory
- Each node = **data + pointer to next** (plus `prev` in doubly linked)
- **Head pointer** is the entry point — lose it, lose the list
- **O(1) insert/delete** only when you already hold the relevant pointer
- **Cannot binary search** a linked list — random access is O(n)
- Arrays usually win in practice due to **cache locality**, **allocation cost**, and **memory overhead**
- Linked lists are more important conceptually — they're the foundation for trees, graphs, hash table chaining

---

## 📚 Next Up

→ [Chapter 07 — Stacks & Queues](../07-stacks-queues)
