# 8. String to Integer (atoi)

## Intuition

The problem requires us to convert a string to an integer, similar to the C/C++ atoi function. We need to handle various edge cases including:

- Leading whitespaces
- Optional plus/minus sign
- Numerical digits
- Overflow conditions
- Invalid characters

## Approach

1. Skip all leading whitespaces
2. Check for optional plus or minus sign
3. Process numerical digits:
    - Convert each digit to integer
    - Build the number by multiplying current result by 10 and adding new digit
    - Handle overflow by checking against INT_MAX/INT_MIN
4. Return the final result with appropriate sign

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- String Parsing
- Integer Overflow
- Edge Cases

## Code

```go
func myAtoi(s string) int {
    ret, sign, i := 0, true, 0
    for ; i < len(s) && s[i] == ' '; i += 1 {}
    if i < len(s) {
        if s[i] == '-' {
            sign, i = false, i + 1
        } else if s[i] == '+' {
            i += 1
        }
    }
    for ; i < len(s); i += 1 {
        c := s[i]
        switch c {
        case '0', '1', '2', '3', '4', '5', '6', '7', '8', '9':
            ret = ret * 10 + int(c - '0')
            if ret > int(^uint32(0) >> 1) {
                if !sign {
                    return -1 * int(^uint32(0) >> 1) - 1
                }
                return int(^uint32(0) >> 1)
            }
        default:
            goto RETURN
        }
    }
RETURN:
    if !sign {
        ret = -ret
    }
    return int(ret)
}
```
