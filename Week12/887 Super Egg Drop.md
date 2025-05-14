# 887. Super Egg Drop

## Intuition

The traditional approach using dynamic programming with states (eggs, floors) leads to O(K*N²) complexity, which is too slow. Instead, we can reformulate the problem: if we have K eggs and can make M moves, what's the maximum number of floors we can cover?

## Approach

For each move, we have two possibilities when dropping an egg from a floor:

1. The egg breaks, so we can only use K-1 eggs for floors below
2. The egg doesn't break, so we can use K eggs for floors above

We use a mathematical recursion to calculate the maximum floors we can cover with K eggs and M moves:

- Let maxFloor[i] represent the maximum number of floors that can be determined with i eggs and current number of moves
- For each move, we update maxFloor[i] = 1 + maxFloor[i-1] + maxFloor[i]
  - maxFloor[i-1]: floors covered if egg breaks
  - maxFloor[i]: floors covered if egg doesn't break
  - +1: the current floor we're dropping from

We keep increasing the number of moves until the maximum floor we can check is greater than or equal to N.

## Complexity

- Time complexity: O(K log N)
- Space complexity: O(K)

## Keywords

- Dynamic Programming

## Code

```go
func superEggDrop(k int, n int) int {
    maxFloor := make([]int, k + 1)
    m := 0
    for ; maxFloor[k] < n; m += 1 {
        for i := k; i > 0; i -= 1 {
            maxFloor[i] = 1 + maxFloor[i - 1] + maxFloor[i]
        }
    }
    return m
}
```
