# Iterative Deepening Search (IDS) Algorithm

## Overview

Iterative Deepening Search (IDS) is a graph traversal and path search algorithm that combines the space-efficiency of Depth-First Search (DFS) with the completeness and optimality guarantees of Breadth-First Search (BFS). It systematically explores a tree or graph by performing a series of depth-limited searches with increasing depth limits until the goal is found.

## How It Works

The algorithm performs depth-first search to a limited depth, and repeats this process with incrementally increasing depth limits (0, 1, 2, 3, ...) until either:
- A solution is found
- The entire search space has been explored

## Enhanced Pseudocode

### Main Algorithm

```pseudocode
function IterativeDeepeningSearch(problem):
    // Iterate through increasing depth limits
    for depth from 0 to ∞:
        result = DepthLimitedSearch(problem, depth)
        
        // If we found a solution, return it
        if result is not CUTOFF:
            return result
    
    // If no solution exists at any depth
    return FAILURE
```

### Depth-Limited Search Component

```pseudocode
function DepthLimitedSearch(problem, maxDepth):
    // Initialize frontier as a stack (LIFO) for depth-first exploration
    frontier = Stack()
    frontier.push(Node(problem.initialState, parent=null, depth=0))
    
    // Track if we hit the depth limit
    cutoffOccurred = false
    
    // Keep track of visited states to avoid cycles
    visited = Set()
    
    while frontier is not empty:
        node = frontier.pop()
        
        // Check if we've reached the goal
        if problem.isGoal(node.state):
            return node
        
        // Mark as visited
        visited.add(node.state)
        
        // Check depth limit
        if node.depth >= maxDepth:
            cutoffOccurred = true
            continue  // Skip expansion of this node
        
        // Expand the node if within depth limit
        for action in problem.getActions(node.state):
            childState = problem.getResult(node.state, action)
            
            // Avoid revisiting states (cycle detection)
            if childState not in visited:
                childNode = Node(
                    state=childState,
                    parent=node,
                    action=action,
                    depth=node.depth + 1
                )
                frontier.push(childNode)
    
    // Return appropriate status
    if cutoffOccurred:
        return CUTOFF  // Depth limit was reached
    else:
        return FAILURE  // No solution exists at this depth
```

### Node Structure

```pseudocode
class Node:
    state        // Current state in the search space
    parent       // Parent node (for path reconstruction)
    action       // Action taken from parent to reach this node
    depth        // Depth of this node in the search tree
    pathCost     // Cost of path from root to this node (optional)
```

## Strengths and Advantages

### 1. **Optimal Memory Usage**
- Space complexity: O(bd) where b is branching factor and d is depth
- Uses only as much memory as DFS, making it suitable for problems with limited memory

### 2. **Completeness**
- Guaranteed to find a solution if one exists (for finite search spaces)
- Will systematically explore all nodes at each depth level

### 3. **Optimality**
- Finds the shallowest solution first (optimal for uniform path costs)
- Guarantees the shortest path in terms of number of steps

### 4. **No Need for Depth Estimation**
- Unlike pure depth-limited search, doesn't require knowing the solution depth in advance
- Automatically adjusts to find solutions at any depth

### 5. **Practical for Many Real-World Problems**
- Works well when solution depth is unknown
- Effective for problems where most solutions are relatively shallow

## When to Use Iterative Deepening Search

### Ideal Use Cases

1. **Tree Search Problems**
   - Game trees (chess, checkers)
   - Decision trees
   - Puzzle solving (8-puzzle, 15-puzzle)

2. **When Memory is Limited**
   - Embedded systems
   - Mobile applications
   - Large state spaces where BFS would exhaust memory

3. **Unknown Solution Depth**
   - Web crawling with depth limits
   - Network routing problems
   - Theorem proving

4. **Uniform Path Costs**
   - When all actions have the same cost
   - Finding shortest paths in unweighted graphs

### Not Recommended For

1. **Very Deep Solutions**
   - Problems where solutions are known to be very deep
   - Time complexity becomes prohibitive due to repeated exploration

2. **Non-Uniform Path Costs**
   - When different actions have different costs
   - Use A* or Dijkstra's algorithm instead

3. **Dynamic Environments**
   - When the search space changes during search
   - Real-time systems with changing constraints

## Time and Space Complexity

- **Time Complexity**: O(b^d) where b is branching factor, d is depth of shallowest solution
- **Space Complexity**: O(bd) - linear in the solution depth
- **Repeated Work**: Nodes at depth k are generated (d-k+1) times

Despite the repeated work, the time complexity is only marginally worse than BFS because most nodes are at the deepest level.

## Related Concepts

### Horizon Effect
The horizon effect occurs in depth-limited search when the algorithm cannot see beyond its depth limit, potentially missing important information just beyond the "horizon." IDS mitigates this by progressively extending the horizon.

### Quiescence Search
In game-playing programs, quiescence search extends the search at volatile positions (non-quiescent positions) to avoid the horizon effect. This ensures the evaluation happens at stable positions where the static evaluation function is more reliable.

## Implementation Tips

1. **Cycle Detection**: Implement proper cycle detection to avoid infinite loops
2. **Path Reconstruction**: Store parent pointers to reconstruct the solution path
3. **Early Termination**: Stop immediately when a goal is found
4. **Memoization**: Consider caching results for repeated states (though this increases memory usage)
5. **Depth Tracking**: Maintain accurate depth information for each node

## Example Application: 8-Puzzle

```python
# Example structure for 8-puzzle problem
class EightPuzzleProblem:
    def __init__(self, initial_state):
        self.initial_state = initial_state
        self.goal_state = [[1,2,3],[4,5,6],[7,8,0]]
    
    def is_goal(self, state):
        return state == self.goal_state
    
    def get_actions(self, state):
        # Return possible moves: UP, DOWN, LEFT, RIGHT
        # Based on empty tile position
        pass
    
    def get_result(self, state, action):
        # Return new state after applying action
        pass
```

## Main thoughts

Iterative Deepening Search is a powerful algorithm that elegantly combines the benefits of both depth-first and breadth-first search strategies. Its optimal memory usage and completeness guarantees make it an excellent choice for many search problems, particularly when memory is constrained and the solution depth is unknown. While it does perform redundant work by re-exploring shallow nodes, this overhead is often acceptable given its memory efficiency and guarantee of finding optimal solutions.
