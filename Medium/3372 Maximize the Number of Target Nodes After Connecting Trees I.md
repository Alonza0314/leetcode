# 3372. Maximize the Number of Target Nodes After Connecting Trees I

## Intuition

The problem involves connecting two trees and finding the maximum number of nodes that can be reached within k steps from each possible root node in tree1. The key insight is that we can:

1. Calculate reachable nodes for each node in tree1 with k steps
2. Find the maximum reachable nodes from any node in tree2 with k-1 steps
3. Combine these results to get the final answer

## Approach

1. First, we build adjacency lists for both trees using the given edges.
2. Create a BFS function that:
   - Takes a tree, root node, and step limit k
   - Returns the count of nodes reachable within k steps
   - Uses a queue to track nodes to visit
   - Uses visited array to avoid cycles
3. Main algorithm:
   - For each node i in tree1, calculate reachable nodes with k steps
   - Find the maximum reachable nodes from any node in tree2 with k-1 steps
   - For each node i in tree1, result[i] = tree1Count[i] + tree2MaxCount

## Complexity

- Time complexity: O(N * (V + E))
- Space complexity: O(V)

## Keywords

- Graph
- BFS
- Tree

## Code

```go
func maxTargetNodes(edges1 [][]int, edges2 [][]int, k int) []int {
    buildTree := func(edges [][]int) map[int][]int {
        tree := make(map[int][]int)
        for _, e := range edges {
            tree[e[0]], tree[e[1]] = append(tree[e[0]], e[1]), append(tree[e[1]], e[0])
        }
        return tree
    }
    tree1, tree2 := buildTree(edges1), buildTree(edges2)

    bfs := func(tree map[int][]int, root, kk int) int {
        count, q, visited := 0, []int{root}, make([]bool, len(tree))
        visited[root] = true
        for len(q) != 0 && kk >= 0 {
            popN := len(q)
            for _, n := range q {
                for _, next := range tree[n] {
                    if visited[next] {
                        continue
                    }
                    q, visited[next] = append(q, next), true
                }
            }
            q, kk, count = q[popN:], kk - 1, count + popN
        }
        return count
    }

    tree1Cnt, tree2Cnt := make([]int, len(tree1)), math.MinInt
    for i := range tree1Cnt {
        tree1Cnt[i] = bfs(tree1, i, k)
    }
    for i := range tree2 {
        tree2Cnt = max(tree2Cnt, bfs(tree2, i, k - 1))
    }

    ret := make([]int, len(tree1))
    for i := range ret {
        ret[i] = tree1Cnt[i] + tree2Cnt
    }

    return ret
}
```
