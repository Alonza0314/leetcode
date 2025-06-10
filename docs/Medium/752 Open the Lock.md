# 752 Open the Lock

## Intuition

The problem can be modeled as a graph traversal problem where each node represents a lock combination (4-digit number) and edges represent valid moves (turning one wheel up or down). We need to find the shortest path from "0000" to the target combination while avoiding deadends. This naturally leads to using BFS (Breadth-First Search) as it guarantees finding the shortest path in an unweighted graph.

## Approach

1. Start with the initial combination "0000"
2. Use BFS to explore all possible combinations:
    - For each digit in the current combination, try turning it up and down
    - Skip combinations that are in the deadends list
    - Keep track of visited combinations to avoid cycles
    - Return the number of steps when the target is found
3. If the queue becomes empty without finding the target, return -1

## Complexity

- Time complexity: O(N), where N is the number of possible combinations (10^4 = 10000)
- Space complexity: O(N)

## Keywords

- BFS
- Graph Traversal

## Code

```go
func openLock(deadends []string, target string) int {
    if "0000" == target {
        return 0
    }
    dds, record, ret := make(map[string]bool, len(deadends)), make(map[string]bool), 0
    for _, dd := range deadends {
        dds[dd] = true
    }
    if dds["0000"] {
        return -1
    }
    record[target] = true

    newCurProcess := func(i int, destDigit byte, cur string, tmp *[]string) bool {
        curByte := []byte(cur)
        curByte[i] = destDigit
        cur = string(curByte)
        if dds[cur] {
            return false
        }
        if cur == target {
            return true
        }
        if _, found := record[cur]; !found {
            *tmp, record[cur] = append(*tmp, cur), true
        }
        return false
    }

    getNext := func(cur string, tmp *[]string) bool {
        for i := 0; i < 4; i += 1 {
            switch cur[i] {
            case '0':
                if newCurProcess(i, '1', cur, tmp) {
                    return true
                }
                if newCurProcess(i, '9', cur, tmp) {
                    return true
                }
            case '9':
                if newCurProcess(i, '0', cur, tmp) {
                    return true
                }
                if newCurProcess(i, '8', cur, tmp) {
                    return true
                }
            default:
                if newCurProcess(i, cur[i] + 1, cur, tmp) {
                    return true
                }
                if newCurProcess(i, cur[i] - 1, cur, tmp) {
                    return true
                }
            }
        }
        return false
    }

    queue := []string{"0000"}
    for len(queue) != 0 {
        ret += 1
        tmp := make([]string, 0)
        for _, cur := range queue {
            if getNext(cur, &tmp) {
                return ret
            }
        }
        queue = tmp
    }

    return -1
}
```
