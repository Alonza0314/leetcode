# 140. Work Break II

## Intuition

We need to explore all possible combinations of words that can form the input string. This naturally leads to a recursive approach where we try different word combinations at each step.

## Approach

1. First, we create a map of valid words from the dictionary for O(1) lookup.
2. We use a recursive function that:
    - Builds the current word character by character
    - When a valid word is found, we have two choices:
        1. Add the word to our current path and start a new word
        2. Continue building the current word
3. When we reach the end of the string with a valid word, we join all words in the current path with spaces and add it to our result.
4. The recursion explores all possible combinations of words that can form the input string.

## Complexity

- Time complexity: O(2^n)
- Space complexity: O(n)

## Keywords

- Recursion
- Backtracking
- DFS

## Code

```go
func wordBreak(s string, wordDict []string) []string {
    ret, wordMap := make([]string, 0), make(map[string]bool)
    for _, word := range wordDict {
        wordMap[word] = true
    }

    var recur func(curI int, curWord string, curStr []string)
    recur = func(curI int, curWord string, curStr []string) {
        if curI == len(s) {
            return
        }
        curWord += string(s[curI])
        if wordMap[curWord] {
            tmp := make([]string, len(curStr))
            copy(tmp, curStr)
            tmp = append(tmp, curWord)
            if curI == len(s) - 1 {
                retStr := strings.Join(tmp, " ")
                ret = append(ret, retStr)    
            } else {
                recur(curI + 1, "", tmp)
            }
        }
        recur(curI + 1, curWord, curStr)
    }

    recur(0, "", make([]string, 0))
    return ret
}
```
