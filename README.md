# CI2025_lab1

## Overview

This repository contains my solution for **Lab 1** of the course *Computational Intelligence (CI2025/26)*.
It consists in solving 3 instances of the **multiple 1-0 knapsack problem** using computational intelligence techniques.

---

## Problem definition

The multiple 1-0 knapsack problem extends the classical knapsack problem to multiple knapsacks and multiple constraint dimensions.

* Each **item** has:

  * A value `v_i`
  * A weight vector `w_i ∈ ℝ^NUM_DIMENSIONS`
* Each **knapsack** has:

  * A capacity vector `c_k ∈ ℝ^NUM_DIMENSIONS`

The goal is to assign items to knapsacks in a way that:

* Maximizes the **total value** of assigned items,
* Respects the **capacity constraints** in every dimension.


## Representation of solutions

Solutions are represented as an integer array of length `NUM_ITEMS`:

```
solution[i] = k   → item i is assigned to knapsack k
solution[i] = -1  → item i is not assigned to any knapsack
```

This guarantees that each item can be assigned to at most one knapsack, keeping manipulation and evaluation efficient.

---

## Solution evaluation

The function `check_sol(solution)` performs three key tasks:

1. **Validity check** – verifies that each knapsack respects its constraints.
2. **Scoring** – sums the values of all assigned items.
3. **Cost computation** – measures normalized violations (if any) to guide the search toward valid regions.

The function returns a tuple:
`(valid, score, cost)`
where `valid` is a boolean flag, and `cost` represents constraint violation severity.

---

## Algorithm description

### 1. Initialization

The algorithm starts from a **greedy solution**, built by iteratively assigning each item to the knapsack that would produce the smallest overload (or leaving it unassigned if there are no valid placements).
This approach provides a better starting point than random initialization, especially for the third problem.

---

### 2. Phase 1 — validity search

The first phase focuses on finding a **valid configuration** (no violations).
This is achieved through a **simulated annealing process** combined with a **tabu search mechanism** for diversification.

* `tweak_findValid()` identifies the knapsack with the highest overload and attempts to move or remove the items contributing most to the violation.
* Recently modified items are stored in a **tabu list**, preventing immediate reconsideration.
* Acceptance of new solutions follows the simulated annealing rule:

  ```
  P(accept) = exp(-Δcost / T)
  ```

  allowing the algorithm to occasionally accept worse solutions to escape local minima.
* The temperature `T` gradually decreases with each iteration.

This phase ends when a valid solution is reached or a maximum iteration limit is met.

---

### 3. Phase 2 — Value optimization

Once a valid solution is available, the second phase aims to **increase the total value** of the assignment while keeping it valid.

* The `tweak_findBest()` operator explores the neighborhood by:

  * Swapping low-value assigned items with high-value unassigned ones,
  * Occasionally adding new items to encourage exploration.
* Only valid solutions are considered; occasionally, slightly worse valid solutions are accepted (5% probability) to prevent stagnation.

---

## Experimental Results

The algorithm was tested on the three instances of the problem.
Below are the key results for the largest case:

```
Valid solution found after 1 iteration.
Final solution: 2546 items assigned
Total value: 1,571,200
Valid: True
Value percentage: 63.08% of theoretical maximum
```

The algorithm achieved valid, high-value assignments while maintaining a reasonable runtime.

---

**Author:** [Riccardo Dattena (helped by ChatGPT for a couple of lines)]
**Course:** Computational Intelligence (CI2025/26)
**Lab 1 — Multiple 1-0 knapsack problem**
