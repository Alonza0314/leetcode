# 14. Longest Common Prefix

## Intuition

The longest common prefix of all strings must be a prefix of the shortest string in the array. We can compare characters at each position across all strings until we find a mismatch or reach the end of the shortest string.

## Approach

1. First, find the length of the shortest string in the array using a helper function `Min`
2. Iterate through each character position up to the length of the shortest string
3. For each position, compare the character with the corresponding character in the first string
4. If any string has a different character at that position, return the prefix up to that position
5. If we complete the iteration without finding any mismatches, return the entire shortest string as the common prefix

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- String
- Prefix

## Code

```go
func Min(strs []string) int {
    ret := math.MaxInt
    for _, str := range strs {
        if len(str) < ret {
            ret = len(str)
        }
    }
    return ret
}
func longestCommonPrefix(strs []string) string {
    min := Min(strs)
    for i := 0; i < min ; i++ {
        flag := strs[0][i]
        for _, str := range strs {
            if str[i] != flag {
                return string(strs[0][:i])
            }
        }
    }
    return string(strs[0][:min])
}
```
