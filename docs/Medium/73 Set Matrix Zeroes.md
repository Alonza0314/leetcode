# 73. Set Matrix Zeroes

## Intuition

The problem requires us to set entire rows and columns to zero when we encounter a zero element in the matrix. A straightforward approach would be to first identify all rows and columns that need to be zeroed out, and then apply these changes to the matrix.

## Approach

1. Use two hash maps to track which rows and columns need to be zeroed out
2. First pass: iterate through the matrix to mark rows and columns containing zeros
3. Second pass:
    - For marked rows, set the entire row to zero
    - For other rows, only set the marked columns to zero
4. Use a pre-created zero array for efficient row zeroing

## Complexity

- Time complexity: O(m*n)
- Space complexity: O(m+n)

## Keywords

- Matrix
- Hash Map

## Code

```go
func setZeroes(matrix [][]int)  {
    rmp, cmp := make(map[int]bool), make(map[int]bool)
    for i, line := range matrix {
        for j, item := range line {
            if item == 0 {
                rmp[i], cmp[j] = true, true
            }
        }
    }
    zero := make([]int, len(matrix[0]))
    for i, line := range matrix {
        if rmp[i] {
            copy(matrix[i], zero)
            continue
        }
        for j := range line {
            if cmp[j] {
                matrix[i][j] = 0
            }
        }
    }
}
```
