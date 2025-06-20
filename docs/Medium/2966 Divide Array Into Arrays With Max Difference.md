# 2966. Divide Array Into Arrays With Max Difference

## Intuition

The main idea is to group the sorted array into subarrays of size 3, ensuring that the maximum difference within each group does not exceed k. Sorting helps to minimize the difference within each group.

## Approach

1. Sort the input array `nums` in ascending order.
2. Iterate through the array in steps of 3, forming groups of three consecutive elements.
3. For each group, check if the difference between the largest and smallest element is greater than `k`. If so, return an empty array as it's impossible to form such groups.
4. If the group is valid, add it to the result.
5. Return the list of valid groups.

## Complexity

- Time complexity: O(n log n)
- Space complexity: O(n)

## Keywords

- Greedy
- Grouping

## Code

```go
func divideArray(nums []int, k int) [][]int {
    ret := make([][]int, 0)
    sort.Ints(nums)

    for i := 0; i < len(nums); i += 3 {
        tmp := []int{nums[i]}
        if (i + 1 < len(nums) && nums[i + 1] - nums[i] > k) || (i + 2 < len(nums) && nums[i + 2] - nums[i] > k) {
            return [][]int{}
        }
        if i + 1 < len(nums) {
            tmp = append(tmp, nums[i + 1])
        }
        if i + 2 < len(nums) {
            tmp = append(tmp, nums[i + 2])
        }
        ret = append(ret, tmp)
    }
    return ret
}
```
