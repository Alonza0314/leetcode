# 16. 3Sum Closest

## Intuition

The problem is similar to 3Sum, but instead of finding exact matches, we need to find the sum that is closest to the target. We can use a similar two-pointer approach after sorting the array, but we'll need to keep track of the closest sum we've found so far.

## Approach

1. Sort the array to enable the two-pointer technique
2. Initialize the result with the sum of first three numbers
3. For each number at index i:
    - Use two pointers (j and k) starting from i+1 and end of array
    - Calculate the current sum of three numbers
    - Update the result if current sum is closer to target
    - Move pointers based on comparison with target:
        - If sum < target, move left pointer right
        - If sum > target, move right pointer left
        - If sum == target, return immediately as it's the closest possible

## Complexity

- Time complexity: O(n²)
- Space complexity: O(1)

## Keywords

- Two Pointers
- Sorting

## Code

```go
func threeSumClosest(nums []int, target int) int {
    abs := func(x int) int {
        if x < 0 {
            return -x
        }
        return x
    }
    sort.Ints(nums)

    ret := nums[0] + nums[1] + nums[2]
    for i := 0; i < len(nums) - 2; i += 1 {
        j, k := i + 1, len(nums) - 1
        for j < k {
            tmp := nums[i] + nums[j] + nums[k]
            if abs(tmp - target) < abs(ret - target) {
                ret = tmp
            }

            if tmp < target {
                j += 1
            } else if tmp > target {
                k -= 1
            } else {
                return tmp
            }
        }
    }
    return ret
}
```
