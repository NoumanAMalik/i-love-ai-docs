## Iterative deepening search

- Iterative deepening search solves the problem of picking a good value for ` by trying Iterative deepening
search
all values: first 0, then 1, then 2, and so on—until either a solution is found, or the depthlimited search returns the failure value rather than the cutoff value.
- Iterative deepening combines many of the benefits of depth-first and breadth-first
search. Like depth-first search, its memory requirements are modest: O(bd) when there is a
solution, or O(bm) on finite state spaces with no solution.
- Like breadth-first search, iterative
deepening is optimal for problems where all actions have the same cost, and is complete on
finite acyclic state spaces, or on any finite state space when we check nodes for cycles all the
way up the path.

### Psudocode
      function ITERATIVE-DEEPENING-SEARCH(problem) returns a solution node or failure
            for depth = 0 to ∞ do
                result←DEPTH-LIMITED-SEARCH(problem, depth)
                if result 6= cutoff then return result

### Demo 

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs11-1024x420.png)
![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/iddfs2-1024x420.png)
