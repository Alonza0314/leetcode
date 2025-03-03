# 765. Couples Holding Hands

## Intuition

Each time we need to find the correct "couple" pair for `2n` and `2n+1` seats, i = 1, 2, 3, ... .

## Approach

The solution uses a greedy algorithm with the following steps:

1. First, create a hash map `idToIndex` to record each person's seat index for efficient lookup and swapping operations.
2. Examine adjacent seats from left to right (step size of 2):

    - Get person `a` in the first seat and person `b` in the second seat
    - Calculate who should be a's partner (expectB):
        - If a is even, their partner is a+1
        - If a is odd, their partner is a-1

3. If b is not a's partner:

    - Find the position of a's partner (expectB)
    - Swap b with expectB
    - Update the indices in the hash map
    - Increment the swap counter

4. Return the total number of swaps

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- Greedy Algorithm
- Hash

## Code

```go
func minSwapsCouples(row []int) int {
    ret, idToIndex := 0, make(map[int]int)
    for i, id := range row {
        idToIndex[id] = i
    }
    for i := 0; i < len(row); i += 2 {
        a, b, expectB := row[i], row[i + 1], 0
        if a & 1 == 0 {
            expectB = a + 1
        } else {
            expectB = a - 1
        }
        if b == expectB {
            continue
        }
        indexB, indexExpectB := idToIndex[b], idToIndex[expectB]
        row[indexB], row[indexExpectB], idToIndex[b], idToIndex[expectB] = row[indexExpectB], row[indexB], indexExpectB, indexB
        ret += 1
    }
    return ret
}
```
