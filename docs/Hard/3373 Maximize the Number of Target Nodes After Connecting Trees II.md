# 3373. Maximize the Number of Target Nodes After Connecting Trees II

## Intuition

The key insight is that when connecting two trees at any node, the nodes at even levels in one tree can be made to align with either even or odd levels in the other tree, depending on which maximizes our target nodes.

## Approach

1. First, we build adjacency lists for both trees using the given edges.
2. For each tree, we perform a BFS starting from node 0 to:
    - Mark nodes at even levels (including level 0)
    - Count the number of nodes at even and odd levels
    - Store this information in a boolean slice for tree1
3. For each node in tree1:
    - If it's at an even level (true in tree1Even), we can connect it to get:
        - All even nodes from tree1 (even1) + max(even2, odd2) from tree2
    - If it's at an odd level (false in tree1Even), we can connect it to get:
        - All odd nodes from tree1 (odd1) + max(even2, odd2) from tree2

## Complexity

- Time complexity: O(N + E)
- Space complexity: O(N + E)

## Keywords

- Graph
- Breadth-First Search (BFS)
- Tree
- Level traversal

## Code

```go
func maxTargetNodes(edges1 [][]int, edges2 [][]int) []int {
    buildTree := func(edges [][]int) map[int][]int {
        tree := make(map[int][]int)
        for _, e := range edges {
            tree[e[0]], tree[e[1]] = append(tree[e[0]], e[1]), append(tree[e[1]], e[0])
        }
        return tree
    }
    tree1, tree2 := buildTree(edges1), buildTree(edges2)

    getEvenSlice := func(tree map[int][]int) ([]bool, int, int) {
        even, q, level, visited, evenCnt, oddCnt := make([]bool, len(tree)), []int{0}, 1, make([]bool, len(tree)), 1, 0
        even[0], visited[0] = true, true
        for len(q) != 0 {
            popN := len(q)
            for _, n := range q {
                for _, next := range tree[n] {
                    if visited[next] {
                        continue
                    }
                    if level & 1 == 0 {
                        even[next], evenCnt = true, evenCnt + 1
                    } else {
                        oddCnt += 1
                    }
                    q, visited[next] = append(q, next), true
                }
            }
            q, level = q[popN:], level + 1
        }
        return even, evenCnt, oddCnt
    }
    tree1Even, even1, odd1 := getEvenSlice(tree1)
    _, even2, odd2 := getEvenSlice(tree2)
    maxTree2 := max(even2, odd2)

    ret := make([]int, len(tree1))
    for i, v := range tree1Even {
        if v {
            ret[i] = even1 + maxTree2
        } else {
            ret[i] = odd1 + maxTree2
        }
    }
    
    return ret
}
```
