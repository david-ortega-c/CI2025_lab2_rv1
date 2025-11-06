# CI2025_lab2

This project implements a **Genetic Algorithm (GA)** to solve the **Travelling Salesman Problem (TSP)**.

## Overview
The GA searches for the shortest Hamiltonian cycle that visits all cities exactly once and returns to the start. Each candidate solution is represented as a **permutation of city indices** (path representation).

## Algorithm Procedure

1. **Initialization**  
   The initial population is generated randomly. Optionally, some individuals are created using a **nearest-neighbor heuristic** to provide a good starting point.

2. **Fitness Evaluation**  
   The **fitness** of each individual is the total tour distance, including the return to the starting city. The GA minimizes this value.

3. **Parent Selection**  
   Two selection methods are available:
   - **Tournament selection:** randomly pick a small subset (e.g., 3 individuals) and choose the best among them.  
   - **Rank selection:** individuals are ranked by fitness, and selection probabilities are proportional to their rank, preserving diversity.

4. **Crossover Operators**  
   Several crossover mechanisms are implemented to combine parents:
   - **Ordered Crossover (OX):** copies a random slice from parent 1 and fills the remaining cities in the order they appear in parent 2.  
   - **Cycle Crossover (CX):** preserves position information by swapping cycles of cities between parents.  
   - **Edge Recombination:** builds the child by preserving shared edges between parents.  
   - **Order-Based / Position-Based Crossovers:** partially inherit city positions from both parents to ensure structural diversity.

5. **Mutation Operators**  
   Mutation introduces small random changes in the offspring:
   - **Swap Mutation:** exchange two randomly chosen cities.  
   - **Insertion Mutation:** remove a city and reinsert it at another position.  
   - **Inversion Mutation:** reverse the order of cities in a random segment of the tour.  
   These operations help maintain genetic diversity and avoid premature convergence.

6. **Replacement and Elitism**  
   The next generation is formed by selecting the best individuals (elitism) along with new offspring, ensuring steady progress toward better tours.

7. **Termination**  
   The algorithm runs until reaching a maximum number of generations or convergence of the population’s fitness.

## Output
The program returns the best tour found, its total distance, and optionally visualizations of:
- Fitness convergence over generations  
- The final optimal or near-optimal route

