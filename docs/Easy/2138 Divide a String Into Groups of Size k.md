# 2138. Divide a String Into Groups of Size k

## Intuition

The problem requires dividing a string into groups of size k. If the last group has fewer characters than k, we need to fill it with the given fill character until it reaches size k. This suggests using a loop to process the string in chunks of k characters.

## Approach

1. Create an empty slice to store the resulting strings
2. Iterate through the string with steps of k characters
3. For each iteration:
    - If there are at least k characters remaining, take a substring of k characters
    - If there are fewer than k characters remaining:
        - Take the remaining characters
        - Fill the rest with the given fill character until reaching length k
4. Return the resulting slice of strings

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- String Manipulation
- String Padding

## Code

```go
func divideString(s string, k int, fill byte) []string {
    ret := make([]string, 0)
    for i := 0; i < len(s); i += k {
        if i + k < len(s) {
            ret = append(ret, string(s[i: i + k]))
        } else {
            ret = append(ret, string(s[i:]))
            for j := len(ret[len(ret) - 1]); j < k; j += 1 {
                ret[len(ret) - 1] += string(fill)
            } 
        }
    }
    return ret
}
```
