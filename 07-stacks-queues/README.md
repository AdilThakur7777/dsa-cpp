# 📚 Chapter 07 — Stacks & Queues

> Two restricted data structures that show up in 30% of real-world algorithms. Master these and a huge chunk of LeetCode opens up.

---

## 🤔 The Two Natural Orders

When you collect things and remove them later, there are only two natural orderings:

1. **Newest first** — most recently added comes out first
2. **Oldest first** — longest-waiting comes out first

| Pattern | Order | Structure |
|---|---|---|
| Stack of plates, undo, browser back | Newest first | **Stack** |
| Coffee shop line, print jobs, BFS | Oldest first | **Queue** |

---

## 🔑 The Single Structural Difference

This is the foundation. Everything else flows from it:

> **A stack has ONE accessible end. A queue has TWO accessible ends — one for adding, one for removing.**

LIFO and FIFO aren't arbitrary rules — they're **consequences** of this design.

### Stack — one accessible end
    ┌────┐
    │ 30 │ ← TOP — only entry/exit point
    ├────┤
    │ 20 │
    ├────┤
    │ 10 │ ← BOTTOM — sealed
    └────┘

Newest item sits on top, blocking access to older ones → **LIFO** (Last In, First Out).

### Queue — two accessible ends
FRONT                              BACK
↓                                  ↓
[ 10 ][ 20 ][ 30 ][ 40 ][ 50 ]
↑                                  ↑
remove here only              add here only

New items enter at the **back**, exit from the **front**. The oldest item has been moving toward the exit the whole time → **FIFO** (First In, First Out).

---

## 📥 Stack — Operations

| Operation | What it does | Time |
|---|---|---|
| `push(x)` | Add x to the top | O(1) |
| `pop()` | Remove and return the top | O(1) |
| `top()` / `peek()` | Look at the top without removing | O(1) |

### Tracing example
push(10)  →  [10]
push(20)  →  [10, 20]
push(30)  →  [10, 20, 30]    top = 30
pop()     →  [10, 20]         returns 30
push(40)  →  [10, 20, 40]
pop()     →  [10, 20]         returns 40

### Why push and pop are O(1)

The top position is always known. No searching, no shifting. Same principle as **inserting at the end of an array** — both avoid shifting because the action happens at a known position.

Compare with arrays:
- Array insert at end → O(1)
- Array insert at index k → O(n − k) shifting
- **Stack push** → always at top → O(1) ✓

---

## 📤 Queue — Operations

| Operation | What it does | Time |
|---|---|---|
| `enqueue(x)` / `push(x)` | Add to back | O(1) |
| `dequeue()` / `pop()` | Remove and return from front | O(1) |
| `front()` | Peek at front | O(1) |
| `back()` | Peek at back | O(1) |

### Tracing example
enqueue(5)   →  [5]
enqueue(10)  →  [5, 10]
enqueue(15)  →  [5, 10, 15]
dequeue()    →  [10, 15]      returns 5  (oldest)
enqueue(20)  →  [10, 15, 20]
dequeue()    →  [15, 20]      returns 10

**Final state: front = 15, back = 20.** Always remember — dequeue takes the oldest (front), not the newest.

---

## ⚠️ The Naive Array Queue Problem

If you implement a queue with a plain array and dequeue from index 0:
Index:    0    1    2    3    4
Array:  [10][20][30][40][50]

Dequeue 10 → must shift everything left to keep front at index 0:
Step 1: arr[0] = arr[1]   →   20 moves to index 0
Step 2: arr[1] = arr[2]   →   30 moves to index 1
Step 3: arr[2] = arr[3]   →   40 moves to index 2
Step 4: arr[3] = arr[4]   →   50 moves to index 3

That's **n − 1 shifts → O(n)**. Same problem as array insert-at-beginning.

This breaks the whole purpose of a queue.

---

## 🔄 The Circular Array Fix

Don't shift. Let `front` and `back` move forward as the queue operates. When either hits the end of the array, **wrap to index 0**.
Initial:
Index:    0    1    2    3    4
Array:  [10][20][30][40][50]
front=0, back=5
dequeue() → returns 10:
Index:    0    1    2    3    4
Array:  [  ][20][30][40][50]
front=1, back=5
enqueue(60) (back wraps to index 0):
Index:    0    1    2    3    4
Array:  [60][  ][30][40][50]
↑         ↑
back=1    front=2

The array is treated like a **circle** — front and back chase each other around.

### The modulo trick
back  = (back + 1) % capacity
front = (front + 1) % capacity

- `back = 2, capacity = 5` → `(2+1) % 5 = 3` → moves forward
- `back = 4, capacity = 5` → `(4+1) % 5 = 0` → wraps to start

**Result:** both enqueue and dequeue are now O(1). This is how real queues are implemented in OS kernels, network buffers, and embedded systems.

---

## 🏗️ Implementation Choices

### Stack — using a linked list

If using a linked list, push and pop happen at the **head**, not the tail. Why? Because in a singly linked list:
- Insert at head → O(1)
- Insert at tail → O(n) (must walk to end)

Stack needs O(1) on both push and pop, so head is the only choice. The head acts as the "top."
top → [30] → [20] → [10] → NULL

### Queue — using a linked list

Need both a **head** pointer (for dequeue) AND a **tail** pointer (for enqueue) to keep both operations O(1).

### In modern C++

You'd typically use the standard library:

```cpp
#include <stack>
stack<int> s;
s.push(10); s.push(20);
s.top();      // 20
s.pop();      // removes 20

#include <queue>
queue<int> q;
q.push(10); q.push(20);
q.front();    // 10
q.pop();      // removes 10
```

Both `std::stack` and `std::queue` wrap `std::deque` by default — a chunk-based dynamic array that combines fast access with dynamic growth.

> **Interview note:** even though you'd use the standard library in production, you should be able to implement a stack and queue from scratch using both an array and a linked list. Classic interview question.

---

## 🌍 Real-World Uses

### Stack

**1. The function call stack — the most important one**

When function A calls B calls C, the OS pushes a "stack frame" for each:
    ┌────┐
    │ C  │ ← top (currently executing)
    ├────┤
    │ B  │
    ├────┤
    │ A  │
    └────┘

When C returns, it's popped first (LIFO), and B resumes exactly where it left off. **Recursion works because of this stack.** Too much recursion → stack overflow.

It must be a stack (not a queue) because the most recently called function must complete first before its caller can resume. If it were a queue, B would try to resume before C finished — chaos.

**2. Undo / redo** — every action is pushed; Ctrl+Z pops the most recent.

**3. Browser back button** — each page visited gets pushed; back pops the most recent.

**4. Balanced parentheses** — push openers, pop and match on closers. If you pop from empty or end non-empty → unbalanced.

**5. DFS (Depth-First Search)** — graph/tree traversal that "goes deep" before going wide. Implicit stack (recursion) or explicit stack (iterative).

### Queue

**1. BFS (Breadth-First Search) — the most important algorithmic use of queues**

Graph and tree traversal that visits everything at distance 1, then 2, then 3, etc.
Add starting node to queue.
While queue not empty:
- Dequeue a node
- Process it
- Enqueue all unvisited neighbors

BFS solves "shortest path in an unweighted graph" in O(V + E). Comes up in maze problems, level-order tree traversal, rotting oranges, etc.

**2. Task scheduling** — print jobs, OS schedulers, message queues (Kafka, RabbitMQ at scale).

**3. Server request handling** — web servers and load balancers process requests in order.

**4. Buffering** — video/audio streaming uses queues between fast producer (network) and slow consumer (your speakers).

---

## 🌀 Important Variants

### Deque (Double-Ended Queue)

Add or remove from **both ends** in O(1).

```cpp
#include <deque>
deque<int> d;
d.push_front(10);   // O(1)
d.push_back(20);    // O(1)
d.pop_front();      // O(1)
d.pop_back();       // O(1)
```

Used heavily in **sliding window** problems (e.g., max in every window of size k). More general than `std::stack` and `std::queue` — both are built on top of it.

### Priority Queue

Element with **highest priority** comes out first, regardless of insertion order.

```cpp
#include <queue>
priority_queue<int> pq;   // max-heap by default
pq.push(30);
pq.push(10);
pq.push(50);
pq.top();   // 50 (highest)
```

Implementation: a **heap** (tree-based, covered in Chapter 11). Operations are **O(log n)**, not O(1) — the cost of maintaining priority order.

**Trade-off you're paying for:** ordering by priority instead of insertion order. You give up constant-time ops, you gain the ability to always pull the most-important item.

Used in: Dijkstra's algorithm, A* pathfinding, top-K problems, median of a stream, task scheduling with priorities.

### Circular Queue

Fixed-size queue where back wraps to front. Used in embedded systems and OS kernels where memory must be predictable.

---

## 🧩 Common Problem Patterns

You'll see these constantly on LeetCode:

### Stack patterns
1. **Balanced parentheses / brackets** — push opens, pop and match
2. **Next greater / smaller element** — monotonic stack
3. **Evaluate postfix expression** — push operands, apply on operator
4. **Min stack** — track min at every level with auxiliary stack
5. **Stock span / largest rectangle in histogram** — monotonic stack
6. **Trapping rain water** (one approach) — monotonic stack
7. **Convert recursion to iteration** — simulate call stack explicitly

### Queue patterns
1. **BFS on graphs and trees** — level-by-level traversal
2. **Sliding window max / min** — using deque
3. **Rotting oranges / shortest path in grid** — multi-source BFS
4. **First non-repeating character in a stream** — queue + frequency map
5. **Task scheduling with priorities** — priority queue
6. **Top K frequent elements** — priority queue
7. **Median of stream** — two heaps

---

## 📊 Complexity Summary

### Stack
| Operation | Time |
|---|---|
| Push | O(1) |
| Pop | O(1) |
| Top | O(1) |
| Search | O(n) |

### Queue (circular array or linked list)
| Operation | Time |
|---|---|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Front / Back | O(1) |
| Search | O(n) |

### Priority Queue (heap-based)
| Operation | Time |
|---|---|
| Insert | O(log n) |
| Extract max/min | O(log n) |
| Peek | O(1) |

---

## ✅ Key Takeaways

- **Stack = one end** accessible. **Queue = two ends** (one for add, one for remove)
- LIFO and FIFO are **consequences** of this structural choice, not arbitrary rules
- All core operations on stacks and queues are **O(1)** — this is the payoff for restricting access
- **Naive array queues are O(n) on dequeue** because of shifting. **Circular arrays fix this** using the modulo trick
- **Linked-list stack** uses the head as top. **Linked-list queue** needs both head AND tail pointers
- The **function call stack** is literally a stack — recursion runs on it
- **BFS uses a queue**, **DFS uses a stack** (implicit via recursion or explicit)
- **Priority queue** trades O(1) ops for O(log n) to gain priority-based ordering
- In C++: `std::stack`, `std::queue`, `std::deque`, `std::priority_queue` — use them, don't reinvent

---

## 📚 Next Up

→ [Chapter 08 — Recursion](../08-recursion)
