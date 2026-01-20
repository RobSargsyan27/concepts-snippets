# Divide and Conquer Algorithmic Paradigm

The core idea behind this paradigm is to split the original problem into subproblems, solve those subproblems individually and then merge the results of the subproblems to get the actual solution. 

In comparison with greedy algorithms where we incrementally built up the solution, here we come up with the solution by merging the solutions to the subproblems.  

Subproblems are independent from each other and don’t overlap, if they overlap then consider using dynamic programming paradigm. Even though D&C algorithms are usually very efficient, they introduce additional complexity and overhead (especially if implemented with recursion). 

A good fit for D&C algorithms are problems that can be recursively split into subproblems until we get to the base scenario, which is easy to solve, and then by merging the solutions from bottom to top yield solution for the main problem. 

Good examples of D&C algorithms are merge sort, quick sort, binary search or Euclidean algorithm.