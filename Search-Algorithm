# Search Algorithms

Search algorithms are fundamental techniques used to find a specific item or information in a collection of data. These algorithms are employed in various applications, ranging from information retrieval in databases to pathfinding in computer games. Here, we'll explore some commonly used search algorithms.

## 1. Linear Search

Linear search, also known as sequential search, involves checking each element in a list one by one until the target item is found or the entire list is traversed.

### Pseudocode


## 2. Binary Search

Binary search is an efficient algorithm for finding a target element in a sorted list. It works by repeatedly dividing the search interval in half.

### Pseudocode

while low <= high:
    mid = (low + high) // 2
    if list[mid] == target:
        return mid        # Target found
    elif list[mid] < target:
        low = mid + 1
    else:
        high = mid - 1

return -1                # Target not found


## 3. Depth-First Search (DFS)

DFS is an algorithm for traversing or searching tree or graph data structures. It explores as far as possible along each branch before backtracking.

### Pseudocode


## 4. Breadth-First Search (BFS)

BFS is an algorithm for traversing or searching tree or graph data structures. It explores all the vertices at the current depth prior to moving on to vertices at the next depth level.

### Pseudocode

while queue is not empty:
    current_node = dequeue
    visit(current_node)

    for each neighbor in current_node:
        if neighbor is not visited:
            enqueue neighbor
            mark neighbor as visited

