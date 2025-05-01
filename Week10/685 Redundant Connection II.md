# 685. Redundant Connection II

## Intuition

The problem requires us to find a redundant edge in a directed graph that causes either a cycle or a node with two parents. The key insight is that there are two possible cases:

1. A node has two parents (in-degree > 1)
2. The graph contains a cycle

## Approach

1. First, we identify if there's a node with two parents by tracking the parent of each node. If found, we store the two candidate edges.
2. We use Union-Find (Disjoint Set Union) to detect cycles in the graph.
3. We process all edges except the second candidate edge (if it exists).
4. If we find a cycle:
   - If we had a node with two parents, return the first candidate edge
   - Otherwise, return the current edge that forms the cycle
5. If no cycle is found, return the second candidate edge (which must be the redundant one)

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- Union-Find
- Graph
- Cycle Detection

## Code

```go
func findRedundantDirectedConnection(edges [][]int) []int {
    parent, candidate1, candidate2 := make(map[int]int), -1, -1
    for i, edge := range edges {
        if ii, found := parent[edge[1]]; found {
            candidate1, candidate2 = ii, i
            break
        }
        parent[edge[1]] = i
    }

    root := make([]int, len(edges) + 1)
    for i := range root {
        root[i] = i
    }

    findRoot := func(n int) int {
        for root[n] != n {
            root[n] = root[root[n]]
            n = root[n]
        }
        return n
    }

    for _, edge := range edges {
        u, v := edge[0], edge[1]
        if candidate2 != -1 && u == edges[candidate2][0] && v == edges[candidate2][1] {
            continue
        }

        ur, uv := findRoot(u), findRoot(v)

        if ur == uv {
            if candidate1 != -1 {
                return edges[candidate1]
            }
            return edge
        }
        root[uv] = ur
    }

    return edges[candidate2]
}
```
