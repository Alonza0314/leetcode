# 3423. Maximum Difference Between Adjacent Elements in a Circular Array

## Intuition

The problem requires finding the maximum absolute difference between any two adjacent elements in a circular array. Since it's a circular array, we need to consider the difference between the first and last elements as they are also adjacent in a circular arrangement.

## Approach

1. Define a helper function `abs` to calculate the absolute value of a number
2. Initialize the result variable `ret` with the minimum possible integer value
3. Iterate through the array from index 0 to n-2:
    - Calculate the absolute difference between current element and next element
    - Update the maximum difference if current difference is larger
4. Finally, check the difference between the last and first elements (circular connection)
5. Return the maximum difference found

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Circular Array
- Absolute Difference
- Math

## Code

```go
func maxAdjacentDistance(nums []int) int {
    abs := func(x int) int {
        if x < 0 {
            return -x
        }
        return x
    }

    ret := math.MinInt
    for i := 0; i < len(nums) - 1; i += 1 {
        ret = max(ret, abs(nums[i] - nums[i + 1]))
    }

    return max(ret, abs(nums[len(nums) - 1] - nums[0]))
}
```
