# 3355. Zero Array Transformation I

## Intuition

The problem requires us to check if we can transform an array into a zero array by applying a series of operations. Each operation decrements all elements within a given range [left, right]. The key insight is that we can track the cumulative effect of all operations at each position.

## Approach

1. Use two arrays `start` and `end` to track where operations begin and end
2. For each query [left, right]:
    - Increment `start[left]` to mark operation starting point
    - Increment `end[right]` to mark operation ending point
3. Iterate through the array:
    - Keep track of current active operations (`current`)
    - Add new operations starting at current position
    - Subtract operations ending at current position
    - Check if remaining value after applying operations is positive
    - If any value remains positive, return false

## Complexity

- Time complexity: O(n + q)
- Space complexity: O(n)

## Keywords

- Prefix Sum
- Range Operations

## Code

```go
func isZeroArray(nums []int, queries [][]int) bool {
    start, end := make([]int, len(nums)), make([]int, len(nums))
    for _, query := range queries {
        start[query[0]], end[query[1]] = start[query[0]] + 1, end[query[1]] + 1
    }

    current := 0
    for i := range nums {
        current += start[i]
        nums[i] -= current
        if nums[i] > 0 {
            return false
        }
        current -= end[i]
    }

    return true
}
```
