# Greedy Algorithmic Paradigm

The core idea behind this algorithmic paradigm is that we attempt to find optimal solution to the problem by choosing the local optimal solution on each step, thus we incremental solve the problem.

Keep in mind that this doesn’t mean that we will **always** end up with the best solution but it always **aims** for it. In some cases we may end up with suboptimal solution.

In some cases suboptimal solution can be better then nothing, when checking each possible solution is either resource-inefficient or impossible. Therefore, we can use greedy algorithmic paradigm to come up with the solution that provides optimal or near-optimal output. 

Greedy Algorithms yield optimal solution to the problem if those two properties hold. 

- **Greedy Choice Property:** The optimal solution to the problem can be constructed from making best local choice on each step.
- **Optimal Substructure:** The optimal solution is includes all the optimal solutions to subproblems.

To identify problems that can be solved using greedy algorithmic paradigm, we need to answer to this question: “Can we incrementally built up the solution by making best choice on each step and would it eventually lead to optimal solution to the main problem?” If the answer is yes, then this problem is a good candidate for greedy algorithms.