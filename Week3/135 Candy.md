# 135 Candy

## Intuition

For candy distribution, I use two times of traversal from left to right and right to left, and record the number of candies satisfied the requirement.

## Approach

1. First, initialize a candy array of the same length as the ratings array
2. First pass (left to right):
    - For the first child, give 2 candies if their rating is higher than the second child, otherwise give 1
    - For other children, if the current child's rating is higher than the left neighbor, give them 1 more candy than the left neighbor; otherwise, give just 1 candy
3. Second pass (right to left):
    - For each child, check their rating relationship with the right neighbor
    - If the current child's rating is higher than the right neighbor, they should have at least 1 more candy than the right neighbor
    - Take the maximum of the current value and the newly calculated value to ensure requirements from both directions are satisfied
4. During the second pass, accumulate the total number of candies given to all children

## Complexity

- Time complexity: O(N)
- Space complexity:O(N)

## Keywords

- Greedy

## Code

```go
func candy(ratings []int) int {
    if len(ratings) == 1 {
        return 1
    }

    ret, candy := 0, make([]int, len(ratings))

    if ratings[0] > ratings[1] {
        candy[0] = 2
    } else {
        candy[0] = 1
    }
    for i := 1; i < len(ratings); i += 1 {
        if ratings[i] <= ratings[i - 1] {
            candy[i] = 1
        } else {
            candy[i] = candy[i - 1] + 1
        }
    }

    ret = candy[len(candy) - 1]
    for i := len(ratings) - 2; i >= 0; i -= 1 {
        if ratings[i] <= ratings[i + 1] {
            candy[i] =  max(candy[i], 1)
        } else {
            candy[i] = max(candy[i], candy[i + 1] + 1)
        }
        ret += candy[i]
    }

    return ret
}
```
