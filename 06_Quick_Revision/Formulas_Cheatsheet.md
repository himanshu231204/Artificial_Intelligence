# AI Formulas Cheatsheet for GATE DA 📝

## Search Algorithms

### Evaluation Functions
- **BFS/DFS:** No evaluation
- **UCS:** f(n) = g(n)
- **Greedy:** f(n) = h(n)  
- **A*:** f(n) = g(n) + h(n)

### Complexities

| Algorithm | Time | Space | Complete | Optimal |
|-----------|------|-------|----------|---------|
| BFS | O(b^d) | O(b^d) | ✅ | ✅* |
| DFS | O(b^m) | O(bm) | ❌ | ❌ |
| IDDFS | O(b^d) | O(bd) | ✅ | ✅ |
| UCS | O(b^(C*/ε)) | O(b^(C*/ε)) | ✅ | ✅ |
| Greedy | O(b^m) | O(b^m) | ❌ | ❌ |
| A* | O(b^d) | O(b^d) | ✅ | ✅** |

*if costs equal, **if h admissible

### Heuristics

**Manhattan Distance:**
```
h(n) = |x₁ - x₂| + |y₁ - y₂|
```

**Euclidean Distance:**
```
h(n) = √[(x₁-x₂)² + (y₁-y₂)²]
```

**Admissible:** h(n) ≤ h*(n)  
**Consistent:** h(n) ≤ c(n,n') + h(n')

### Minimax & Alpha-Beta

**Minimax Value:**
- Terminal: utility(n)
- MAX: max(children)
- MIN: min(children)

**Alpha-Beta Bounds:**
- α: best for MAX
- β: best for MIN
- Prune when β ≤ α

---

## Probability

### Basic Rules

**Joint Probability:**
```
P(A ∩ B) = P(A|B) × P(B) = P(B|A) × P(A)
```

**Conditional Probability:**
```
P(A|B) = P(A ∩ B) / P(B)
```

**Bayes' Theorem:**
```
P(A|B) = P(B|A) × P(A) / P(B)
```

**Total Probability:**
```
P(B) = Σᵢ P(B|Aᵢ) × P(Aᵢ)
```

### Independence

**Independent:**
```
P(A ∩ B) = P(A) × P(B)
P(A|B) = P(A)
```

**Conditionally Independent:**
```
P(A ∩ B | C) = P(A|C) × P(B|C)
```

---

## Bayesian Networks

### Joint Probability
```
P(x₁,...,xₙ) = ∏ᵢ P(xᵢ | Parents(xᵢ))
```

### Variable Elimination

1. Join factors
2. Eliminate hidden variables
3. Normalize

**Complexity:** O(n × d^(w+1))  
where w = induced width

---

## Hidden Markov Models

### Forward Algorithm
```
α_t(j) = P(y₁:t, X_t=j)
α_t(j) = P(y_t|X_t=j) Σᵢ α_{t-1}(i) P(X_t=j|X_{t-1}=i)
```

### Viterbi Algorithm
```
δ_t(j) = max P(X₁:t₋₁, X_t=j, y₁:t)
δ_t(j) = P(y_t|X_t=j) maxᵢ[δ_{t-1}(i) P(X_t=j|X_{t-1}=i)]
```

---

## Quick Reference

### When to Use What

**Uninformed Search:**
- Unknown domain → IDDFS
- Memory available → BFS
- Deep solutions → DFS

**Informed Search:**
- Have heuristic → A*
- Need speed > optimality → Greedy
- Large state space → IDA*

**Probability:**
- Known structure → Bayesian Network
- Sequential data → HMM
- Need samples → Monte Carlo

