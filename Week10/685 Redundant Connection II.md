# 685. Redundant Connection II

## Intuition

## Approach

## Complexity

- Time complexity:
- Space complexity:

## Keywords

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
