# 847. Shortest Path Visiting All Nodes

## Intuition

The problem requires finding the shortest path that visits all nodes in an undirected graph. We can think of this as a state-based BFS problem where each state consists of:

1. The current node we're at
2. A bitmask representing which nodes we've visited so far

## Approach

1. Use BFS to explore all possible paths while keeping track of visited states
2. Each state is represented by a combination of:
   - Current node position
   - Bitmask indicating visited nodes (1 << i for node i)
3. Initialize the queue with all possible starting nodes
4. For each state, explore all neighboring nodes
5. Use a visited map to avoid revisiting the same state
6. The target is reached when all nodes are visited (bitmask equals 2^n - 1)

## Complexity

- Time complexity: O(n * 2^n)
- Space complexity: O(n * 2^n)

## Keywords

- BFS
- Bitmask
- State-based Search
- Graph Traversal
- Shortest Path

## Code

```go
func shortestPathLength(graph [][]int) int {
    if len(graph) == 1 {
        return 0
    }

    type unit struct {
        n int
        mask int
    }
    newUnit := func(i, m int) *unit {
        return &unit{
            n: i,
            mask: m,
        }
    }

    visited := make([]map[int]bool, len(graph))
    queue := make([]*unit, 0)
    for i := range graph {
        mask := 1 << i
        visited[i] = make(map[int]bool)
        queue = append(queue, newUnit(i, mask))
    }

    target, ret := 1 << len(graph) - 1, 1
    for {
        n := len(queue)
        for _, cur := range queue {
            for _, nb := range graph[cur.n] {
                nextMask := cur.mask | (1 << nb)
                if !visited[nb][nextMask] {
                    if nextMask == target {
                        goto RETURN
                    }
                    visited[nb][nextMask], queue = true, append(queue, newUnit(nb, nextMask))
                }
            }
        }
        queue, ret = queue[n:], ret + 1
    }

RETURN:
    return ret
}
```
