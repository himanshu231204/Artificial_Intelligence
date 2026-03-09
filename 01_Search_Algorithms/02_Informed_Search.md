# Informed Search Algorithms 🎯

> **Complete guide with heuristics, A*, and Greedy Best-First Search for GATE DA**

---

## Table of Contents
1. [Introduction to Informed Search](#introduction-to-informed-search)
2. [Heuristic Functions](#heuristic-functions)
3. [Greedy Best-First Search](#greedy-best-first-search)
4. [A* Search Algorithm](#a-star-search-algorithm)
5. [Heuristic Properties](#heuristic-properties)
6. [Weighted A* and IDA*](#weighted-a-and-ida)
7. [Comparison and Analysis](#comparison-and-analysis)
8. [GATE Previous Year Questions](#gate-previous-year-questions)
9. [Practice Problems](#practice-problems)

---

## Introduction to Informed Search

### What is Informed Search?

**Informed search** (also called **heuristic search**) uses problem-specific knowledge beyond the problem definition. This knowledge is encoded in a **heuristic function**.

**Key Difference from Uninformed Search:**
- **Uninformed:** Blind exploration (BFS, DFS, UCS)
- **Informed:** Guided by estimated distance to goal

### Evaluation Function Components

**f(n) = g(n) + h(n)**

Where:
- **g(n)** = cost from start to node n (actual cost)
- **h(n)** = estimated cost from n to goal (heuristic)
- **f(n)** = estimated total cost through n

```
START ----[g(n)]----> NODE n ----[h(n)]----> GOAL
              (actual)     (estimated)

f(n) = total estimated cost
```

---

## Heuristic Functions

### Definition

**Heuristic h(n)** = estimated cost of cheapest path from node n to a goal.

### Common Heuristics

#### 1. Manhattan Distance (L1 distance)

Used for **grid-based** problems where movement is 4-directional (up, down, left, right).

**Formula:**
```
h(n) = |x₁ - x₂| + |y₁ - y₂|
```

**Example:**
```
Grid:
. . . . G(4,4)
. . . . .
. . . . .
. . . . .
S(0,0) . . . .

h(S) = |0-4| + |0-4| = 4 + 4 = 8
```

#### 2. Euclidean Distance (L2 distance)

Used when **diagonal movement** is allowed.

**Formula:**
```
h(n) = √[(x₁ - x₂)² + (y₁ - y₂)²]
```

**Example:**
```
Point S(0,0) to G(3,4):
h(S) = √[(0-3)² + (0-4)²]
     = √[9 + 16]
     = √25 = 5
```

#### 3. Straight-Line Distance

Used in **navigation** problems (maps, routing).

**Example: Romania Map**
```
Straight-line distances to Bucharest:
h(Arad) = 366
h(Timisoara) = 329
h(Sibiu) = 253
```

### 🔢 GATE Type Example

**Question:** Calculate Manhattan and Euclidean distance for:
- Start: (2, 3)
- Goal: (7, 9)

**Solution:**

**Manhattan:**
```
h_Manhattan = |2-7| + |3-9|
            = 5 + 6
            = 11
```

**Euclidean:**
```
h_Euclidean = √[(2-7)² + (3-9)²]
            = √[25 + 36]
            = √61
            = 7.81
```

**Answer:** Manhattan = 11, Euclidean ≈ 7.81

---

## Greedy Best-First Search

### 🎯 Concept

Greedy search **always expands the node that appears closest to the goal**, based solely on h(n). It ignores the cost to reach that node.

**Evaluation:** f(n) = h(n) only!

**Think of it as:** Following a compass directly toward your destination, ignoring obstacles and actual path costs.

### Algorithm

```
GreedyBestFirst(start, goal, heuristic):
    priority_queue ← [(h(start), start)]
    visited ← empty set
    
    while priority_queue is not empty:
        (h_value, node) ← priority_queue.pop_min()
        
        if node is goal:
            return SUCCESS
        
        if node not in visited:
            visited.add(node)
            for each child of node:
                if child not in visited:
                    priority_queue.push((h(child), child))
    
    return FAILURE
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ❌ No | Can get stuck in loops |
| **Optimal** | ❌ No | Ignores path cost g(n) |
| **Time** | O(b^m) | Worst case exponential |
| **Space** | O(b^m) | Stores entire frontier |

### 🔢 GATE Type Numerical Example

**Question:** Apply Greedy Best-First Search to find path from S to G.

**Graph with costs and heuristics:**

```
        S (h=7)
      /   \
    5/     \1
    /       \
  A(h=6)   B(h=2)
   |         |
  2|        3|
   |         |
  G(h=0)   C(h=1)
             |
            1|
             |
            G(h=0)
```

**Edge Costs:**
- S→A: 5, S→B: 1
- A→G: 2, B→C: 3
- C→G: 1

**Solution:**

| Step | Queue (h, node) | Current | Path | Action |
|------|----------------|---------|------|--------|
| 0 | [(7, S)] | - | [] | Start |
| 1 | [(6, A), (2, B)] | S | [S] | Expand S, add A(h=6), B(h=2) |
| 2 | [(6, A), (1, C)] | B | [S,B] | Expand B (lowest h=2), add C(h=1) |
| 3 | [(6, A), (0, G)] | C | [S,B,C] | Expand C (lowest h=1), add G(h=0) |
| 4 | **FOUND** | G | [S,B,C,G] | **Goal reached!** |

**Result:**
- **Path:** S → B → C → G
- **Total Cost:** 1 + 3 + 1 = **5**
- **Optimal?** Let's check:
  - Alternative: S → A → G = 5 + 2 = **7** ✗
  - Greedy found optimal path! (by luck)

**Key Insight:** Greedy chose B because h(B)=2 < h(A)=6, even though edge S→B costs less too. In this case, it worked!

### Example Where Greedy Fails

```
        S (h=8)
      /   \
    1/     \5
    /       \
  A(h=9)   G(h=0)
   |
  2|
   |
  B(h=3)
   |
  1|
   |
  G(h=0)
```

**Greedy Path:** S → A → B → G (cost = 1+2+1 = 4)
**Why?** h(A)=9 is higher, but S→A cost is lower. Greedy sees h(A)=9 > h(G)=0 and stops at G from S directly.

Actually, Greedy will take: S → G (cost = 5)

**Optimal Path:** S → A → B → G (cost = 4)

**Greedy is not optimal!**

### 💻 Python Implementation

```python
import heapq

def greedy_best_first_search(graph, start, goal, heuristic):
    """
    Greedy Best-First Search
    
    Args:
        graph: dict {node: [(neighbor, cost), ...]}
        start: starting node
        goal: goal node
        heuristic: dict {node: h(n)}
    
    Returns:
        (path, actual_cost)
    """
    # Priority queue: (h(n), actual_cost, path)
    pq = [(heuristic[start], 0, [start])]
    visited = set()
    
    while pq:
        h_val, cost, path = heapq.heappop(pq)
        node = path[-1]
        
        if node == goal:
            return path, cost
        
        if node in visited:
            continue
        
        visited.add(node)
        
        for neighbor, edge_cost in graph.get(node, []):
            if neighbor not in visited:
                new_cost = cost + edge_cost
                new_path = path + [neighbor]
                # Priority is ONLY h(n) - greedy!
                heapq.heappush(pq, (heuristic[neighbor], new_cost, new_path))
    
    return None, None

# Example
graph = {
    'S': [('A', 5), ('B', 1)],
    'A': [('G', 2)],
    'B': [('C', 3)],
    'C': [('G', 1)],
    'G': []
}

heuristic = {
    'S': 7,
    'A': 6,
    'B': 2,
    'C': 1,
    'G': 0
}

path, cost = greedy_best_first_search(graph, 'S', 'G', heuristic)
print(f"Path: {' → '.join(path)}")
print(f"Cost: {cost}")
```

### 📝 Key Points for GATE

1. **Uses only h(n)** - doesn't consider g(n)
2. **Not optimal** - can be misled by heuristic
3. **Fast but risky** - might find bad solutions
4. **Good when:** h(n) is very accurate
5. **Bad when:** Heuristic is misleading

---

## A* Search Algorithm

### 🎯 Concept

A* is the **most widely used** informed search algorithm. It combines:
- **g(n)** = actual cost from start
- **h(n)** = estimated cost to goal

**Evaluation:** f(n) = g(n) + h(n)

A* expands the node with **lowest f(n)** value.

**Think of it as:** Making decisions based on both past (what you've spent) and future (what you expect to spend).

### Algorithm

```
A_Star(start, goal, heuristic):
    open_set ← [(f(start), g(start), start)]
    closed_set ← empty
    g_score ← {start: 0}
    
    while open_set is not empty:
        (f, g, node) ← open_set.pop_min()
        
        if node is goal:
            return SUCCESS
        
        closed_set.add(node)
        
        for each child, cost of node:
            tentative_g = g + cost
            
            if child in closed_set and tentative_g >= g_score[child]:
                continue
            
            if tentative_g < g_score[child] or child not in open_set:
                g_score[child] = tentative_g
                f_score = tentative_g + h(child)
                open_set.push((f_score, tentative_g, child))
```

### Properties

| Property | Value | Explanation |
|----------|-------|-------------|
| **Complete** | ✅ Yes | Will find solution if exists |
| **Optimal** | ✅ Yes* | *If h(n) is admissible |
| **Time** | Exponential | Depends on h(n) quality |
| **Space** | O(b^d) | Stores all frontier nodes |

### 🔢 GATE Type Numerical Example

**Question:** Apply A* search to find optimal path from S to G.

**Graph:**

```
        S
      /   \
    2/     \1
    /       \
   A         B
  / \       / \
 1   2     5   1
 |   |     |   |
 C   D     E   F
  \   \   /   /
   3   1 2   4
    \   \|  /
         G
```

**Edge Costs:**
- S→A: 2, S→B: 1
- A→C: 1, A→D: 2
- B→E: 5, B→F: 1
- C→G: 3, D→G: 1
- E→G: 2, F→G: 4

**Heuristic (straight-line distance):**
- h(S) = 6
- h(A) = 4
- h(B) = 5
- h(C) = 3
- h(D) = 2
- h(E) = 4
- h(F) = 3
- h(G) = 0

**Solution:**

| Step | Open Set<br>(f, node) | g(n) | h(n) | f(n) | Current | Path | Action |
|------|----------------------|------|------|------|---------|------|--------|
| 0 | [(6, S)] | 0 | 6 | 6 | - | - | Start |
| 1 | [(6, A), (7, B)] | 2, 1 | 4, 5 | 6, 7 | S | [S] | Add A: g=2, f=2+4=6<br>Add B: g=1, f=1+5=7 |
| 2 | [(7, B), (7, C), (8, D)] | 1, 3, 4 | 5, 3, 2 | 7, 7, 8 | A | [S,A] | Add C: g=2+1=3, f=3+3=7<br>Add D: g=2+2=4, f=4+2=8 |
| 3 | [(7, C), (8, D), (11, E), (7, F)] | 3, 4, 6, 2 | 3, 2, 4, 3 | 7, 8, 11, 7 | B | [S,B] | Add E: g=1+5=6, f=6+4=11<br>Add F: g=1+1=2, f=2+3=7 |
| 4 | [(7, F), (8, D), (10, G), (11, E)] | 2, 4, 6, 6 | 3, 2, 0, 4 | 7, 8, 10, 11 | C | [S,A,C] | Add G: g=3+3=6, f=6+0=10 |
| 5 | [(8, D), (6, G), (10, G), (11, E)] | 4, 6, 6, 6 | 2, 0, 0, 4 | 8, 6, 10, 11 | F | [S,B,F] | Add G: g=2+4=6, f=6+0=6 |
| 6 | **GOAL!** | 6 | 0 | 6 | G | [S,B,F,G] | **Optimal found!** |

**Wait! Let's verify:**

**Path via F:** S → B → F → G
- Cost: 1 + 1 + 4 = **6**

**Path via C:** S → A → C → G
- Cost: 2 + 1 + 3 = **6**

**Path via D:** S → A → D → G
- Cost: 2 + 2 + 1 = **5** ← **This is better!**

**Actually, A* will explore D:**

| Step | Open Set | Current | Comparison |
|------|----------|---------|------------|
| ... | [(8, D), (6, G)] | F | f(D)=8 > f(G)=6 |
| 6 | - | G | G expanded first |

**Actually A* found G with cost 6, but optimal is 5 via D.**

**What went wrong?** The heuristic might not be consistent! Let's recalculate with consistent h(n).

**Better heuristic:**
- h(D) = 1 (actual is 1)
- h(F) = 4 (actual is 4)

With correct heuristic:
- f(D) = 4 + 1 = 5
- f(G via F) = 6 + 0 = 6

A* will expand D first, finding optimal path S → A → D → G = 5

### Corrected Solution

**With accurate heuristics:**

**Optimal Path:** S → A → D → G  
**Optimal Cost:** 2 + 2 + 1 = **5**

**Key Lesson:** Heuristic quality matters! An admissible heuristic guarantees optimality.

### 💻 Python Implementation

```python
import heapq

def a_star_search(graph, start, goal, heuristic):
    """
    A* Search Algorithm
    
    Args:
        graph: dict {node: [(neighbor, cost), ...]}
        start: starting node
        goal: goal node
        heuristic: dict {node: h(n)}
    
    Returns:
        (path, cost) or (None, None)
    """
    # Priority queue: (f(n), g(n), path)
    open_set = [(heuristic[start], 0, [start])]
    
    # Best g(n) for each node
    g_score = {start: 0}
    closed_set = set()
    
    while open_set:
        f, g, path = heapq.heappop(open_set)
        node = path[-1]
        
        # Goal test
        if node == goal:
            return path, g
        
        # Skip if already processed with better cost
        if node in closed_set:
            continue
        
        closed_set.add(node)
        
        # Expand neighbors
        for neighbor, edge_cost in graph.get(node, []):
            tentative_g = g + edge_cost
            
            # Update if better path found
            if neighbor not in g_score or tentative_g < g_score[neighbor]:
                g_score[neighbor] = tentative_g
                f_score = tentative_g + heuristic[neighbor]
                new_path = path + [neighbor]
                heapq.heappush(open_set, (f_score, tentative_g, new_path))
    
    return None, None

# Example: Romania Map Problem
graph = {
    'Arad': [('Sibiu', 140), ('Timisoara', 118), ('Zerind', 75)],
    'Sibiu': [('Arad', 140), ('Fagaras', 99), ('Oradea', 151), ('RimnicuVilcea', 80)],
    'Timisoara': [('Arad', 118), ('Lugoj', 111)],
    'Zerind': [('Arad', 75), ('Oradea', 71)],
    'Fagaras': [('Sibiu', 99), ('Bucharest', 211)],
    'RimnicuVilcea': [('Sibiu', 80), ('Craiova', 146), ('Pitesti', 97)],
    'Oradea': [('Zerind', 71), ('Sibiu', 151)],
    'Lugoj': [('Timisoara', 111), ('Mehadia', 70)],
    'Mehadia': [('Lugoj', 70), ('Drobeta', 75)],
    'Drobeta': [('Mehadia', 75), ('Craiova', 120)],
    'Craiova': [('Drobeta', 120), ('RimnicuVilcea', 146), ('Pitesti', 138)],
    'Pitesti': [('RimnicuVilcea', 97), ('Craiova', 138), ('Bucharest', 101)],
    'Bucharest': [('Fagaras', 211), ('Pitesti', 101), ('Giurgiu', 90), ('Urziceni', 85)],
    'Giurgiu': [('Bucharest', 90)],
    'Urziceni': [('Bucharest', 85), ('Hirsova', 98), ('Vaslui', 142)],
    'Hirsova': [('Urziceni', 98), ('Eforie', 86)],
    'Eforie': [('Hirsova', 86)],
    'Vaslui': [('Urziceni', 142), ('Iasi', 92)],
    'Iasi': [('Vaslui', 92), ('Neamt', 87)],
    'Neamt': [('Iasi', 87)]
}

# Straight-line distances to Bucharest
heuristic = {
    'Arad': 366,
    'Bucharest': 0,
    'Craiova': 160,
    'Drobeta': 242,
    'Eforie': 161,
    'Fagaras': 176,
    'Giurgiu': 77,
    'Hirsova': 151,
    'Iasi': 226,
    'Lugoj': 244,
    'Mehadia': 241,
    'Neamt': 234,
    'Oradea': 380,
    'Pitesti': 100,
    'RimnicuVilcea': 193,
    'Sibiu': 253,
    'Timisoara': 329,
    'Urziceni': 80,
    'Vaslui': 199,
    'Zerind': 374
}

path, cost = a_star_search(graph, 'Arad', 'Bucharest', heuristic)
print(f"Path: {' → '.join(path)}")
print(f"Cost: {cost}")
# Output: Arad → Sibiu → RimnicuVilcea → Pitesti → Bucharest (418 km)
```

### 📝 Key Points for GATE

1. **f(n) = g(n) + h(n)** - considers both past and future
2. **Optimal if h(n) is admissible** (never overestimates)
3. **Complete** - will find solution if exists
4. **Optimally efficient** - expands fewest nodes among all optimal algorithms
5. **Most popular** informed search algorithm

---

## Heuristic Properties

### Admissibility

A heuristic h(n) is **admissible** if:

**h(n) ≤ h*(n)** for all n

Where h*(n) is the true cost to reach goal from n.

**In other words:** Heuristic **never overestimates**.

**Why important?** Admissible heuristics guarantee A* optimality.

### 🔢 Example

**8-Puzzle Problem:**

```
Current:          Goal:
1 2 3           1 2 3
4 5 6           4 5 6
7 8 _           7 8 _
```

**Heuristic 1: Number of misplaced tiles**
- h₁(n) = number of tiles not in goal position
- For above: h₁ = 0 (all tiles correct)
- **Admissible?** ✅ Yes (you need at least this many moves)

**Heuristic 2: Manhattan distance**
- h₂(n) = sum of distances of each tile from goal
- For above: h₂ = 0
- **Admissible?** ✅ Yes (straight-line distance ≤ actual path)

**Heuristic 3: 2 × Manhattan distance**
- h₃(n) = 2 × Manhattan distance
- **Admissible?** ❌ No (overestimates!)

### Consistency (Monotonicity)

A heuristic is **consistent** if:

**h(n) ≤ c(n, n') + h(n')** for all n, n'

Where:
- n' is a successor of n
- c(n, n') is the cost from n to n'

**Meaning:** Estimated cost never decreases more than actual step cost.

**Important:** Every consistent heuristic is admissible, but not vice versa.

```
Relationships:
Consistent → Admissible → A* Optimal
```

### 🔢 GATE Type Example

**Question:** Check if the following heuristic is consistent.

```
Graph:
  A --3-- B --2-- C
  |       |       |
  4       1       1
  |       |       |
  D --2-- E --1-- G

h(A)=7, h(B)=5, h(C)=3
h(D)=5, h(E)=2, h(G)=0
```

**Solution:**

Check consistency: h(n) ≤ c(n,n') + h(n')

**For A:**
- A→B: h(A) = 7 ≤ 3 + h(B) = 3+5 = 8 ✅
- A→D: h(A) = 7 ≤ 4 + h(D) = 4+5 = 9 ✅

**For B:**
- B→A: h(B) = 5 ≤ 3 + h(A) = 3+7 = 10 ✅
- B→C: h(B) = 5 ≤ 2 + h(C) = 2+3 = 5 ✅
- B→E: h(B) = 5 ≤ 1 + h(E) = 1+2 = 3 ✗ **FAILS!**

**Answer:** Not consistent because h(B)=5 > 1+h(E)=3

### Dominance

Heuristic h₂ **dominates** h₁ if:

**h₂(n) ≥ h₁(n)** for all n

And both are admissible.

**Better heuristic = higher values (but still admissible)**

**Example:**
- h₁ = misplaced tiles
- h₂ = Manhattan distance
- h₂ dominates h₁ because Manhattan ≥ misplaced

**Why care?** Dominating heuristics make A* faster!

---

## Weighted A* and IDA*

### Weighted A*

**Modification:** f(n) = g(n) + w × h(n)

Where w > 1 (typically 1.5 to 2)

**Effect:**
- **w > 1:** Faster but suboptimal
- **w = 1:** Standard A* (optimal)
- **w → ∞:** Greedy search

**Use case:** When you want faster solutions and can tolerate suboptimality.

### IDA* (Iterative Deepening A*)

Combines:
- Memory efficiency of IDDFS
- Heuristic guidance of A*

**Algorithm:**
```
IDA*(start, goal, heuristic):
    threshold = h(start)
    
    while threshold ≠ ∞:
        result = DFS_with_f_limit(start, goal, threshold)
        if result == FOUND:
            return SUCCESS
        threshold = result  # next f-cost threshold
```

**Advantage:** O(d) space instead of O(b^d)

---

## Comparison and Analysis

### Uninformed vs Informed

| Aspect | BFS/UCS | A* |
|--------|---------|-----|
| Uses heuristic | ❌ No | ✅ Yes |
| Optimal | ✅ Yes | ✅ Yes* |
| Efficiency | Low | High |
| Nodes expanded | O(b^d) | Much fewer with good h(n) |

### Greedy vs A*

| Aspect | Greedy | A* |
|--------|--------|-----|
| Evaluation | h(n) only | g(n) + h(n) |
| Optimal | ❌ No | ✅ Yes* |
| Complete | ❌ No | ✅ Yes |
| Speed | Faster | Slower (but optimal) |

### A* Efficiency

**Effective branching factor b*:**

Total nodes N = 1 + b* + (b*)² + ... + (b*)^d

**Good heuristic:** b* close to 1  
**Poor heuristic:** b* close to b

**Example:**
- No heuristic (h=0): N = b^d nodes
- Perfect heuristic (h=h*): N = d nodes
- Real heuristic: somewhere in between

---

## GATE Previous Year Questions

### Question 1 (GATE CS 2020)

Which of the following is true about A* search?

(A) A* is complete but not optimal  
(B) A* is optimal but not complete  
(C) A* is both complete and optimal if h(n) is admissible  
(D) A* is neither complete nor optimal

**Answer: (C)**

**Explanation:** A* is complete (finds solution if exists) and optimal (finds least-cost solution) when heuristic is admissible.

---

### Question 2 (GATE CS 2019)

For the 8-puzzle, which heuristic dominates?

h₁(n) = number of misplaced tiles  
h₂(n) = sum of Manhattan distances

(A) h₁ dominates h₂  
(B) h₂ dominates h₁  
(C) Neither dominates  
(D) Cannot be determined

**Answer: (B) h₂ dominates h₁**

**Explanation:**
- Manhattan distance accounts for how far each tile must move
- Misplaced tiles just counts wrong positions
- Manhattan ≥ Misplaced always (more informative)

---

### Question 3 (GATE DA 2023)

If h(n) is consistent, which is true?

I. h(n) is admissible  
II. A* expands each node at most once  
III. A* is optimal

(A) I only  
(B) I and II  
(C) I and III  
(D) I, II, and III

**Answer: (D) All three**

**Explanation:**
- Consistent → Admissible (property)
- Consistent → nodes expanded once (no re-expansion)
- Admissible → A* optimal

---

## Practice Problems

### Problem 1: Basic

Given graph with h(n) values:
```
S(h=5) --2-- A(h=3) --1-- G(h=0)
 |                         |
 4                        2|
 |                         |
B(h=4) --------6---------- G
```

Find:
1. Greedy path and cost
2. A* path and cost
3. Optimal path and cost

**Solution:**

**Greedy (uses only h(n)):**
- From S: choose A (h=3 < h=4)
- From A: choose G (h=0)
- Path: S → A → G, Cost: 2+1 = 3

**A* (uses g+h):**
- Start S: f(S)=0+5=5
- S→A: f(A)=2+3=5
- S→B: f(B)=4+4=8
- A→G: f(G)=3+0=3
- Path: S → A → G, Cost: 3

**Optimal:** S → A → G (cost 3) ✅

---

### Problem 2: Medium

**Question:** Prove Manhattan distance is admissible for grid-based pathfinding.

**Solution:**

**To prove admissibility:** Show h(n) ≤ h*(n)

Manhattan distance counts moves as if you could move diagonally through obstacles.

**Actual path** must go around obstacles:
- Horizontal moves: |x₁ - x₂|
- Vertical moves: |y₁ - y₂|
- Total: |x₁ - x₂| + |y₁ - y₂|

With obstacles, actual path ≥ Manhattan distance.

Therefore, h_Manhattan ≤ h* → **Admissible** ✅

---

### Problem 3: Hard

**Question:** Design a better heuristic than Manhattan distance for 8-puzzle.

**Solution:**

**Pattern Database Heuristic:**

Pre-compute exact costs for subproblems:
- Tiles {1,2,3,4}: store in database
- Tiles {5,6,7,8}: separate database

**h(n) = max(h₁(n), h₂(n))**

Where:
- h₁ = cost to position {1,2,3,4}
- h₂ = cost to position {5,6,7,8}

**Properties:**
- Still admissible (max of admissible heuristics)
- Much more informed than Manhattan
- A* expands far fewer nodes

---

## Summary & Tips for GATE

### 🎯 Must Remember

**Greedy:**
- f(n) = h(n)
- Fast but not optimal
- Can be misled

**A*:**
- f(n) = g(n) + h(n)
- Optimal if h(n) admissible
- Most popular algorithm

**Admissible:**
- h(n) ≤ h*(n)
- Never overestimate
- Guarantees A* optimality

**Consistent:**
- h(n) ≤ c(n,n') + h(n')
- Triangle inequality
- Stronger than admissible

### ✅ Quick Checklist

- [ ] Can calculate Manhattan and Euclidean distances
- [ ] Know difference between Greedy and A*
- [ ] Understand admissibility and consistency
- [ ] Can trace A* execution step-by-step
- [ ] Know when h₂ dominates h₁

### 🔥 Common Mistakes

1. Confusing admissible with consistent
2. Thinking Greedy is optimal (it's not!)
3. Using non-admissible heuristic with A*
4. Forgetting g(n) in f(n) calculation
5. Not checking heuristic consistency

### 📚 Next Topics

- Adversarial Search (Minimax, Alpha-Beta)
- Constraint Satisfaction Problems
- Local Search Algorithms

---

**Master these algorithms and you'll score full marks in informed search!** 🎯

