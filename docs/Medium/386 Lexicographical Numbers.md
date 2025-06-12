# 386. Lexicographical Numbers

## Intuition

The problem requires us to find all numbers from 1 to n in lexicographical order. In lexicographical order, numbers are compared as strings, so "1" comes before "2", "10" comes before "2" because when comparing strings, we compare character by character from left to right. The key insight is that we can simulate this ordering by systematically exploring numbers digit by digit.

## Approach

1. We start with the number 1 and use it as our current number (cnt).
2. For each position in our result array:
    - Add the current number to the result
    - If current number × 10 is still ≤ n, we append 0 to explore the next level (multiply by 10)
    - Otherwise:
        - If we've reached 9 in the last digit or exceeded n, we need to backtrack (divide by 10)
        - Then increment by 1 to try the next number at the same level
3. This process naturally generates numbers in lexicographical order because:
    - We always try to go deeper by appending 0 first
    - When we can't go deeper, we increment the last digit
    - When we can't increment anymore, we backtrack and try the next number

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Lexicographical Order

## Code

```go
func lexicalOrder(n int) []int {
    ret, cnt := make([]int, n), 1

    for i := 0; i < n; i += 1 {
        ret[i] = cnt

        if cnt * 10 <= n {
            cnt *= 10
        } else {
            for cnt % 10 == 9 || cnt >= n {
                cnt /= 10
            }
            cnt += 1
        }
    }

    return ret
}
```
