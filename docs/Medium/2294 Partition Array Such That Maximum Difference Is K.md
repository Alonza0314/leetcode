# 2294. Partition Array Such That Maximum Difference Is K

## Intuition

The main idea is to group the numbers in such a way that the difference between the smallest and largest number in each group does not exceed K. By sorting the array first, we can efficiently form such groups in a greedy manner.

## Approach

First, sort the input array.
Then, iterate through the sorted array and start a new group whenever the current number exceeds the smallest number in the current group by more than K. Each time this happens, increment the group count and update the starting point of the new group.
At the end, add one to the result to account for the last group.

## Complexity

- Time complexity: O(n log n)
- Space complexity: O(1)

## Keywords

Greedy, Sorting, Array Partition

## Code

```go
func partitionArray(nums []int, k int) int {
    sort.Ints(nums)
    ret, cur := 0, nums[0]

    for _, n := range nums {
        if n - cur > k {
            ret, cur = ret + 1, n
        }
    }
    ret += 1

    return ret
}
```
