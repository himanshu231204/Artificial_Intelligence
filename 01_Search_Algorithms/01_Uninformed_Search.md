# Uninformed Search Algorithms 🔍

> **Complete guide for GATE DA preparation with theory, numericals, and implementations**

---

## Table of Contents
1. [Introduction](#introduction)
2. [Breadth First Search (BFS)](#breadth-first-search-bfs)
3. [Depth First Search (DFS)](#depth-first-search-dfs)
4. [Depth Limited Search (DLS)](#depth-limited-search-dls)
5. [Iterative Deepening DFS (IDDFS)](#iterative-deepening-dfs-iddfs)
6. [Uniform Cost Search (UCS)](#uniform-cost-search-ucs)
7. [Bidirectional Search](#bidirectional-search)
8. [Comparison Table](#comparison-table)
9. [GATE Previous Year Questions](#gate-previous-year-questions)
10. [Practice Problems](#practice-problems)

---

## Introduction

### What is Uninformed Search?

**Uninformed search** (also called **blind search**) strategies have no additional information about states beyond that provided in the problem definition. They can only:
- Generate successors
- Distinguish goal state from non-goal state

**Key Properties to Evaluate:**
1. **Completeness**: Is the algorithm guaranteed to find a solution?
2. **Optimality**: Does it find the optimal (least-cost) solution?
3. **Time Complexity**: How long does it take?
4. **Space Complexity**: How much memory is needed?

**Notations:**
- **b** = branching factor (maximum number of successors)
- **d** = depth of shallowest goal node
- **m** = maximum depth of the search tree
- **l** = depth limit

---

## Breadth First Search (BFS)

### 🎯 Concept

BFS expands the **shallowest** unexpanded node. It explores all nodes at depth `d` before exploring any node at depth `d+1`.

**Think of it as:** Ripples expanding in water when you drop a stone.

### Algorithm

```
BFS(start, goal):
    queue ← [start]
    visited ← empty set
    
    while queue is not empty:
        node ← queue.dequeue()
        
        if node is goal:
            return SUCCESS
        
        if node not in visited:
            visited.add(node)
            for each child of node:
                queue.enqueue(child)
    
    return FAILURE
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ✅ Yes | Will find solution if exists |
| **Optimal** | ✅ Yes* | *Only if all step costs are equal |
| **Time** | O(b^d) | Explores all nodes up to depth d |
| **Space** | O(b^d) | Must store all nodes at current level |

### 🔢 GATE Type Numerical Example

**Question:** Consider the following graph. Find the path from S to G using BFS. Also calculate the number of nodes expanded.

```
        S
      / | \
     A  B  C
    /|  |  |\
   D E  F  G H
```

**Solution:**

**Step-by-step execution:**

| Step | Queue | Visited | Current | Goal Found? |
|------|-------|---------|---------|-------------|
| 0 | [S] | [] | - | No |
| 1 | [A, B, C] | [S] | S | No |
| 2 | [B, C, D, E] | [S, A] | A | No |
| 3 | [C, D, E, F] | [S, A, B] | B | No |
| 4 | [D, E, F, G, H] | [S, A, B, C] | C | No |
| 5 | [E, F, G, H] | [S, A, B, C, D] | D | No |
| 6 | [F, G, H] | [S, A, B, C, D, E] | E | No |
| 7 | [G, H] | [S, A, B, C, D, E, F] | F | No |
| 8 | [H] | [S, A, B, C, D, E, F, G] | **G** | **YES!** |

**Answer:**
- **Path found:** S → C → G
- **Nodes expanded:** 8 (S, A, B, C, D, E, F, G)
- **Path length:** 2

### 💻 Python Implementation

```python
from collections import deque

def bfs(graph, start, goal):
    """
    BFS implementation with path tracking
    
    Args:
        graph: dict with adjacency list
        start: starting node
        goal: goal node
    
    Returns:
        path: list of nodes from start to goal
        nodes_expanded: count of nodes explored
    """
    queue = deque([[start]])
    visited = set()
    nodes_expanded = 0
    
    while queue:
        path = queue.popleft()
        node = path[-1]
        
        if node == goal:
            return path, nodes_expanded
        
        if node not in visited:
            visited.add(node)
            nodes_expanded += 1
            
            for neighbor in graph.get(node, []):
                if neighbor not in visited:
                    new_path = list(path)
                    new_path.append(neighbor)
                    queue.append(new_path)
    
    return None, nodes_expanded

# Example
graph = {
    'S': ['A', 'B', 'C'],
    'A': ['D', 'E'],
    'B': ['F'],
    'C': ['G', 'H'],
    'D': [], 'E': [], 'F': [], 'G': [], 'H': []
}

path, expanded = bfs(graph, 'S', 'G')
print(f"Path: {' → '.join(path)}")
print(f"Nodes expanded: {expanded}")
```

### 📝 Key Points for GATE

1. **BFS always finds shortest path** when all costs are equal
2. **Uses Queue (FIFO)** - First In First Out
3. **Memory intensive** - stores all frontier nodes
4. **Level-order traversal** in trees
5. **Time = Space = O(b^d)** - major disadvantage

### Common GATE Questions Pattern

**Type 1:** Which search finds shortest path in unweighted graph?
- **Answer:** BFS ✅

**Type 2:** What is space complexity of BFS?
- **Answer:** O(b^d)

**Type 3:** Given a tree, find BFS traversal order
- **Answer:** Level by level from left to right

---

## Depth First Search (DFS)

### 🎯 Concept

DFS expands the **deepest** unexpanded node. It explores as far as possible along each branch before backtracking.

**Think of it as:** Exploring a maze by always taking the first unexplored path until you hit a dead end.

### Algorithm

```
DFS(start, goal):
    stack ← [start]
    visited ← empty set
    
    while stack is not empty:
        node ← stack.pop()
        
        if node is goal:
            return SUCCESS
        
        if node not in visited:
            visited.add(node)
            for each child of node (in reverse):
                stack.push(child)
    
    return FAILURE
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ❌ No | Can get stuck in infinite loops |
| **Optimal** | ❌ No | May not find shortest path |
| **Time** | O(b^m) | May explore entire tree |
| **Space** | O(bm) | Only stores single path |

### 🔢 GATE Type Numerical Example

**Question:** For the same graph, find path using DFS. Compare with BFS.

```
        S
      / | \
     A  B  C
    /|  |  |\
   D E  F  G H
```

**Solution:**

**Step-by-step execution (assuming left-to-right order):**

| Step | Stack | Visited | Current | Action |
|------|-------|---------|---------|--------|
| 0 | [S] | [] | - | Start |
| 1 | [C, B, A] | [S] | S | Push children (reverse) |
| 2 | [C, B, E, D] | [S, A] | A | Push children |
| 3 | [C, B, E] | [S, A, D] | D | No children |
| 4 | [C, B] | [S, A, D, E] | E | No children |
| 5 | [C, F] | [S, A, D, E, B] | B | Push F |
| 6 | [C] | [S, A, D, E, B, F] | F | No children |
| 7 | [H, G] | [S, A, D, E, B, F, C] | C | Push children |
| 8 | [H] | [S, A, D, E, B, F, C, G] | **G** | **FOUND!** |

**Answer:**
- **Path found:** S → C → G
- **Nodes expanded:** 8
- **Comparison:** Same result, but DFS uses less memory

### 💻 Python Implementation

```python
def dfs_recursive(graph, node, goal, visited=None, path=None):
    """
    Recursive DFS implementation
    """
    if visited is None:
        visited = set()
    if path is None:
        path = []
    
    visited.add(node)
    path = path + [node]
    
    if node == goal:
        return path
    
    for neighbor in graph.get(node, []):
        if neighbor not in visited:
            result = dfs_recursive(graph, neighbor, goal, visited, path)
            if result is not None:
                return result
    
    return None

def dfs_iterative(graph, start, goal):
    """
    Iterative DFS using stack
    """
    stack = [[start]]
    visited = set()
    nodes_expanded = 0
    
    while stack:
        path = stack.pop()
        node = path[-1]
        
        if node == goal:
            return path, nodes_expanded
        
        if node not in visited:
            visited.add(node)
            nodes_expanded += 1
            
            # Add children in reverse order for left-to-right exploration
            for neighbor in reversed(graph.get(node, [])):
                if neighbor not in visited:
                    new_path = list(path)
                    new_path.append(neighbor)
                    stack.append(new_path)
    
    return None, nodes_expanded

# Example
graph = {
    'S': ['A', 'B', 'C'],
    'A': ['D', 'E'],
    'B': ['F'],
    'C': ['G', 'H'],
    'D': [], 'E': [], 'F': [], 'G': [], 'H': []
}

path, expanded = dfs_iterative(graph, 'S', 'G')
print(f"Path: {' → '.join(path)}")
print(f"Nodes expanded: {expanded}")
```

### 📝 Key Points for GATE

1. **DFS uses Stack (LIFO)** - Last In First Out
2. **Space efficient** - O(bm) vs O(b^d) for BFS
3. **Not complete** - can loop infinitely
4. **Not optimal** - finds any solution, not best
5. **Good for:** Game trees, topological sorting, cycle detection

### 🎯 BFS vs DFS Quick Comparison

| Aspect | BFS | DFS |
|--------|-----|-----|
| Data Structure | Queue | Stack |
| Memory | High O(b^d) | Low O(bm) |
| Shortest Path | ✅ Yes | ❌ No |
| Complete | ✅ Yes | ❌ No |
| Use Case | Shortest path | Deep solutions |

---

## Depth Limited Search (DLS)

### 🎯 Concept

DLS is DFS with a **depth limit** `l`. It stops exploring at depth `l` to prevent infinite descent.

**Think of it as:** Exploring a cave but only going N levels deep.

### Algorithm

```
DLS(node, goal, limit):
    if node == goal:
        return SUCCESS
    
    if limit == 0:
        return CUTOFF
    
    for each child of node:
        result = DLS(child, goal, limit - 1)
        if result == SUCCESS:
            return SUCCESS
        if result == CUTOFF:
            cutoff_occurred = true
    
    if cutoff_occurred:
        return CUTOFF
    else:
        return FAILURE
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ❌ No | If l < d, won't find solution |
| **Optimal** | ❌ No | May not find shortest path |
| **Time** | O(b^l) | Explores up to depth l |
| **Space** | O(bl) | Linear space |

### 🔢 GATE Type Numerical Example

**Question:** In the graph below, apply DLS with limit = 2. Will it find G?

```
        S (depth 0)
      / | \
     A  B  C (depth 1)
    /|  |  |\
   D E  F  G H (depth 2)
   |
   I (depth 3)
```

**Solution:**

**With limit = 2:**

| Depth | Nodes Explored | G Found? |
|-------|----------------|----------|
| 0 | S | No |
| 1 | A, B, C | No |
| 2 | D, E, F, G, H | **Yes!** |
| 3 | CUTOFF | - |

**Answer:**
- **G found:** ✅ Yes (at depth 2)
- **Path:** S → C → G
- **I found:** ❌ No (beyond limit)

**With limit = 1:**
- **G found:** ❌ No (beyond limit)
- **Nodes explored:** S, A, B, C

### 💻 Python Implementation

```python
def depth_limited_search(graph, start, goal, limit):
    """
    DLS implementation
    
    Returns:
        path: if found
        'CUTOFF': if depth limit reached
        None: if not found within limit
    """
    def dls_recursive(node, goal, depth, path, visited):
        if node == goal:
            return path + [node]
        
        if depth >= limit:
            return 'CUTOFF'
        
        if node in visited:
            return None
        
        visited.add(node)
        cutoff_occurred = False
        
        for neighbor in graph.get(node, []):
            result = dls_recursive(neighbor, goal, depth + 1, 
                                  path + [node], visited.copy())
            if result != None and result != 'CUTOFF':
                return result
            if result == 'CUTOFF':
                cutoff_occurred = True
        
        return 'CUTOFF' if cutoff_occurred else None
    
    return dls_recursive(start, goal, 0, [], set())

# Example
graph = {
    'S': ['A', 'B', 'C'],
    'A': ['D', 'E'],
    'B': ['F'],
    'C': ['G', 'H'],
    'D': ['I'],
    'E': [], 'F': [], 'G': [], 'H': [], 'I': []
}

# Test with different limits
for limit in [1, 2, 3]:
    result = depth_limited_search(graph, 'S', 'G', limit)
    print(f"Limit {limit}: {result}")
```

### 📝 Key Points for GATE

1. **Solves infinite path problem** of DFS
2. **Diameter of state space** is good choice for limit
3. **Returns CUTOFF** vs FAILURE (important distinction)
4. **Used in Iterative Deepening** (next topic)

---

## Iterative Deepening DFS (IDDFS)

### 🎯 Concept

IDDFS repeatedly applies DLS with **increasing depth limits** (0, 1, 2, ...) until goal is found.

**Advantage:** Combines benefits of BFS (optimal, complete) and DFS (low memory).

### Algorithm

```
IDDFS(start, goal):
    for depth = 0 to ∞:
        result = DLS(start, goal, depth)
        if result != CUTOFF:
            return result
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ✅ Yes | Will eventually find solution |
| **Optimal** | ✅ Yes | Finds shallowest goal |
| **Time** | O(b^d) | Same as BFS |
| **Space** | O(bd) | Much better than BFS! |

### 🔢 GATE Type Numerical Example

**Question:** Apply IDDFS on the graph. How many nodes are generated?

```
        S
      / | \
     A  B  C
    /   |   \
   D    E    G
```

**Solution:**

**Iteration with depth = 0:**
- Nodes generated: S
- Goal found: No

**Iteration with depth = 1:**
- Nodes generated: S, A, B, C
- Goal found: No

**Iteration with depth = 2:**
- Nodes generated: S, A, B, C, D, E, G
- Goal found: **Yes! (G at depth 2)**

**Total nodes generated:**
- Depth 0: 1 node
- Depth 1: 1 + 3 = 4 nodes
- Depth 2: 1 + 3 + 3 = 7 nodes
- **Total: 1 + 4 + 7 = 12 nodes**

**Answer:**
- **Goal found at:** Depth 2
- **Path:** S → C → G
- **Total nodes generated:** 12

### 📊 Node Generation Formula

For a uniform tree with branching factor `b` and solution at depth `d`:

**Nodes generated = (d+1)b^0 + d·b^1 + (d-1)·b^2 + ... + 1·b^d**

For b=2, d=3:
- N = 4(1) + 3(2) + 2(4) + 1(8) = 4 + 6 + 8 + 8 = **26 nodes**

### 💻 Python Implementation

```python
def iterative_deepening_dfs(graph, start, goal, max_depth=100):
    """
    IDDFS implementation
    
    Returns:
        (path, depth, total_nodes_generated)
    """
    total_nodes = 0
    
    for depth in range(max_depth):
        result = depth_limited_search(graph, start, goal, depth)
        
        # Count nodes generated at this depth
        nodes_at_depth = count_nodes_at_depth(graph, start, depth)
        total_nodes += nodes_at_depth
        
        if result != 'CUTOFF' and result != None:
            return result, depth, total_nodes
    
    return None, -1, total_nodes

def count_nodes_at_depth(graph, start, max_depth):
    """Helper to count nodes generated"""
    count = 0
    
    def count_recursive(node, depth, visited):
        nonlocal count
        if depth > max_depth or node in visited:
            return
        
        count += 1
        visited.add(node)
        
        for neighbor in graph.get(node, []):
            count_recursive(neighbor, depth + 1, visited.copy())
    
    count_recursive(start, 0, set())
    return count

# Example
graph = {
    'S': ['A', 'B', 'C'],
    'A': ['D'],
    'B': ['E'],
    'C': ['G'],
    'D': [], 'E': [], 'G': []
}

path, depth, total = iterative_deepening_dfs(graph, 'S', 'G')
print(f"Path: {path}")
print(f"Found at depth: {depth}")
print(f"Total nodes generated: {total}")
```

### 📝 Key Points for GATE

1. **Best uninformed search** for large state spaces
2. **Appears wasteful** but overhead is not as bad as it seems
3. **Preferred when depth unknown** and space is limited
4. **Time complexity same as BFS** but **space much better**
5. **Generates nodes multiple times** at shallow levels

### 🎯 Why IDDFS is Not Wasteful

For b=10, d=5:
- Nodes at depth 5: 100,000
- Nodes at depth 4: 10,000
- Nodes at depth 3: 1,000
- **Most work at deepest level!**
- Repeating shallow levels adds only ~11% overhead

---

## Uniform Cost Search (UCS)

### 🎯 Concept

UCS expands the node with **lowest path cost** g(n). It's like BFS but uses a **priority queue** instead of regular queue.

**Think of it as:** Always choosing the cheapest route, regardless of direction.

### Algorithm

```
UCS(start, goal):
    priority_queue ← [(0, start)]
    visited ← empty set
    
    while priority_queue is not empty:
        (cost, node) ← priority_queue.pop_min()
        
        if node is goal:
            return SUCCESS
        
        if node not in visited:
            visited.add(node)
            for each child, edge_cost of node:
                new_cost = cost + edge_cost
                priority_queue.push((new_cost, child))
    
    return FAILURE
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ✅ Yes | If step cost ≥ ε > 0 |
| **Optimal** | ✅ Yes | Finds least-cost path |
| **Time** | O(b^(1+⌊C*/ε⌋)) | C* = cost of optimal solution |
| **Space** | O(b^(1+⌊C*/ε⌋)) | Must store all frontier |

### 🔢 GATE Type Numerical Example

**Question:** Find the path from S to G using UCS. What is the optimal cost?

```
Graph with edge costs:

      S
    /   \
   1     4
  /       \
 A         B
  \       /
   2     1
    \   /
      C
      |
      3
      |
      G
```

**Edge costs:**
- S → A: 1
- S → B: 4
- A → C: 2
- B → C: 1
- C → G: 3

**Solution:**

| Step | Priority Queue (cost, node) | Visited | Current | Path Cost | Expand |
|------|----------------------------|---------|---------|-----------|--------|
| 0 | [(0, S)] | [] | - | - | - |
| 1 | [(1, A), (4, B)] | [S] | S | 0 | Add A(cost 1), B(cost 4) |
| 2 | [(3, C), (4, B)] | [S, A] | A | 1 | Add C via A(1+2=3) |
| 3 | [(4, B), (6, G)] | [S, A, C] | C | 3 | Add G(3+3=6) |
| 4 | [(5, C), (6, G)] | [S, A, C, B] | B | 4 | Add C via B(4+1=5), already visited |
| 5 | [(6, G)] | [S, A, C, B] | - | - | C already visited |
| 6 | **FOUND** | [S, A, C, B, G] | G | 6 | **Goal!** |

**Answer:**
- **Optimal path:** S → A → C → G
- **Optimal cost:** 1 + 2 + 3 = **6**
- **Alternative:** S → B → C → G = 4 + 1 + 3 = 8 (costlier)

### 💻 Python Implementation

```python
import heapq

def uniform_cost_search(graph, start, goal):
    """
    UCS implementation using priority queue
    
    Args:
        graph: dict {node: [(neighbor, cost), ...]}
    
    Returns:
        (path, cost) or (None, None)
    """
    # Priority queue: (cost, path)
    pq = [(0, [start])]
    visited = {}  # Store best cost to reach each node
    
    while pq:
        cost, path = heapq.heappop(pq)
        node = path[-1]
        
        # Goal test
        if node == goal:
            return path, cost
        
        # Skip if better path already found
        if node in visited and visited[node] <= cost:
            continue
        
        visited[node] = cost
        
        # Expand neighbors
        for neighbor, edge_cost in graph.get(node, []):
            new_cost = cost + edge_cost
            
            if neighbor not in visited or visited[neighbor] > new_cost:
                new_path = path + [neighbor]
                heapq.heappush(pq, (new_cost, new_path))
    
    return None, None

# Example
graph = {
    'S': [('A', 1), ('B', 4)],
    'A': [('C', 2)],
    'B': [('C', 1)],
    'C': [('G', 3)],
    'G': []
}

path, cost = uniform_cost_search(graph, 'S', 'G')
print(f"Path: {' → '.join(path)}")
print(f"Cost: {cost}")
```

### 📝 Key Points for GATE

1. **Like Dijkstra's algorithm** for single source shortest path
2. **Uses priority queue** (min-heap)
3. **Optimal even with different costs** (unlike BFS)
4. **Expands nodes in order of path cost**
5. **Can expand same node multiple times** (with different costs)

### 🎯 UCS vs BFS

| Aspect | BFS | UCS |
|--------|-----|-----|
| Costs | All equal | Can vary |
| Data Structure | Queue | Priority Queue |
| Expansion Order | By depth | By path cost |
| Optimal | Yes (if costs equal) | Yes (always) |

---

## Bidirectional Search

### 🎯 Concept

Run **two simultaneous searches**:
1. Forward from initial state
2. Backward from goal state

Stop when the two searches meet.

**Advantage:** Reduces time complexity from O(b^d) to O(b^(d/2))

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ✅ Yes | Both directions find solution |
| **Optimal** | ✅ Yes* | *If both use BFS |
| **Time** | O(b^(d/2)) | Square root of regular BFS! |
| **Space** | O(b^(d/2)) | Must store both frontiers |

### 🔢 Numerical Example

**Example:** For b=10, d=6

**Regular BFS:**
- Nodes = 10^6 = 1,000,000

**Bidirectional:**
- Nodes = 2 × 10^3 = 2,000
- **500× speedup!**

### 📝 Key Points for GATE

1. **Requires reversible operators**
2. **Space is bottleneck** (must store both frontiers)
3. **Best for:** State space with single goal
4. **Worst for:** Multiple goals or large branching factor

---

## Comparison Table

| Algorithm | Complete | Optimal | Time | Space | Notes |
|-----------|----------|---------|------|-------|-------|
| **BFS** | ✅ Yes | ✅ Yes* | O(b^d) | O(b^d) | *if costs equal |
| **DFS** | ❌ No | ❌ No | O(b^m) | O(bm) | Memory efficient |
| **DLS** | ❌ No | ❌ No | O(b^l) | O(bl) | With depth limit |
| **IDDFS** | ✅ Yes | ✅ Yes | O(b^d) | O(bd) | **Best uninformed** |
| **UCS** | ✅ Yes | ✅ Yes | O(b^(C*/ε)) | O(b^(C*/ε)) | For weighted graphs |
| **Bidirectional** | ✅ Yes | ✅ Yes | O(b^(d/2)) | O(b^(d/2)) | Fastest but memory-heavy |

---

## GATE Previous Year Questions

### Question 1 (GATE CS 2018)

Which of the following is/are true about BFS and DFS?

I. BFS is complete but DFS is not complete
II. Both BFS and DFS are optimal
III. Space complexity of DFS is better than BFS
IV. BFS is preferred over DFS for finding shortest path

**Options:**
- (A) I, III, IV
- (B) I, II, III
- (C) II, III, IV
- (D) I, III

**Answer: (A) I, III, IV**

**Explanation:**
- I is TRUE: BFS is complete, DFS can loop infinitely
- II is FALSE: BFS is optimal (for equal costs), DFS is not
- III is TRUE: DFS uses O(bm), BFS uses O(b^d)
- IV is TRUE: BFS finds shortest path in unweighted graphs

---

### Question 2 (GATE CS 2019)

In a graph with branching factor b=3 and solution at depth d=4, how many nodes will IDDFS generate?

**Solution:**

Nodes generated:
- Depth 0: 5 × 3^0 = 5
- Depth 1: 4 × 3^1 = 12
- Depth 2: 3 × 3^2 = 27
- Depth 3: 2 × 3^3 = 54
- Depth 4: 1 × 3^4 = 81

**Total = 5 + 12 + 27 + 54 + 81 = 179 nodes**

---

### Question 3 (GATE DA 2023)

For the graph shown, what is the order of node expansion in UCS from S to G?

```
S --2-- A --1-- G
 \           /
  3--B--4--C--2
```

**Solution:**

| Step | Queue (cost, node) | Expand |
|------|-------------------|--------|
| 1 | [(0, S)] | S |
| 2 | [(2, A), (3, B)] | A |
| 3 | [(3, B), (3, G)] | B or G (tie) |

**Order:** S → A → (B or G depending on tie-breaking)

**Path:** S → A → G with cost = 3

---

## Practice Problems

### Problem 1: Easy

Given tree:
```
     A
   / | \
  B  C  D
 /   |   \
E    F    G
```

Find:
1. BFS traversal order
2. DFS traversal order
3. If goal is F, which is better?

**Solution:**
1. BFS: A, B, C, D, E, F, G
2. DFS: A, B, E, C, F, D, G
3. DFS is better (fewer nodes: 3 vs 6)

---

### Problem 2: Medium

For graph with b=4, d=5:
1. How many nodes does BFS generate?
2. How many nodes does IDDFS generate?
3. Compare space requirements.

**Solution:**
1. BFS: 4^5 = 1024 nodes
2. IDDFS: 6×1 + 5×4 + 4×16 + 3×64 + 2×256 + 1×1024 = 1560 nodes
3. Space: BFS needs O(1024), IDDFS needs O(20) - **51× better!**

---

### Problem 3: Hard

Design a scenario where:
1. DFS outperforms BFS
2. BFS outperforms DFS
3. UCS outperforms both

**Solution:**

1. **DFS better:** Solution is deep, limited memory
   - Example: Deep directory search
   
2. **BFS better:** Solution is shallow, need optimal
   - Example: Social network (6 degrees of separation)
   
3. **UCS better:** Weighted graph, need optimal cost
   - Example: Road network with variable distances

---

## Summary & Tips for GATE

### 🎯 Must Remember

1. **BFS** - Queue, Optimal for equal costs, O(b^d) space
2. **DFS** - Stack, Not complete, O(bm) space
3. **IDDFS** - Best uninformed, combines BFS + DFS benefits
4. **UCS** - Priority queue, Optimal for any costs

### ✅ Quick Checklist

- [ ] Can code all algorithms from scratch
- [ ] Know time/space complexity of each
- [ ] Understand when to use which
- [ ] Can trace execution step-by-step
- [ ] Solved at least 20 numerical problems

### 🔥 Common Mistakes

1. Confusing completeness with optimality
2. Forgetting DFS can loop infinitely
3. Thinking IDDFS is wasteful (it's not!)
4. Using BFS for weighted graphs (use UCS!)

### 📚 Next Topics

After mastering this, move to:
- Informed Search (A*, Greedy)
- Adversarial Search (Minimax, Alpha-Beta)
- Constraint Satisfaction Problems

---

**Practice makes perfect! Solve at least 5-10 problems per algorithm.**

