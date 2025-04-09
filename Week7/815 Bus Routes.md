# 815 Bus Routes

## Intuition

The problem can be modeled as a graph where:

- Each bus route represents a set of connected stops
- We need to find the minimum number of bus changes required to go from source to target
- We can use BFS to explore all possible routes level by level, where each level represents one bus change

## Approach

1. First check if source and target are the same (return 0 if true)
2. Create a map `stopToBus` to store which buses stop at each station
3. Use BFS to explore all possible routes:
   - Start from the source stop
   - For each stop, check all buses that pass through it
   - For each bus, check all stops it can reach
   - If we reach the target, return the current number of bus changes
   - Keep track of visited stops and buses to avoid cycles
4. If we exhaust all possibilities without finding the target, return -1

## Complexity

- Time complexity: O(N + M), where N is the number of stops and M is the number of bus routes
- Space complexity: O(N + M)

## Keywords

- BFS
- Graph

## Code

```go
func numBusesToDestination(routes [][]int, source int, target int) int {
    if source == target {
        return 0
    }
    ret, n, stopToBus := 0, 0, make(map[int][]int)
    for i, route := range routes {
        for _, stop := range route {
            stopToBus[stop] = append(stopToBus[stop], i)
        }
    }
    
    queue, stopRecord, busRecord := []int{source}, map[int]bool{source: true}, make(map[int]bool)
    bfs := func(curStop int) bool {
        for _, bus := range stopToBus[curStop] {
            if busRecord[bus] {
                continue
            }
            busRecord[bus] = true
            for _, stop := range routes[bus] {
                if stopRecord[stop] {
                    continue
                }
                if stop == target {
                    return true
                }
                queue, stopRecord[stop] = append(queue, stop), true
            }
        }
        return false
    }
    for len(queue) != 0 {
        ret, n = ret + 1, len(queue)
        for _, curStop := range queue {
            if bfs(curStop) {
                return ret
            }
        }
        queue = queue[n:]
    }

    return -1
}
```
