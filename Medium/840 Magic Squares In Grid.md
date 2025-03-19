# 840 Magic Squares In Grid

## Intuition

As we konw that the magicSquares is limited in 3x3, we can prepare these all magicSquares in advanced (only 8 types).
Then we can just slide the 3x3 window through the input grid and get the the answer.

## Approach

First, prepare the magicSquares.
Second, check the size of the grid if it is larger than 3x3; otherwise, it does not need to continue.
Last, slide the 3x3 window through the whole inpput grid.

## Complexity

- Time complexity: O(n^2)
- Space complexity: O(n^2)

## Keywords

- Sliding Window
- Matrix

## Code

```go
func numMagicSquaresInside(grid [][]int) int {
    magicSquares := [][][]int{
        {
            {8, 1, 6},
            {3, 5, 7},
            {4, 9, 2},
        },
        {
            {4, 3, 8},
            {9, 5, 1},
            {2, 7, 6},
        },
        {
            {2, 9, 4},
            {7, 5, 3},
            {6, 1, 8},
        },
        {
            {6, 7, 2},
            {1, 5, 9},
            {8, 3, 4},
        },
        {
            {4, 9, 2},
            {3, 5, 7},
            {8, 1, 6},
        },
        {
            {2, 7, 6},
            {9, 5, 1},
            {4, 3, 8},
        },
        {
            {6, 1, 8},
            {7, 5, 3},
            {2, 9, 4},
        },
        {
            {8, 3, 4},
            {1, 5, 9},
            {6, 7, 2},
        },
    }
    ret, r, c := 0, len(grid), len(grid[0])
    if r < 3 || c < 3 {
        return 0
    }
    var check func(x, y int) int
    var checkRow func(a, b []int) bool
    checkRow = func(a, b []int) bool {
        for i := range a {
            if a[i] != b[i] {
                return false
            }
        }
        return true
    }
    check = func(x, y int) int {
        matrix := make([][]int, 3)
        for i := range matrix {
            matrix[i] = grid[x + i][y: y + 3]
        }
        for _, m := range magicSquares {
            if checkRow(m[0], matrix[0]) && checkRow(m[1], matrix[1]) && checkRow(m[2], matrix[2]) {
                return 1
            }
        }
        return 0
    }
    for i := 0; i < r - 2; i += 1 {
        for j := 0; j < c - 2; j += 1 {
            ret += check(i, j)
        }
    }
    return ret
}
```
