# 2742. Painting the Walls

## Intuition

The key insight is that while a paid painter is painting one wall, the free painter can paint other walls during that time. We can use this to optimize our solution by considering the trade-off between cost and time.

## Approach

1. We use dynamic programming where `dp[i]` represents the minimum cost to paint `i` walls
2. Initialize dp array with maximum values except dp[0] = 0
3. For each wall:
   - We consider using the paid painter for this wall
   - While paid painter works on current wall, free painter can paint `time[i]` walls
   - We update dp[k] where k is min(total_walls, current_walls + 1 + time[i])
   - The minimum cost would be either existing cost or cost of painting current wall plus previous state
4. The final answer is stored in dp[len(cost)]

## Complexity

- Time complexity: O(n^2)
- Space complexity: O(n)

## Keywords

- Dynamic Programming
- Array
- Optimization
- Minimum Cost
- Two Painters Problem

## Code

```go
func paintWalls(cost []int, time []int) int {
    dp := make([]int, len(cost)+1)
    for i := range dp {
        dp[i] = math.MaxInt32
    }
    dp[0] = 0

    for i := range cost {
        for j := len(cost)-1; j >= 0; j -= 1 {
            k := min(len(cost), j+1+time[i])
            dp[k] = min(dp[k], dp[j]+cost[i])
        }
    }

    return dp[len(cost)]
}
```
