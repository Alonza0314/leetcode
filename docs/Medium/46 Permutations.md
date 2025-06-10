# 46. Permutations

## Intuition

The key insight is that we can build permutations recursively by selecting each number as the first element and then generating permutations for the remaining numbers.

## Approach

1. Use a recursive backtracking approach
2. For each recursive call:
    - If there are no numbers left to permute, add the current permutation to the result
    - Otherwise, iterate through the remaining numbers
    - For each number, create a new permutation by adding it to the current permutation
    - Recursively call the function with the remaining numbers (excluding the current number)
3. The base case is when there are no numbers left to permute

## Complexity

- Time complexity: O(n * n!)
- Space complexity: O(n * n!)

## Keywords

- Backtracking
- Recursion
- Permutation

## Code

```go
func recursion(cur, nums []int, ret *[][]int) {
    if len(nums) == 0 {
        *ret = append(*ret, cur)
        return
    }
    for i, num := range nums {
        cp := make([]int, len(nums))
        copy(cp, nums)
        recursion(append(cur, num), append(cp[:i], cp[i + 1:]...), ret)
    }
}
func permute(nums []int) [][]int {
    ret := make([][]int, 0)
    recursion([]int{}, nums, &ret)
    return ret
}
```
