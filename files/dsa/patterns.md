# DSA Patterns - Detailed Guide

For each pattern: what it is, when to use it, how it works, trigger words to recognize it, and example problems sorted by difficulty.

> New to DSA? Learn each pattern step by step in the course: [00. Start Here](./learn/00-start-here.md). For how to run the actual interview (clarify, plan, code, test, communicate) see [25. Interview Playbook](./learn/25-interview-playbook.md).

---

## 1. ARRAYS & STRINGS

### 1.1 Sliding Window

**What:** Maintain a window of elements that slides across the array/string.

**When to use:**
- Contiguous subarrays or substrings
- Find max/min/length/count within a region

**How it works:**
```
[  window  ]
 left    right  -->  expand right, shrink left when constraint breaks
```

**Trigger words:** "longest substring", "maximum sum subarray", "at most K"

**Complexity:** O(n) time, O(k) space where k = window content

**Example problems:**
- Easy: Max Sum Subarray of Size K
- Medium: Longest Substring Without Repeating Characters, Fruits Into Baskets
- Hard: Minimum Window Substring

> **Key Interview Point:** Always ask yourself: what shrinks the window? That determines your inner while-loop condition.

---

### 1.2 Two Pointers

**What:** Two pointers iterate from different positions or at different speeds.

**When to use:** Sorted array, in-place operations, pair comparisons

**How it works:**
```
[1, 2, 3, 4, 5, 6, 7]
 ^                 ^
left             right   -->  move inward based on condition
```

**Trigger words:** "sorted array", "in-place", "without extra space", "pairs"

**Complexity:** O(n) time, O(1) space

**Example problems:**
- Easy: Remove Duplicates from Sorted Array, Reverse String
- Medium: Two Sum II, Container With Most Water, 3Sum

> **Key Interview Point:** Two pointers on a sorted array often replaces an O(n^2) brute force with O(n).

---

### 1.3 Binary Search

**What:** Halve the search space each step to find a target or boundary.

**When to use:** Sorted array or monotonic function, need O(log n)

**How it works:**
```
[1, 3, 5, 7, 9, 11]
 ^        ^       ^
low      mid    high   -->  compare mid, adjust low/high
```

**Trigger words:** "sorted", "find minimum/maximum", "search", "O(log n)"

**Complexity:** O(log n) time, O(1) space

> **Common Mistake:** Off-by-one errors. Use `left + (right - left) / 2` to avoid overflow. Be clear whether your search is `left <= right` or `left < right`.

**Example problems:**
- Easy: Binary Search, Search Insert Position
- Medium: Search in Rotated Sorted Array, Find Minimum in Rotated Sorted Array, Koko Eating Bananas
- Hard: Median of Two Sorted Arrays

---

### 1.4 Prefix Sum

**What:** Precompute cumulative sums for O(1) range queries.

**How it works:**
```
arr:       [1, 2, 3, 4, 5]
prefix:  [0, 1, 3, 6, 10, 15]
sum(i..j) = prefix[j+1] - prefix[i]
```

**Trigger words:** "sum from i to j", "count subarrays with sum K"

**Example problems:**
- Easy: Range Sum Query
- Medium: Subarray Sum Equals K

---

### 1.5 Kadane's Algorithm

**What:** Find maximum sum subarray in O(n).

**How it works:** Track `currentSum` and `maxSum`. If the running sum so far is negative, drop it and start fresh at the current element.

```
currentSum = max(nums[i], currentSum + nums[i])
maxSum = max(maxSum, currentSum)
```

**Example problems:** Maximum Subarray (#53)

---

### 1.6 Merge Intervals

**What:** Merge overlapping intervals.

**How it works:**
1. Sort by start time
2. If `current.end >= next.start`, merge (extend end)
3. Otherwise, start new interval

**Trigger words:** "intervals", "merge", "overlap", "meeting rooms"

**Example problems:**
- Medium: Merge Intervals, Insert Interval
- Medium: Meeting Rooms II

---

### 1.7 Cyclic Sort

**What:** Place each number at its correct index for arrays with values 1..N.

**Trigger words:** "1 to N", "find missing/duplicate", "constant space"

**Example problems:** Missing Number (#268), Find All Duplicates in an Array (#442), First Missing Positive (#41, Hard)

---

### 1.8 Dutch National Flag (3-Way Partition)

**What:** Sort array with only 3 distinct values using 3 pointers: `low`, `mid`, `high`.

**Example problems:** Sort Colors (#75)

---

## 2. LINKED LISTS

### 2.1 Fast & Slow Pointers (Tortoise & Hare)

**What:** Two pointers moving at different speeds.

**When to use:** Cycle detection, finding middle, intersection

**How it works:**
```
slow: 1 -> 2 -> 3 -> 4
fast: 1 ------> 3 ------> (end or meets slow)
```
- **Middle:** Stop when fast reaches end
- **Cycle:** They meet inside the cycle
- **Cycle start:** After meeting, reset one to head, move both by 1

**Complexity:** O(n) time, O(1) space

**Example problems:** Linked List Cycle, Find Middle, Cycle II, Intersection of Two Lists

> **Key Interview Point:** For finding cycle start -- after fast and slow meet, reset slow to head, advance both by 1. They meet at cycle start. Know the math proof.

---

### 2.2 Reversing a Linked List

**How it works:**
```
prev = null, curr = head
while curr:
    next = curr.next    // save next
    curr.next = prev    // reverse pointer
    prev = curr         // advance prev
    curr = next         // advance curr
return prev             // new head
```

**Example problems:** Reverse Linked List, Reverse II, Palindrome Linked List

> **Common Mistake:** Forgetting to save `curr.next` before overwriting it.

---

### 2.3 Merging Linked Lists

**How it works:** Dummy node + compare values, attach smaller.

**Example problems:** Merge Two Sorted Lists, Merge K Sorted Lists (use min heap)

---

### 2.4 Dummy Node Technique

**What:** Fake head node to simplify edge cases.

```
dummy -> [real list]
return dummy.next
```

**When to use:** Any time the head might change (deletions, insertions at front).

---

## 3. STACKS & QUEUES

### 3.1 Monotonic Stack

**What:** Stack that maintains elements in sorted order (increasing or decreasing).

**When to use:** Next greater/smaller element problems

**How it works:**
```
For each element:
  While stack top is smaller/larger than current:
    Pop and record answer for popped element
  Push current onto stack
```

**Complexity:** O(n) -- each element pushed and popped at most once

**Example problems:**
- Medium: Daily Temperatures, Next Greater Element
- Hard: Largest Rectangle in Histogram

---

### 3.2 Stack for Parsing

**When to use:** Parentheses validation, decoding nested strings

**Example problems:** Valid Parentheses, Decode String (`"3[a2[c]]"`)

---

### 3.3 BFS with Queue

**What:** Explore nodes level by level using a queue.

**When to use:** Shortest path (unweighted), level-order traversal

**How it works:**
```
queue = [start]; visited = {start}
while queue not empty:
    node = queue.dequeue()
    for each neighbor:
        if neighbor not in visited:
            visited.add(neighbor)      // mark when ENQUEUED, not when dequeued
            queue.enqueue(neighbor)
```

**Example problems:** Number of Islands, Shortest Path in Binary Matrix, Word Ladder

---

### 3.4 Deque for Sliding Window Max/Min

**What:** Maintain a deque of indices where values are in decreasing order.

**Example problems:** Sliding Window Maximum

---

### 3.5 Multi-Stack Simulation

**Example problems:** Min Stack, Queue Using Two Stacks

---

## 4. TREES

### 4.1 DFS (Recursive)

**What:** Explore deeply before backtracking. Process subtrees and combine results.

**Traversal orders:**
```
       1
      / \
     2   3

Preorder:  1, 2, 3  (Node, Left, Right)
Inorder:   2, 1, 3  (Left, Node, Right)  <-- sorted for BST
Postorder: 2, 3, 1  (Left, Right, Node)
```

**Example problems:** Max Depth, Path Sum, Symmetric Tree, LCA

> **Key Interview Point:** Most tree problems are DFS. Ask: do I need info from children (postorder) or pass info down (preorder)?

---

### 4.2 BFS (Level Order)

**When to use:** Anything that says "level", "minimum depth", "left/right view"

**Template:**
```
queue = [root]
while queue:
    levelSize = queue.size
    for i in 0..levelSize:
        node = queue.dequeue()
        process(node)
        enqueue children
```

**Example problems:** Level Order Traversal, Min Depth, Right Side View, Zigzag

---

### 4.3 BST Invariants

**Key property:** Inorder traversal of BST gives sorted order.

**Example problems:** Validate BST, Kth Smallest in BST, LCA in BST

---

### 4.4 Serialization

**Example problems:** Serialize and Deserialize Binary Tree

---

## 5. GRAPHS

### 5.1 DFS in Graphs

**When to use:** Explore connected components, detect cycles

**For cycle detection in directed graph:** Use visited + recursion stack (two arrays).

**Example problems:** Number of Islands (DFS), Clone Graph, Detect Cycle

---

### 5.2 BFS in Graphs

**When to use:** Shortest path (unweighted), multi-source BFS

**Example problems:** Word Ladder, Rotten Oranges (multi-source), Shortest Path in Binary Matrix

---

### 5.3 Topological Sort

**What:** Order nodes so dependencies come before dependents. Only works on DAGs.

**Two approaches:**
1. **Kahn's (BFS):** Track in-degrees, process nodes with in-degree 0
2. **DFS:** Process in reverse postorder

**Example problems:** Course Schedule I & II, Alien Dictionary

> **Key Interview Point:** If result size != number of nodes, there is a cycle (impossible to order).

---

### 5.4 Union-Find (Disjoint Set)

**What:** Group nodes into components. Two operations: `find()` and `union()`.

**Optimizations:** Path compression + union by rank -> nearly O(1) per operation.

```
find(x): follow parent pointers to root (with path compression)
union(x, y): merge roots (by rank)
```

**Example problems:** Number of Connected Components in an Undirected Graph, Redundant Connection, Accounts Merge

---

### 5.5 Flood Fill / Connected Components

**What:** Count or label clusters in a grid using DFS or BFS.

**Example problems:** Number of Islands, Surrounded Regions, Flood Fill

---

## 6. RECURSION & BACKTRACKING

### 6.1 Backtracking Template

```
function backtrack(path, choices):
    if isComplete(path):
        result.add(copy of path)
        return
    for choice in choices:
        make(choice)           // choose
        backtrack(updated)     // explore
        undo(choice)           // unchoose
```

**When to use:** "Return all combinations/permutations/solutions"

**Example problems:**
- Medium: Subsets, Permutations, Combination Sum, Letter Combinations
- Hard: N-Queens, Sudoku Solver

> **Common Mistake:** Forgetting to make a copy of `path` when adding to result. Forgetting to undo the choice (backtrack step).

---

### 6.2 Constraint Pruning

**What:** Cut off branches early when constraints are violated.

**How:** Sort first, then skip duplicates with `if (i > start && candidates[i] == candidates[i-1]) continue;`

**Example problems:** Combination Sum II, Palindrome Partitioning

---

## 7. DYNAMIC PROGRAMMING

### 7.1 When to Use DP

- Problem has **optimal substructure** (optimal solution uses optimal sub-solutions)
- Problem has **overlapping subproblems** (same subproblems solved multiple times)
- Problem asks for **max/min/count/can-do**

### 7.2 Approach

1. **Define state:** What does `dp[i]` represent?
2. **Recurrence:** How does `dp[i]` relate to previous states?
3. **Base case:** What is `dp[0]`?
4. **Order:** Fill bottom-up or use memoization top-down
5. **Optimize space** if possible (often only need previous row/value)

### 7.3 Common Patterns

| Pattern | State | Recurrence | Example |
|---------|-------|------------|---------|
| 1D Sequence | `dp[i]` = best up to index i | `dp[i] = f(dp[i-1], dp[i-2])` | House Robber, Climbing Stairs |
| 2D Grid | `dp[i][j]` = best to reach cell (i,j) | `dp[i][j] = f(dp[i-1][j], dp[i][j-1])` | Unique Paths, Min Path Sum |
| Knapsack | `dp[j]` = can achieve sum j | `dp[j] = dp[j] or dp[j-num]` | Partition Equal Subset Sum |
| String DP | `dp[i][j]` = result for s1[0..i] vs s2[0..j] | Compare chars, take best | LCS, Edit Distance |
| Memoization | `memo[state]` = cached result | Top-down recursion with cache | Word Break, Fibonacci |

> **Key Interview Point:** Start with brute-force recursion, identify overlapping subproblems, add memoization, then convert to bottom-up if needed.

> **Common Mistake:** In knapsack-type problems, iterate capacity backwards (right to left) to avoid using an item twice.
