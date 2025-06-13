# 13. Roman to Integer

## Intuition

The key insight is that Roman numerals are typically written from largest to smallest value, but there are special cases where a smaller value precedes a larger value (like IV for 4). In these cases, we need to subtract the smaller value from the larger one.

## Approach

1. Create a mapping of Roman numerals to their integer values
2. Start from the end of the string and work backwards
3. For each character:
    - If the current value is less than the next value, subtract it from the result
    - Otherwise, add it to the result
4. Return the final result

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Hash Map
- String
- Math

## Code

```go
func romanToInt(s string) int {
    mp := map[byte]int {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000,
    }
    ret := mp[s[len(s) - 1]]
    for i := len(s) - 2; i >= 0; i-- {
        if mp[s[i]] < mp[s[i + 1]] {
            ret -= mp[s[i]]
        } else {
            ret += mp[s[i]]
        }
    }
    return ret
}
```
