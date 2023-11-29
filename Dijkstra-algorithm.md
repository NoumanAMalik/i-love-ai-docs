## Dijkstra’s algorithm or uniform-cost search

- When actions have different costs, an obvious choice is to use best-first search where the
evaluation function is the cost of the path from the root to the current node. This is called Dijkstra’s algorithm by the theoretical computer science community, and uniform-cost search Uniform-cost search
by the AI community
- The idea is that while breadth-first search spreads out in waves of uniform depth—first depth 1, then depth 2, and so on—uniform-cost search spreads out in waves
of uniform path-cost. The algorithm can be implemented as a call to BEST-FIRST-SEARCH
with PATH-COST as the evaluation function

### Psudocode
	    function Dijkstra(Graph, source):
    	for each vertex v in Graph:	// Initialization
        	dist[v] := infinity	// initial distance from source to vertex v is set to infinite
        	previous[v] := undefined	// Previous node in optimal path from source
    	dist[source] := 0	// Distance from source to source
    	Q := the set of all nodes in Graph	// all nodes in the graph are unoptimized - thus are in Q
    	while Q is not empty:	// main loop
        	u := node in Q with smallest dist[ ]
        	remove u from Q
            for each neighbor v of u:	// where v has not yet been removed from Q.
            	alt := dist[u] + dist_between(u, v)
                if alt < dist[v]	// Relax (u,v)
                	dist[v] := alt
        	        previous[v] := u
        return previous[ ]

### Demo 
![image](https://github.com/NoumanAMalik/i-love-ai-docs/assets/63931307/3648fa29-44a6-4f61-bdec-c8e72cf15e5f)
