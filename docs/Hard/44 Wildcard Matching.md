# 44. Wildcard Matching

## Intuition

This problem can be solved using a greedy approach with two pointers. The key insight is to handle the '*' character differently from other characters. When we encounter a '*', we keep track of its position and the position in the string where we matched it, so we can try different matching lengths for the '*' if needed.

## Approach

1. Use two pointers `ss` and `pp` to track positions in string `s` and pattern `p` respectively
2. Keep track of the latest '*' position (`latestStar`) and the position in string where we started matching this '*' (`latestStarMatch`)
3. For each character in string s:
    - If current characters match (same char or '?'), move both pointers forward
    - If we see a '*', record its position and current string position, then move pattern pointer
    - If characters don't match but we have a previous '*', backtrack to try matching one more character with that '*'
    - If none of above works, return false
4. After processing string s, check if remaining pattern only contains '*'

## Complexity

- Time complexity: O(S*P)
- Space complexity: O(1)

## Keywords

- Two Pointers
- Greedy Algorithm
- Pattern Matching

## Code

```go
func isMatch(s string, p string) bool {
    ss, pp, latestStar, latestStarMatch := 0, 0, -1, 0

    for ss < len(s) {
        if pp < len(p) && (p[pp] == '?' || p[pp] == s[ss]) {
            ss, pp = ss+1, pp+1
        } else if pp < len(p) && p[pp] == '*' {
            latestStar, latestStarMatch, pp = pp, ss, pp+1
        } else if latestStar != -1 {
            pp, latestStarMatch, ss = latestStar+1, latestStarMatch+1, latestStarMatch+1
        } else {
            return false
        }
    }

    for pp < len(p) && p[pp] == '*' {
        pp += 1
    }
    return pp == len(p)
}
```
