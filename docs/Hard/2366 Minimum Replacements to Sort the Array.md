# 2366. Minimum Replacement to Sort the Array

## Intuition

This is definitely math question. The two key points are to compute `parts` and the `prev` value. As the hint saying, we need to start from the second-to-last since the last one can't be splitted.

## Approach

1. Start traversing from the second-to-last element of the array
2. For each element nums[i], if it's greater than the previously processed element prev:
    - Calculate how many parts we need to split the current number into: `(num + prev - 1) / prev`
    - Update prev to be the minimum value after splitting: `num / parts`
    - Add the number of operations needed: `partss - 1`
3. If the current element is less than or equal to prev, no splitting is needed
4. Return the total number of operations

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Greedy Algorithm
- Mathematics

## Code

```go
func minimumReplacement(nums []int) int64 {
    if len(nums) == 1 {
        return 0
    }
    ret := int64(0)
    var split func(num int, prev *int) int
    split = func(num int, prev *int) int {
        if num <= *prev {
            *prev = num
            return 0
        }
        tmp := (num + *prev - 1) / *prev
        *prev = num / tmp
        return tmp - 1
    }
    prev := nums[len(nums) - 1]
    for i := len(nums) - 2; i >= 0; i -= 1 {
        ret += int64(split(nums[i], &prev))
    }
    return ret
}
```
