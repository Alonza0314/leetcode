# 3. Longest Substring Without Repeating Characters

## Intuition

To find the longest substring without repeating characters, we can use the sliding window technique with two pointers (beginning and end) and a character frequency map. This allows us to efficiently track unique characters within our current window.

## Approach

1. Use a sliding window with two pointers: `beg` (start of window) and `end` (end of window)
2. Maintain a frequency map (using an array of size 256 for ASCII characters) to track character occurrences
3. For each character at the `end` pointer:
    - If the character hasn't been seen (frequency is 0), add it to our window and update max length
    - If the character has been seen, shrink the window from the beginning until we remove the duplicate
4. Continue this process until we reach the end of the string

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Sliding Window
- Two Pointers

## Code

```go
func Max(a int, b int) int {
    if a > b {
        return a
    }
    return b
}

func lengthOfLongestSubstring(s string) int {
    mmap := [256]int{}
    beg, end, max := 0, 0, 0
    for end < len(s) {
        if mmap[s[end]] == 0 {
            mmap[s[end]] = 1
            end += 1
            max = Max(max, end-beg)
        } else {
            mmap[s[beg]] = 0
            beg += 1
        }
    }
    return max
}
```
