# 1395 Count Number of Teams

## Intuition

Use the "simplest" DP method to solve the problem. For each rating value, go "finding" how many numbers make me be the third member in the team and "record" how many numbers make be the middle member in the team.

## Approach

**First**, make two dp array for recording increase condition and decrease condition.

**Second**, go through the rating array and check the numbers before the current index-i whether it is comply the rules, i.e. increase or decrease.

**Third**, if true, the increase[j] / decrease[j] value means rating[i] will be the third member in these teams. So add it to the ret(urn) value. And also update the increase[i] / decrease[i] value, which means rating[j] and rating[j] will be the first and middle member of the team and waiting for the third member in the following round.

## Complexity

- Time complexity: O(N^2)
- Space complexity: O(N)

## Keywords

- DP

## Code

```go
func numTeams(rating []int) int {
    ret, increase, decrease := 0, make([]int, len(rating)), make([]int, len(rating))
    for i := 0; i < len(rating); i += 1 {
        for j := i - 1; j >= 0; j -= 1 {
            if rating[j] < rating[i] {
                ret, increase[i] = ret + increase[j], increase[i] + 1
            }
            if rating[j] > rating[i] {
                ret, decrease[i] = ret + decrease[j], decrease[i] + 1
            }
        }
    }
    return ret
}
```
