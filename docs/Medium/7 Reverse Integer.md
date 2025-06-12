# 7. Reverse Integer

## Intuition

The key idea is to extract each digit from the input number from right to left and build the reversed number. We need to be careful about integer overflow since the problem requires handling 32-bit integer constraints.

## Approach

1. Initialize a variable `ans` to store the reversed number
2. While the input number `x` is not zero:
    - Get the last digit using modulo operation (`x % 10`)
    - Remove the last digit from x by integer division (`x /= 10`)
    - Build the reversed number by multiplying current ans by 10 and adding the new digit
    - Check for 32-bit integer overflow
3. Return the reversed number if no overflow occurred

## Complexity

- Time complexity: O(log(x))
- Space complexity: O(1)

## Keywords

- Math
- Integer Overflow
- Modulo Operation
- 32-bit Integer Constraints

## Code

```go
func reverse(x int) int {
    ans := 0
    for x != 0 {
        num := x%10
        x /= 10
        ans = ans*10 + num

        if ans < -1*int(^uint32(0)>>1) || ans > int(^uint32(0)>>1) {
            return 0
        }
    }
    return ans
}
```
