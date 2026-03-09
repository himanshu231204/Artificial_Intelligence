# Adversarial Search (Game Playing) 🎮

> **Minimax, Alpha-Beta Pruning, and Game Trees for GATE DA**

## Table of Contents
1. [Introduction to Game Playing](#introduction)
2. [Game Trees](#game-trees)
3. [Minimax Algorithm](#minimax-algorithm)
4. [Alpha-Beta Pruning](#alpha-beta-pruning)
5. [Evaluation Functions](#evaluation-functions)
6. [GATE Questions](#gate-questions)

---

## Introduction

**Adversarial search** deals with situations where agents have **conflicting goals** (e.g., games like chess, tic-tac-toe).

### Key Differences from Regular Search

| Regular Search | Adversarial Search |
|----------------|-------------------|
| Single agent | Multiple agents |
| Deterministic outcome | Opponent's move unpredictable |
| Find optimal path | Find optimal move |

### Game Types

**Perfect Information Games:**
- Both players see complete state
- Examples: Chess, Checkers, Tic-Tac-Toe

**Imperfect Information:**
- Hidden information
- Examples: Poker, Scrabble

We focus on **perfect information, deterministic, turn-taking, two-player, zero-sum games**.

---

## Game Trees

### Structure

```
          MAX (You)
         /    \
       /        \
     MIN       MIN (Opponent)
    / \       / \
  MAX MAX   MAX MAX (You again)
```

**Ply:** One move by one player  
**Depth:** Number of plies from root

### Tic-Tac-Toe Example

```
Initial State (MAX to move):
X | O |  
---------
  |   |  
---------
  |   |  

Possible moves: 7 positions
Each leads to MIN's turn (O to move)
```

---

## Minimax Algorithm

### Concept

**MAX** tries to maximize score  
**MIN** tries to minimize score

**Minimax value** of node n:
```
If n is terminal: utility(n)
If n is MAX node: max of children
If n is MIN node: min of children
```

### Algorithm

```python
def minimax(node, depth, isMaximizing):
    if depth == 0 or isTerminal(node):
        return evaluate(node)
    
    if isMaximizing:
        maxEval = -∞
        for child in getChildren(node):
            eval = minimax(child, depth-1, False)
            maxEval = max(maxEval, eval)
        return maxEval
    else:
        minEval = +∞
        for child in getChildren(node):
            eval = minimax(child, depth-1, True)
            minEval = min(minEval, eval)
        return minEval
```

### 🔢 Example

```
Game Tree (MAX starts):

              A (MAX)
           /     \
         /         \
       B(MIN)      C(MIN)
      / \         /  \
    D   E       F    G
    3   5       2    9

Evaluation:
1. B = min(3, 5) = 3
2. C = min(2, 9) = 2
3. A = max(3, 2) = 3

Best move: LEFT (to node B)
```

### Properties

- **Complete:** Yes (if tree is finite)
- **Optimal:** Yes (against optimal opponent)
- **Time:** O(b^m) where m = max depth
- **Space:** O(bm) for DFS implementation

---

## Alpha-Beta Pruning

### Concept

**Optimization of Minimax** that prunes branches that cannot affect final decision.

**α (alpha):** Best value for MAX so far  
**β (beta):** Best value for MIN so far

**Pruning conditions:**
- At MIN node: if β ≤ α, prune remaining siblings
- At MAX node: if α ≥ β, prune remaining siblings

### Algorithm

```python
def alphabeta(node, depth, α, β, isMaximizing):
    if depth == 0 or isTerminal(node):
        return evaluate(node)
    
    if isMaximizing:
        maxEval = -∞
        for child in getChildren(node):
            eval = alphabeta(child, depth-1, α, β, False)
            maxEval = max(maxEval, eval)
            α = max(α, eval)
            if β ≤ α:  # β cutoff
                break
        return maxEval
    else:
        minEval = +∞
        for child in getChildren(node):
            eval = alphabeta(child, depth-1, α, β, True)
            minEval = min(minEval, eval)
            β = min(β, eval)
            if β ≤ α:  # α cutoff
                break
        return minEval
```

### 🔢 Example with Pruning

```
Tree (left-to-right evaluation):

              A (MAX) α=-∞, β=+∞
           /     \
         /         \
       B(MIN)      C(MIN)
      / \         /  \
    D   E       F    G
    3   5       2    9

Step-by-step:
1. Explore D: value = 3
   At B: β = min(+∞, 3) = 3
2. Explore E: value = 5
   At B: β = min(3, 5) = 3
   B returns 3
3. At A: α = max(-∞, 3) = 3
4. Explore F: value = 2
   At C: β = min(+∞, 2) = 2
5. Check: β(2) ≤ α(3)? YES!
   **PRUNE G** - no need to evaluate!

C returns 2
A = max(3, 2) = 3

Nodes evaluated: 5 (instead of 6)
Pruned: G
```

### Effectiveness

**Best case (perfect ordering):** O(b^(m/2))  
**Worst case (poor ordering):** O(b^m)

**Move ordering matters!** Evaluate likely best moves first.

---

## Evaluation Functions

For games too large to search completely:

**Heuristic evaluation** estimates value of non-terminal position.

### Chess Example

```
eval(state) = 
  9×(#queens_white - #queens_black) +
  5×(#rooks_white - #rooks_black) +
  3×(#bishops_white - #bishops_black) +
  3×(#knights_white - #knights_black) +
  1×(#pawns_white - #pawns_black)
```

### Properties of Good Eval Functions

1. **Fast to compute**
2. **Correlates with winning chances**
3. **Considers multiple factors** (material, position, mobility)

---

## GATE Questions

### Q1: Minimax Value

```
Tree:
        A(MAX)
       / \
     B    C (MIN)
    /\    /\
   3  5  6  2

Find minimax value of A.
```

**Solution:**
- B = max(3,5) = 5
- C = min(6,2) = 2
- A = max(5,2) = 5

**Answer: 5**

### Q2: Alpha-Beta Pruning

```
How many nodes pruned?

        A(MAX)
      / | \
    B  C  D (MIN)
   /\ /\ /\
  3 5 6 2 7 4

Evaluate left-to-right.
```

**Solution:**
- B=min(3,5)=3, α=3
- C=min(6,2)=2, α still 3
- At D: evaluate 7
  - β=min(∞,7)=7
  - β(7) > α(3), continue
  - Evaluate 4
  - β=min(7,4)=4
- No pruning in this case

**Answer: 0 nodes pruned**

---

## Summary

### Key Points

1. **Minimax:** Optimal strategy assuming optimal opponent
2. **Alpha-Beta:** Speeds up minimax via pruning
3. **Evaluation:** Needed for deep game trees
4. **Move Ordering:** Critical for pruning effectiveness

### Complexities

| Algorithm | Time | Space |
|-----------|------|-------|
| Minimax | O(b^m) | O(bm) |
| Alpha-Beta (worst) | O(b^m) | O(bm) |
| Alpha-Beta (best) | O(b^(m/2)) | O(bm) |

**Best ordering doubles searchable depth!**

