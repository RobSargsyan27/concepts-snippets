# Brute Force Algorithmic Paradigm

The brute force algorithmic paradigm also known as brute force searching is the most straightforward and reliable way to find solution for the given problem. On its core it is about testing each possible solution in the problem’s domain. 

In most real world scenarios where code simplicity, reliability is more important and where the problem has a small size of possible solutions, brute force algorithms can be a good choice. In most production-grade platforms, simplicity and bullet proofines is more important then couple of saved milliseconds, that’s said, those kind of algorithms rely heavily on computational power and less of “creativeness” of the algorithms. 

However, there are use cases, where the brute force algorithms fail badly, for example, “Traveling Salesman” problem where combinatorial explosion is inevitable for most real-world scenarios. In such cases more heuristic approaches are much more performant and reliable.  

Remember that true brute force algorithms always check all possible solutions to the problem. If there is any heuristic approach involved, for example, pruning search space or aim for best approximate solution rather then optimal one, then it is not true brute force algorithmic paradigm, despite the fact that it is highly recommended. Any kind of early-stopping logic is also considered as an optimization.

Example of brute force search is trying to pick the pin code of the credit card by checking all
$4 * 4 * 4 * 4$ solutions.