# Propositional Logic 🧮

> **Complete guide for GATE DA with truth tables, inference rules, and SAT**

## Syntax and Semantics

### Atomic Propositions
- Simple statements (P, Q, R)
- True or False

### Logical Connectives

| Symbol | Name | Meaning |
|--------|------|---------|
| ¬ | NOT | negation |
| ∧ | AND | conjunction |
| ∨ | OR | disjunction |
| → | IMPLIES | implication |
| ↔ | IFF | biconditional |

### Truth Tables

**NOT:**
| P | ¬P |
|---|-----|
| T | F |
| F | T |

**AND:**
| P | Q | P∧Q |
|---|---|-----|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | F |

**OR:**
| P | Q | P∨Q |
|---|---|-----|
| T | T | T |
| T | F | T |
| F | T | T |
| F | F | F |

**IMPLIES:**
| P | Q | P→Q |
|---|---|-----|
| T | T | T |
| T | F | F |
| F | T | T |
| F | F | T |

**IFF:**
| P | Q | P↔Q |
|---|---|-----|
| T | T | T |
| T | F | F |
| F | T | F |
| F | F | T |

### Important Equivalences

```
De Morgan's Laws:
¬(P ∧ Q) ≡ ¬P ∨ ¬Q
¬(P ∨ Q) ≡ ¬P ∧ ¬Q

Implication:
P → Q ≡ ¬P ∨ Q

Contrapositive:
P → Q ≡ ¬Q → ¬P

Biconditional:
P ↔ Q ≡ (P → Q) ∧ (Q → P)
```

## Inference Rules

### Modus Ponens
```
P → Q
P
------
∴ Q
```

### Modus Tollens
```
P → Q
¬Q
------
∴ ¬P
```

### Resolution
```
P ∨ Q
¬Q ∨ R
----------
∴ P ∨ R
```

### GATE Example

**Given:**
1. If it rains, the ground is wet: R → W
2. The ground is not wet: ¬W
3. Prove: It did not rain

**Solution:**
```
1. R → W     (Given)
2. ¬W        (Given)
3. ¬W → ¬R   (Contrapositive of 1)
4. ¬R        (Modus Ponens on 2, 3)
```

## Conjunctive Normal Form (CNF)

**Definition:** Conjunction of disjunctions
```
(P ∨ Q) ∧ (¬R ∨ S) ∧ (P ∨ ¬Q ∨ R)
```

### Converting to CNF

**Example:** Convert (P → Q) ∧ R to CNF

```
Step 1: Eliminate →
(P → Q) ≡ ¬P ∨ Q

Step 2: Apply
(¬P ∨ Q) ∧ R

Already in CNF!
```

## SAT Problem

**Given:** Formula in CNF  
**Find:** Assignment making it TRUE

**Example:**
```
(P ∨ Q) ∧ (¬P ∨ R) ∧ (¬Q ∨ ¬R)

Solution: P=T, Q=F, R=T
Check:
(T ∨ F) ∧ (F ∨ T) ∧ (T ∨ F)
= T ∧ T ∧ T = T ✓
```

**NP-Complete:** No known polynomial algorithm

## Practice Problems

### Problem 1
Prove: (P → Q) ∧ (Q → R) ⊢ P → R

**Solution:**
```
1. P → Q           (Given)
2. Q → R           (Given)
3. P               (Assumption)
4. Q               (Modus Ponens 1,3)
5. R               (Modus Ponens 2,4)
6. P → R           (→ Introduction)
```

### Problem 2
Convert to CNF: ¬(P ∧ Q) ∨ R

**Solution:**
```
Step 1: De Morgan
(¬P ∨ ¬Q) ∨ R

Step 2: Associate
¬P ∨ ¬Q ∨ R

CNF: (¬P ∨ ¬Q ∨ R)
```

## Summary

**Key Concepts:**
- Logical connectives and truth tables
- Inference rules (Modus Ponens, Resolution)
- CNF conversion
- SAT solving

**For GATE:**
- Practice truth table construction
- Master inference rules
- Convert to CNF quickly
- Identify valid arguments

