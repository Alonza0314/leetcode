# 38. Count and Say

## Intuition

Brute force.

## Approach

1. Start with initial string "1"
2. For each iteration (n-1 times):
   - Scan through the current string
   - Count consecutive same characters
   - Build the next string by combining the count and the character
3. Use a closure function to handle the counting and saying process
4. Handle edge cases where character changes by resetting counter

## Complexity

- Time complexity: O(N * M)
- Space complexity: O(M)

## Keywords

- String Manipulation

## Code

```go
func countAndSay(n int) string {
    init := "1"
    
    cntAndSay := func() {
        current, currentCnt, ret := '?', 0, ""
        for _, char := range init {
            if char != current {
                ret += strconv.Itoa(currentCnt) + string(current)
                current, currentCnt = char, 1
            } else {
                currentCnt += 1
            }

        }
        ret += strconv.Itoa(currentCnt) + string(current)
        init = string([]byte(ret[2:]))
    }

    for i := 1; i < n; i += 1 {
        cntAndSay()
    }

    return init
}
```
