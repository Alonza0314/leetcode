# 834. Sum of Distances in Tree

## Intuition

A key observation is that we can leverage the tree structure and parent-child relationships to compute these sums efficiently. Instead of calculating distances from scratch for each node, we can use the results from a parent node to derive the results for its children.

## Approach

The solution uses a two-pass DFS approach:

1. First DFS (dfs1):
    - Calculates the number of child nodes for each node
    - Computes the initial sum of distances for the root node (0)
    - This pass builds the foundation for the second pass
2. Second DFS (dfs2):
    - Uses the results from the first pass to compute distances for all other nodes
    - For each child node, the sum of distances can be derived from its parent's sum using the formula:
        `ret[child] = ret[parent] + (n - 2 * childNodes[child])`
    - This formula works because moving from parent to child:
        - Increases distance by 1 for all nodes not in the child's subtree
        - Decreases distance by 1 for all nodes in the child's subtree

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- DFS
- Distance Calculation

## Code

```go
func sumOfDistancesInTree(n int, edges [][]int) []int {
    ret, childNodes, tree := make([]int, n), make([]int, n), make([][]int, n)
    for _, edge := range edges {
        a, b := edge[0], edge[1]
        tree[a], tree[b] = append(tree[a], b), append(tree[b], a)
    }

    var dfs1 func(cur, prev int)
    dfs1 = func(cur, prev int) {
        childNodes[cur] = 1
        for _, neighbor := range tree[cur] {
            if neighbor == prev {
                continue
            }
            dfs1(neighbor, cur)
            childNodes[cur] += childNodes[neighbor]
            ret[cur] += ret[neighbor] + childNodes[neighbor]
        }
    }

    var dfs2 func(cur, prev int)
    dfs2 = func(cur, prev int) {
        for _, neighbor := range tree[cur] {
            if neighbor == prev {
                continue
            }
            ret[neighbor] = ret[cur] + (n- 2 * childNodes[neighbor])
            dfs2(neighbor, cur)
        }
    }

    dfs1(0, -1)
    dfs2(0, -1)

    return ret
}
```
