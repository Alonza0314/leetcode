# 10. Regular Expression Matching

## Intuition

The problem requires us to implement a regular expression matcher that supports '.' and '\*' patterns. The key insight is that we can use dynamic programming to solve this problem efficiently. We need to consider all possible matching scenarios, especially when dealing with the '*' pattern which can match zero or more of the preceding element.

## Approach

1. Create a 2D DP array where `dp[i][j]` represents whether the substring `s[i:]` matches the pattern `p[j:]`
2. Initialize the base case: `dp[n][m]` = true (empty string matches empty pattern)
3. Process the string and pattern from right to left
4. For each position, consider two cases:
    - If the next character is '*', we can either:
        - Skip the current pattern character and '*' (`dp[i][j+2]`)
        - Match the current character if it matches and continue with the same pattern (`dp[i+1][j]`)
    - If the next character is not '*', simply check if current characters match and move forward

## Complexity

- Time complexity: O(m*n)
- Space complexity: O(m*n)

## Keywords

- Dynamic Programming
- String Matching
- Regular Expression
- Pattern Matching

## Code

```go
func isMatch(s string, p string) bool {
    n, m := len(s), len(p)
    dp := make([][]bool, n + 1)
    for i := range dp {
        dp[i] = make([]bool, m + 1)
    }
    dp[n][m] = true

    for i := n; i >= 0; i -= 1 {
        for j := m - 1; j >= 0; j -= 1 {
            currentMatch := i < n && (s[i] == p[j] || p[j] == '.')

            if j + 1 < m && p[j + 1] == '*' {
                dp[i][j] = dp[i][j + 2] || (currentMatch && dp[i + 1][j])
            } else {
                dp[i][j] = currentMatch && dp[i + 1][j + 1]
            }
        }
    }

    return dp[0][0]
}
```
