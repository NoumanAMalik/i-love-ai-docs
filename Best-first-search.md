## 1. Best-first search

### Overview
How do we decide which node from the frontier to expand next? A very general approach
is called **best-first search**, in which we choose a node, n, with minimum value of some Best-first search
**evaluation function**, f(n). The psudocode shows the algorithm. On each iteration we choose Evaluation function
a node on the frontier with minimum f(n) value, return it if its state is a goal state, and
otherwise apply EXPAND to generate child nodes. Each child node is added to the frontier
if it has not been reached before, or is re-added if it is now being reached with a path that
has a lower path cost than any previous path. The algorithm returns either an indication of
failure, or a node that represents a path to a goal. By employing different f(n) functions, we
get different specific algorithms, which this chapter will cover. 
### Psudocode

    function BEST-FIRST-SEARCH(problem, f) returns a solution node or failure
        node←NODE(STATE=problem.INITIAL)
        frontier←a priority queue ordered by f , with node as an element
        reached←a lookup table, with one entry with key problem.INITIAL and value node
        **while not** IS-EMPTY(frontier) do
            node←POP(frontier)
            if problem.IS-GOAL(node.STATE) then return node
            for each child in EXPAND(problem, node) do
            s←child.STATE
            if s is not in reached or child.PATH-COST < reached[s].PATH-COST then
                reached[s]←child
                add child to frontier
    return failure
    
    function EXPAND(problem, node) yields nodes
        s←node.STATE
        for each action in problem.ACTIONS(s) do
            s'←problem.RESULT(s, action)
            cost←node.PATH-COST + problem.ACTION-COST(s, action,s')
            yield NODE(STATE=s', PARENT=node, ACTION=action, PATH-COST=cost)


### Demo
![](https://blog.finxter.com/wp-content/uploads/2021/12/Best-first-search.gif)
