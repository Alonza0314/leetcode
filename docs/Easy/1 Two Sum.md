# 1. Two Sum

## Intuition

The problem requires finding two numbers in an array that add up to a target value. A straightforward approach would be using two nested loops to check every possible pair, but that would be inefficient. Instead, we can use a hash map to store previously seen numbers and their indices, which allows us to find complements in constant time.

## Approach

1. Create a hash map to store numbers and their indices
2. Iterate through the array once
3. For each number:
    - Calculate the complement (target - current number)
    - Check if the complement exists in the hash map
    - If found, return current index and the complement's index
    - If not found, store current number and its index in the hash map
4. Continue until a solution is found

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- Hash Map

## Code

```go
func twoSum(nums []int, target int) []int {
    ans := make([]int, 2, 2)
    tmp_map := make(map[int]int)
    size := len(nums)
    for i := 0; i < size; i++ {
        tmp := target-nums[i]
        if _, ok := tmp_map[tmp]; ok {
            ans[0] = i
            ans[1] = tmp_map[tmp]
            return ans
        }
        tmp_map[nums[i]] = i
    }
    return ans
}
```
