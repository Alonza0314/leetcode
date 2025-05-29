# 2894. Divisible and Non-divisible Sums Difference

## Intuition

The problem requires us to find the difference between two sums:
1. Sum of numbers from 1 to n that are NOT divisible by m
2. Sum of numbers from 1 to n that are divisible by m

We can solve this by iterating through numbers 1 to n and checking if each number is divisible by m using the modulo operator.

## Approach

1. Initialize two variables:
   - `nd` for sum of non-divisible numbers
   - `d` for sum of divisible numbers
2. Iterate through numbers from 1 to n
3. For each number i:
   - If i is divisible by m (i % m == 0), add it to d
   - If i is not divisible by m, add it to nd
4. Return the difference (nd - d)

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Math
- Modulo Operation

## Code

```go
func differenceOfSums(n int, m int) int {
    nd, d := 0, 0
    for i := 1; i <= n; i += 1 {
        if i % m == 0 {
            d += i
        } else {
            nd += i
        }
    }
    return nd - d
}
```
