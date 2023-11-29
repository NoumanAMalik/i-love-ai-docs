## Bidirectional search
bidirectional search simultaneously
searches forward from the initial state and backwards from the goal state(s), hoping that the
two searches will meet. The motivation is that b
d/2 +b
d/2
is much less than b
d
(e.g., 50,000
times less when b=d =10).



### Psudocode

    function BIBF-SEARCH(problemF, fF, problemB, fB) returns a solution node, or failure
        nodeF ←NODE(problemF.INITIAL) // Node for a start state
        nodeB←NODE(problemB.INITIAL) // Node for a goal state
        frontierF ←a priority queue ordered by fF, with nodeF as an element
        frontierB←a priority queue ordered by fB, with nodeB as an element
        reachedF ←a lookup table, with one key nodeF.STATE and value nodeF
        reachedB←a lookup table, with one key nodeB.STATE and value nodeB
        solution←failure
        while not TERMINATED(solution, frontierF, frontierB) do
            if fF(TOP(frontierF)) < fB(TOP(frontierB)) then
                solution←PROCEED(F, problemF, frontierF, reachedF, reachedB, solution)
            else solution←PROCEED(B, problemB, frontierB, reachedB, reachedF, solution)
    return solution
    
    function PROCEED(dir, problem, frontier, reached, reached2, solution) returns a solution
            // Expand node on frontier; check against the other frontier in reached2.
            // The variable “dir” is the direction: either F for forward or B for backward.
        node←POP(frontier)
            for each child in EXPAND(problem, node) do
              s←child.STATE
            if s not in reached or PATH-COST(child) < PATH-COST(reached[s]) then
                reached[s]←child
                add child to frontier
                if s is in reached2 then
                    solution2←JOIN-NODES(dir, child, reached2[s]))
                    if PATH-COST(solution2) < PATH-COST(solution) then
                    solution←solution2
    return solution

### Demo 
![image](https://github.com/NoumanAMalik/i-love-ai-docs/assets/63931307/a76a9d12-413a-40fb-bd85-64f75b2125eb)
