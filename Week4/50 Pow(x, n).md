# 50. Pow(x, n)

## Intuition

A naive approach would be to multiply x by itself n times, but this would be inefficient for large values of n. Instead, we can use the property that x ^ n = [x ^ (n/2)] ^ 2 when n is even, and x ^ n = x * [x ^ (n/2)] ^ 2 when n is odd.

## Approach

1. Handle the base case: If n is 0, return 1.
2. Create a memoization map to store computed powers and avoid redundant calculations.
3. Implement two recursive functions:
   - `recurPositive` for positive exponents
   - `recurNegative` for negative exponents
4. For both functions:
   - Check if the result for the current exponent is already in the map
   - For even exponents, calculate the result for n/2 and square it
   - For odd exponents, do the same but multiply by x (or divide by x for negative exponents)
5. Handle negative exponents by using the relation x^(-n) = 1/(x ^ n)

## Complexity

- Time complexity: O(log n)
- Space complexity: O(log n)

## Keywords

- Binary Exponentiation
- Divide and Conquer
- Recursion
- Fast Power Algorithm

## Code

```go
func myPow(x float64, n int) float64 {
    if n == 0 {
        return 1
    }
    record := make(map[int]float64)

    var recurPositive func(curN int) float64
    var recurNegative func(curN int) float64
    recurPositive = func(curN int) float64 {
        if v, found := record[curN]; found {
            return v
        }

        flag := false
        if curN & 1 == 1 {
            flag = true
            curN -= 1
        }
        tmp := recurPositive(curN / 2)
        record[curN] = tmp * tmp
        if flag {
            record[curN + 1] = record[curN] * x
            return record[curN + 1]
        }
        return record[curN]
    }
    recurNegative = func(curN int) float64 {
        if v, found := record[curN]; found {
            return v
        }

        flag := false
        if curN & 1 == 1 {
            flag = true
            curN += 1
        }
        tmp := recurNegative(curN / 2)
        record[curN] = tmp * tmp
        if flag {
            record[curN - 1] = record[curN] / x
            return record[curN - 1]
        }
        return record[curN]
    }
    if n < 0 {
        record[-1] = 1/x
        return recurNegative(n)
    }
    record[1] = x
    return recurPositive(n)
}
```
