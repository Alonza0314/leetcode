# 948 Bag Of Tokens

## Intuition

Get the points by reducing the least power, and use only one points to gain the most power.

It's precisely a greedy problem.

## Approach

At first, check whether the length of "tockens" is 0. If it is, just return 0.

Second, sort the tokens, because I would like to get the least tocken (idx == front) and the most token (idx == back) as soon as possible.

For each loop, if power is greater than the least token (idx == 0), get it directly and increase the point. But if there doesn't have enough power, get the most token with decreasing one point.

However, for each time you get the point you need to check if it the most points record you get, since it may be decreased by get the most token.

Last, return the maximum point you have ever get!

## Complexity

- Time complexity: O(NlogN)
- Space complexity: O(1)

## Keywords

- Greedy
- Sort

## Code

```go
func bagOfTokensScore(tokens []int, power int) int {
    if len(tokens) == 0 {
        return 0
    }
    maxret, ret := 0, 0
    sort.Ints(tokens)
    minT, maxT := 0, len(tokens) - 1
    for minT <= maxT{
        if power >= tokens[minT] {
            power -= tokens[minT]
            minT += 1
            ret += 1
            maxret = max(maxret, ret)
        } else {
            if ret > 0 {
                power += tokens[maxT]
                maxT -= 1
                ret -= 1
            } else {
                break
            }
        }
    }
    return maxret
}
```
