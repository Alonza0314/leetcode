# 17. Letter Combinations of a Phone Number

## Intuition

The problem requires us to generate all possible letter combinations that could be represented by a given sequence of digits on a phone keypad. Each digit (2-9) maps to a set of letters, and we need to find all possible combinations by selecting one letter from each digit's corresponding set.

## Approach

1. Create a hash map to store the mapping between digits and their corresponding letters
2. Use a recursive approach to build combinations:
    - For each digit in the input string, try all possible letters mapped to that digit
    - When we reach the last digit, add the complete combination to the result
    - Use a byte slice to build combinations efficiently
    - Pass a pointer to the result slice to avoid copying large slices

## Complexity

- Time complexity: O(4^N * N)
- Space complexity: O(N)

## Keywords

- Recursion
- Backtracking
- Hash Map

## Code

```go
func recursive(result []byte, idx int, ret *[]string, hash map[byte][]byte, digits string) {
    if idx == len(result) - 1 {
        for _, alph := range hash[digits[idx]] {
            result[idx] = alph
            *ret = append(*ret, string(result))
        }
        return
    }
    for _, alph := range hash[digits[idx]] {
        result[idx] = alph
        recursive(result, idx + 1, ret, hash, digits)
    }
}

func letterCombinations(digits string) []string {
    ret := make([]string, 0)
    if digits == "" {
        return ret
    }
    hash := map[byte][]byte{
        '2': {'a', 'b', 'c'},
        '3': {'d', 'e', 'f'},
        '4': {'g', 'h', 'i'},
        '5': {'j', 'k', 'l'},
        '6': {'m', 'n', 'o'},
        '7': {'p', 'q', 'r', 's'},
        '8': {'t', 'u', 'v'},
        '9': {'w', 'x', 'y', 'z'},
    }
    result := make([]byte, len([]byte(digits)))
    recursive(result, 0, &ret, hash, digits)
    return ret
}
```
