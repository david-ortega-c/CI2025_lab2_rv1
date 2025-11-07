# CI2025_lab2 — Genetic Algorithm for the Travelling Salesman Problem (TSP)

## Overview
This project implements a **Genetic Algorithm (GA)** to approximate optimal solutions to the **Travelling Salesman Problem (TSP)** — finding the shortest possible route that visits each city exactly once and returns to the starting city.

Each candidate tour is represented as a **permutation of city indices** (path representation).

## Algorithm

1. **Input & Validation**
   - The TSP problem is represented as a distance matrix.
   - Sanity checks verify non-negativity, zero diagonal and symmetry.

2. **Initialization (Greedy Nearest-Neighbor)**
   - Each individual is constructed using a **nearest-neighbor heuristic**:
     - Start from a random city.
     - Repeatedly pick the next city from among the *k* closest unvisited cities (with `k ∈ [1, 3]`).
     - This produces good-quality but diverse starting tours.
   - Each individual is represented as:
     ```python
     Individual(genotype, fitness)
     ```

3. **Fitness Evaluation**
   - Fitness = total tour distance (including return to start).

4. **Parent Selection**
   - **Tournament selection** (`τ=3`): select a small subset and choose the fittest.
   - **Rank-Based Selection**  
  Individuals are first sorted by fitness (best to worst) and assigned ranks.  
  Selection probabilities are then derived from these ranks — higher-ranked individuals have proportionally higher chances of being chosen, but all individuals retain a non-zero probability.  

5. **Crossover Operators**

   - **Ordered Crossover (OX1)**  
     Selects a random slice from the first parent and copies it directly into the child.  
     The remaining cities are filled in the order they appear in the second parent, skipping any already included.  

   - **Edge Recombination Crossover (ERX)**  
     Builds the child incrementally by focusing on **edges** (connections between cities) rather than order.  
     A global edge table is created for both parents, listing neighbors for each city.  
     The offspring is constructed by repeatedly selecting the next city with the fewest available edges, preferring edges shared by both parents.  

   - **Order-Based Crossover (OX2)**  
      Selects a random subset of positions from the second parent and extracts the cities occupying those positions.  
      These selected cities are **removed** from the first parent’s tour, and then **reinserted** at the same positions as in the second parent, preserving their order.  

---

6. **Mutation Operators**
   - **Insertion Mutation**  
     Randomly selects one city, removes it from its position, and reinserts it into another random position in the tour.  

7. **Replacement & Elitism**
   - The offspring is added to the previous population and only the best  ```population_size ``` individuals are kept.

8. **Termination**
   - Stop after a fixed number of generations.


## Configuration Rationale by Problem Type

Different TSP problem sets (`g`, `r1`, and `r2`) exhibit distinct structural properties, so the algorithm uses different configurations to match their characteristics.

| Problem Type | Matrix Properties | GA Configuration | Rationale |
|---------------|------------------|------------------|------------|
| **g** | - No negative values  <br> - Zero diagonal  <br> - Symmetric distances | **Inversion Mutation + Edge Recombination + Rank Selection** | Since the problem is **symmetric**, preserving edge relationships is crucial. Edge Recombination maintains adjacency information effectively, while Rank Selection provides stable selective pressure without relying on raw fitness values. Inversion mutation further refines local order. |
| **r1** | - No negative values  <br> - Zero diagonal  <br> - **Asymmetric** distances | **Inversion Mutation + Ordered Crossover (OX2) + Tournament Selection** | Asymmetry means direction matters. OX2 efficiently preserves partial city sequences while allowing direction-dependent exploration. Tournament Selection accelerates convergence by consistently favoring fitter (shorter) tours. |
| **r2** | - **Contains negative values**  <br> - **Non-zero diagonal**  <br> - **Asymmetric** distances | **Inversion Mutation + Ordered Crossover (OX1) + Tournament Selection** | For noisy or irregular matrices, OX1 helps maintain feasible permutations. Tournament Selection keeps selective pressure strong enough to counter instability caused by negative or inconsistent distances. |