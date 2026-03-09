# Probability Basics for AI 🎲

## Fundamental Concepts

### Sample Space (Ω)
Set of all possible outcomes

**Example:** Coin flip
```
Ω = {H, T}
```

### Event (E)
Subset of sample space

**Example:** Rolling even number on die
```
E = {2, 4, 6}
```

### Probability Function P

**Axioms:**
1. 0 ≤ P(E) ≤ 1
2. P(Ω) = 1
3. P(E₁ ∪ E₂) = P(E₁) + P(E₂) if disjoint

## Basic Rules

### Addition Rule
```
P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
```

**Example:**
- P(rolling 6) = 1/6
- P(rolling even) = 3/6
- P(6 ∩ even) = 1/6
- P(6 ∪ even) = 1/6 + 3/6 - 1/6 = 3/6

### Complement Rule
```
P(¬A) = 1 - P(A)
```

### Conditional Probability
```
P(A|B) = P(A ∩ B) / P(B)
```

**Interpretation:** Probability of A given B occurred

**Example:** Two dice
- P(sum=7 | first=3) = ?
- If first=3, second can be 1,2,3,4,5,6
- Sum=7 when second=4
- P(sum=7|first=3) = 1/6

## Independence

**Definition:** A, B independent if
```
P(A ∩ B) = P(A) × P(B)

Equivalently:
P(A|B) = P(A)
```

**Example:** Two coin flips are independent
```
P(H₁ ∩ H₂) = 1/4 = 1/2 × 1/2 = P(H₁) × P(H₂)
```

**Counter-example:** Drawing cards without replacement
```
P(Ace₂|Ace₁) = 3/51 ≠ 4/52 = P(Ace₂)
Not independent!
```

## Bayes' Theorem

### Formula
```
P(A|B) = P(B|A) × P(A) / P(B)

Or with total probability:
P(A|B) = P(B|A) × P(A) / [P(B|A)P(A) + P(B|¬A)P(¬A)]
```

### 🔢 GATE Example: Medical Test

**Given:**
- Disease prevalence: P(D) = 0.01
- Test sensitivity: P(+|D) = 0.95
- False positive rate: P(+|¬D) = 0.05

**Find:** P(D|+) = ?

**Solution:**
```
P(D|+) = P(+|D) × P(D) / P(+)

P(+) = P(+|D)P(D) + P(+|¬D)P(¬D)
     = 0.95(0.01) + 0.05(0.99)
     = 0.0095 + 0.0495
     = 0.059

P(D|+) = 0.95 × 0.01 / 0.059
       = 0.0095 / 0.059
       ≈ 0.161 = 16.1%
```

**Interpretation:** Even with positive test, only 16% chance of disease!

## Random Variables

### Discrete Random Variable

**Probability Mass Function (PMF):**
```
P(X = x)
```

**Example:** Die roll
```
P(X=1) = P(X=2) = ... = P(X=6) = 1/6
```

### Expected Value
```
E[X] = Σ x × P(X=x)
```

**Example:** Die roll
```
E[X] = 1(1/6) + 2(1/6) + ... + 6(1/6)
     = (1+2+3+4+5+6)/6
     = 21/6 = 3.5
```

### Variance
```
Var(X) = E[(X - E[X])²]
       = E[X²] - (E[X])²
```

## Joint Probability

### Definition
```
P(X=x, Y=y) = probability both occur
```

### Marginalization
```
P(X=x) = Σy P(X=x, Y=y)
```

**Example:**
```
       Y=0  Y=1  P(X)
X=0    0.2  0.3  0.5
X=1    0.1  0.4  0.5
P(Y)   0.3  0.7  1.0

P(X=0) = 0.2 + 0.3 = 0.5
P(Y=1) = 0.3 + 0.4 = 0.7
```

## Conditional Independence

**Definition:** X ⊥ Y | Z if
```
P(X,Y|Z) = P(X|Z) × P(Y|Z)
```

**Meaning:** X, Y independent given Z

**Example:** Alarm System
- Burglary (B) causes Alarm (A)
- Earthquake (E) causes Alarm (A)
- B, E are independent
- B ⊥ E
- But B ⫫ E | A (NOT independent given A!)

**Why?** If alarm rings and no earthquake, burglar more likely!

## Practice Problems

### Problem 1: Basic Probability

Two dice rolled. Find P(sum ≥ 10).

**Solution:**
```
Favorable outcomes:
(4,6), (5,5), (5,6), (6,4), (6,5), (6,6)
= 6 outcomes

Total outcomes = 36

P(sum ≥ 10) = 6/36 = 1/6
```

### Problem 2: Bayes' Theorem

Spam filter:
- P(spam) = 0.3
- P("free"|spam) = 0.8
- P("free"|¬spam) = 0.1

Find P(spam|"free")

**Solution:**
```
P(spam|free) = P(free|spam) × P(spam) / P(free)

P(free) = P(free|spam)P(spam) + P(free|¬spam)P(¬spam)
        = 0.8(0.3) + 0.1(0.7)
        = 0.24 + 0.07 = 0.31

P(spam|free) = 0.8 × 0.3 / 0.31
             = 0.24 / 0.31
             ≈ 0.774 = 77.4%
```

### Problem 3: Independence

Are X, Y independent?

```
       Y=0  Y=1
X=0    0.2  0.3
X=1    0.1  0.4
```

**Solution:**
```
Check: P(X,Y) = P(X) × P(Y)?

P(X=0) = 0.5, P(Y=0) = 0.3
P(X=0, Y=0) = 0.2

P(X) × P(Y) = 0.5 × 0.3 = 0.15 ≠ 0.2

NOT independent!
```

## Summary

**Key Formulas:**
- P(A|B) = P(A∩B) / P(B)
- Bayes: P(A|B) = P(B|A)P(A) / P(B)
- Independence: P(A∩B) = P(A)P(B)
- E[X] = Σ x P(X=x)

**For GATE:**
- Master Bayes' theorem
- Understand conditional independence
- Practice joint probability tables
- Know when to use total probability

**Common Mistakes:**
- Confusing P(A|B) with P(B|A)
- Assuming independence incorrectly
- Forgetting to normalize probabilities

