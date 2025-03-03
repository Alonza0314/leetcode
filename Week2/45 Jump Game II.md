# 45. Jump Game II

## Intuition

This problem is solved using a greedy algorithm. The core idea is to always choose positions that can reach the farthest distance at each jump. Instead of actually simulating the jumping process, we maintain two pointers tracking the current maximum reachable position and the farthest position we can reach in the next jump to calculate the minimum number of jumps.

## Approach

1. Use three variables:
   - ret: records the minimum number of jumps
   - current: the farthest position reachable in the current jump
   - farthest: the farthest position reachable in the next jump from the current position

2. While traversing the array:
    - For each position i, calculate the farthest distance reachable from this position (i + nums[i])
    - Update farthest to be the maximum reachable distance
    - When we reach the current position, we need to make a jump:
        - Increment the jump count
        - Update current to farthest
        - If current can reach the end, break the loop

1. Finally, return the minimum number of jumps needed

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Greedy Algorithm

## Code

```go
func jump(nums []int) int {
    if len(nums) == 1 {
        return 0
    }
    ret, current, fartest := 0, 0, 0
    for i, num := range nums {
        fartest = max(fartest, i + num)
        if i == current {
            ret, current = ret + 1, fartest
            if current >= len(nums) - 1 {
                break
            }
        }
    }
    return ret
}
```
