# 416. Partition Equal Subset Sum

## Intuition

The problem can be transformed into finding if we can select some numbers from the array that sum up to half of the total sum. This is essentially a 0/1 knapsack problem where we need to determine if we can fill a knapsack of capacity sum/2 using the given numbers.

## Approach

1. First, calculate the total sum of the array
2. If the sum is odd, return false as equal partition is impossible
3. Divide the sum by 2 to get our target sum
4. Use dynamic programming to solve:
   - Create a dp array where dp[j] represents if we can make sum j using the numbers
   - For each number, we can either include it or not include it
   - Use a temporary array to avoid affecting previous calculations
   - If at any point we can make the target sum, return true
5. As an optimization, we also:
   - Sort the array first
   - Use binary search to check if target sum exists directly in array

## Complexity

- Time complexity: O(n * sum)
- Space complexity: O(sum)

## Keywords

- Dynamic Programming
- 0/1 Knapsack
- Binary Search

## Code

```go
func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    if sum & 1 == 1 {
        return false
    }
    sum >>= 1
    sort.Ints(nums)
    _, flag := slices.BinarySearch(nums, sum)
    if flag {
        return true
    }
    dp := make([]bool, sum + 1)
    dp[0] = true
    for _, num := range nums {
        tmp := make([]bool, sum + 1)
        tmp[0] = true
        for j := 1; j <= sum ; j += 1 {
            tmp[j] = dp[j]
            if j >= num {
                tmp[j] = (tmp[j] || dp[j - num])
            }
        }

        if tmp[sum] {
            return true
        }

        copy(dp, tmp)
    }
    return dp[sum]
}
```
