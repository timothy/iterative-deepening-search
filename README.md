# Iterative Deepening Search

```python
function iterativeDeepeningSearch(problem):
    depth = 0 to ∞
    result = depthLimitedSearch(problem, depth)
    if result != cutoff: then return result

function depthLimitedSearch(promblem, depth): return a node or failure or cutoff
  fronier <- LIFO queue (stack) with NODE(problem.INITIAL) as an element
  result <- failure
  while not IsEmpty(frontier) do:
    node <- POP(frontier)
    if problem.IsGoal(node.STATE) then return node
    if DEPTH(node) > depth then:
      result <- cutoff
    else if not IsCycle(node) do:
      add child to frontier
  return result
```

ref: 3.4 81
