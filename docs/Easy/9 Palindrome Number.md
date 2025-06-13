# 9. Palindrome Number

## Intuition

The intuition is to convert the number to a string and then check if it reads the same forwards and backwards. This is a straightforward approach that makes it easy to compare characters from both ends of the number.

## Approach

1. First, we handle the edge case where negative numbers cannot be palindromes
2. Convert the number to a string using `strconv.Itoa()`
3. Convert the string to a rune slice to handle any potential Unicode characters
4. Use two pointers (i and j) starting from both ends of the string
5. Compare characters at both pointers and move them towards the center
6. If any characters don't match, return false
7. If all characters match, return true

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- Two Pointers
- String Conversion
- Palindrome

## Code

```go
func isPalindrome(x int) bool {
    if x < 0 {
        return false
    }
    xx := strconv.Itoa(x)
    str := []rune(xx)

    for i, j := 0, len(str) - 1; i < j; i, j = i + 1, j - 1 {
        if str[i] != str[j] {
            return false
        }
    }
    return true
}
```
