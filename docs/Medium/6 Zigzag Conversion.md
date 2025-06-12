# 6. Zigzag Conversion

## Intuition

The key insight is to understand that the zigzag pattern follows a predictable sequence. We can simulate this pattern by using a flag to control the direction (up or down) and maintaining separate arrays for each row. The characters are distributed row by row, changing direction when we hit the top or bottom row.

## Approach

1. Create a 2D array where each inner array represents a row in the zigzag pattern
2. Use a flag to track the direction of movement (true for moving down, false for moving up)
3. Iterate through each character in the input string:
    - Add the character to the current row
    - Update the row index based on the direction
    - Change direction when reaching the top or bottom row
4. Finally, concatenate all rows to form the result string

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- String Manipulation
- Pattern Recognition
- Two-dimensional Array

## Code

```go
func convert(s string, numRows int) string {
    arr := make([][]rune, numRows)
    for i := 0; i < numRows; i++ {
        arr[i] = make([]rune, 0)
    }
    flag := true
    idx := 0
    for _, char := range s {
        arr[idx] = append(arr[idx], char)
        if flag {
            if idx == numRows - 1 {
                flag = false
                idx -= 1
                if idx == -1 {
                    idx = 0
                }
            } else {
                idx += 1
            }
        } else {
            if idx == 0 {
                flag = true
                idx += 1
                if idx == numRows {
                    idx = numRows - 1
                }
            } else {
                idx -= 1
            }
        }
    }
    ret := make([]rune, len(s))
    idx = 0
    for _, substring := range arr {
        copy(ret[idx: idx + len(substring)], substring)
        idx += len(substring)
    }
    return string(ret)
}
```
