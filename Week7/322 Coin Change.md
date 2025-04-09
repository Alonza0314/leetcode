# 322. Coin Change

## Intuition

We can use dynamic programming to solve this problem by breaking it down into smaller subproblems. For each amount from 1 to the target amount, we can calculate the minimum number of coins needed by considering all possible coin denominations.

## Approach

1. First, handle edge cases:
   - If amount is 0, return 0
   - If any coin equals the amount, return 1
   - If all coins are larger than the amount, return -1
2. Initialize a DP array where dp[i] represents the minimum number of coins needed for amount i
3. Set initial values:
   - dp[0] = 0 (0 coins needed for amount 0)
   - All other values initialized to a large number (math.MaxInt - 1)
4. Sort the coins array for optimization
5. For each amount from 1 to target amount:
   - For each coin denomination:
     - If the coin is less than or equal to the current amount
     - Update dp[i] with the minimum of current value and (dp[i - coin] + 1)
6. Return dp[amount] if it's not the initial large value, otherwise return -1

## Complexity

- Time complexity: O(n * m)
- Space complexity: O(n)

## Keywords

- Dynamic Programming
- Bottom-up Approach

## Code

```go
func coinChange(coins []int, amount int) int {
    if amount == 0 {
        return 0
    }
    smallFlag := false
    for _, coin := range coins {
        if coin == amount {
            return 1
        } else if coin < amount {
            smallFlag = true
        }
    }
    if !smallFlag {
        return -1
    }

    dp := make([]int, amount + 1)
    for i := range dp {
        dp[i] = math.MaxInt - 1
    }
    dp[0] = 0

    sort.Ints(coins)

    for i := 1; i <= amount ; i += 1 {
        for _, coin := range coins {
            if coin <= i {
                dp[i] = min(dp[i], dp[i - coin] + 1)
            } else {
                break
            }
        }
    }
    if dp[amount] == math.MaxInt - 1 {
        return -1
    }
    return dp[amount]
}
```
