# 3443. Maximum Manhattan Distance After K Changes

## Intuition

The problem asks for the maximum Manhattan distance that can be achieved after making at most k changes to a sequence of moves (N, S, W, E). The key idea is to track the net movement in each direction and, at each step, consider how changing up to k moves could further increase the distance.

## Approach

We maintain an array to record the cumulative movement in each of the four directions.

As we iterate through the string, we update the corresponding direction count. At every step, we calculate the current maximum and minimum values for the north-south and west-east axes.

The Manhattan distance is computed as the sum of the differences between the maximum and minimum values for both axes. To maximize the distance, we can change up to k moves, so we add 2 * min(k, xm + ym) to the distance, where xm and ym are the minimum values for the two axes. We keep track of the maximum distance found during the iteration.

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Manhattan distance
- Greedy

## Code

```go
func maxDistance(s string, k int) int {
    dir, ret := make([]int, 4), math.MinInt
    for _, d := range s {
        switch d {
        case 'N':
            dir[0] += 1
        case 'S':
            dir[2] += 1
        case 'W':
            dir[1] += 1
        case 'E':
            dir[3] += 1
        }
        xM, xm := max(dir[0], dir[2]), min(dir[0], dir[2])
        yM, ym := max(dir[1], dir[3]), min(dir[1], dir[3])
        ret = max(ret, xM + yM - xm - ym + 2 * min(k, xm + ym))
    }
    return ret
}
```
