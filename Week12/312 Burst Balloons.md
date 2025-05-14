# 312. Burst Balloons

## Intuition

The key insight is to think about the problem in reverse: instead of bursting balloons one by one, we can think about adding balloons back one by one. For each step, we consider which balloon to add last.

## Approach

1. Add two dummy balloons with value 1 at the beginning and end of the array.
2. Define `dp[left][right]` as the maximum coins that can be collected by bursting all balloons between indices left and right (exclusive).
3. For each subproblem of length, we try each balloon (at position tmp) as the last one to burst.
4. The recurrence relation is:
   `dp[left][right] = max(dp[left][right], dp[left][tmp] + dp[tmp][right] + nums[left] * nums[tmp] * nums[right])`
5. The final answer is `dp[0][n+1]`, representing the maximum coins obtained by bursting all original balloons.

## Complexity

- Time complexity: O(n³)
- Space complexity: O(n²)

## Keywords

- Dynamic Programming
- Interval DP

## Code

```go
func maxCoins(nums []int) int {
    nums = append([]int{1}, nums...)
    nums = append(nums, 1)

    dp := make([][]int, len(nums))
    for i := range dp {
        dp[i] = make([]int, len(nums))
    }

    for length := 2; length < len(nums); length += 1 {
        for left := 0; left < len(nums) - length; left += 1 {
            right := left + length
            for tmp := left + 1; tmp < right; tmp += 1 {
                dp[left][right] = max(dp[left][right], dp[left][tmp] + dp[tmp][right] + nums[left] * nums[tmp] * nums[right])
            }
        }
    }

    return dp[0][len(nums) - 1]
}
```
