# 5. Longest Palindromic Substring

## Intuition

The key insight is to use dynamic programming to solve this problem. For each substring, we can determine if it's a palindrome based on:

1. The characters at both ends must be equal
2. The substring between these characters must also be a palindrome

## Approach

1. Create a 2D DP table where `dp[i][j]` represents whether the substring from index i to j is a palindrome
2. Initialize single characters as palindromes (`dp[i][i] = true`)
3. For each possible substring length:
    - Check if characters at both ends are equal
    - If they are equal, check if the inner substring is also a palindrome
    - Keep track of the longest palindrome found so far
4. Return the longest palindromic substring found

## Complexity

- Time complexity: O(n²)
- Space complexity: O(n²)

## Keywords

- Dynamic Programming
- String
- Palindrome
- Two-dimensional DP

## Code

```go
func longestPalindrome(s string) string {
    dp := make([][]bool, len(s))
    for i := range dp {
        dp[i] = make([]bool, len(s))
        dp[i][i] = true
    }
    maxLength, l, r := 1, 0, 0
    for i := len(s) - 1; i >= 0; i -= 1 {
        for j := i; j < len(s); j += 1 {
            if s[i] == s[j] {
                if i + 1 <= j - 1 {
                    if dp[i + 1][j - 1] {
                        dp[i][j] = true
                    }
                } else {
                    dp[i][j] = true
                }
            }

            if dp[i][j] && j - i + 1 > maxLength {
                maxLength, l, r = j - i + 1, i, j
            }
        }
    }
    return string(s[l: r + 1])
}
```
