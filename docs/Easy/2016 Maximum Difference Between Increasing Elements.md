# 2016. Maximum Difference Between Increasing Elements

## Intuition

The problem asks for the maximum difference between two elements in the array such that the larger element comes after the smaller one. The intuition is to keep track of the minimum value seen so far and, for each element, calculate the difference if it is greater than the minimum.

## Approach

We iterate through the array while maintaining the minimum value encountered so far. For each element, if it is greater than the minimum, we compute the difference and update the result if this difference is larger than the current result. If the current element is smaller than the minimum, we update the minimum. If no such pair exists, we return -1.

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Minimum Tracking
- Greedy

## Code

```go
func maximumDifference(nums []int) int {
    m, ret := nums[0], -1
    for i := 1; i < len(nums); i += 1 {
        if nums[i] > m {
            ret = max(ret, nums[i] - m)
        } else if nums[i] < m {
            m = nums[i]
        }
    }
    return ret
}
```
