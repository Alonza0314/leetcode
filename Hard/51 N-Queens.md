# 51. N-Queens

## Intuition

The key insight is that queens can attack horizontally, vertically, and diagonally. We need to ensure that no two queens share the same row, column, or diagonal.

## Approach

1. Use backtracking to explore all possible queen placements
2. Maintain three sets to track occupied rows, columns, and diagonals
3. For each row, try placing a queen in each column if it's safe
4. If a valid placement is found, recursively try the next row
5. If we reach the last row successfully, add the current board configuration to the result
6. Backtrack by removing the last placed queen and try the next position

## Complexity

- Time complexity: O(N!)
- Space complexity: O(N)

## Keywords

- Backtracking
- Recursion
- N-Queens
- DFS

## Code

```go
func solveNQueens(n int) [][]string {
    ret, board, queens := make([][]string, 0), make([]string, n), make([][2]int, 0)
    row := ""
    for i := 0; i < n; i += 1 {
        row += "."
    }
    for i := range board {
        board[i] = row
    }
    rowMap, colMap := make(map[int]bool, n), make(map[int]bool, n)
    var checkq func(i, j int) bool
    var replaceQ func(i, j int)
    var revertQ func(i, j int)
    var recursion func(level int)
    checkq = func(i, j int) bool {
        for _, queen := range queens {
            if math.Abs(float64(queen[0] - i) / float64(queen[1] - j)) == 1 {
                return false
            }
        }
        return true
    }
    replaceQ = func(i, j int) {
        tmp := []byte(board[i])
        tmp[j] = 'Q'
        board[i] = string(tmp)
    }
    revertQ = func(i, j int) {
        tmp := []byte(board[i])
        tmp[j] = '.'
        board[i] = string(tmp)
    }
    recursion = func(level int) {
        for i := 0; i < n; i += 1 {
            if !rowMap[level] && !colMap[i] && checkq(level, i) {
                replaceQ(level, i)
                rowMap[level], colMap[i] = true, true
                queens = append(queens, [2]int{level, i})
                if level == n - 1 {
                    ret = append(ret, append([]string{}, board...))
                } else {
                    recursion(level + 1)
                }
                revertQ(level, i)
                rowMap[level], colMap[i] = false, false
                queens = queens[: len(queens) - 1]
            }
        }
    }
    recursion(0)
    return ret
}
```
