# 12. Integer to Roman

## Intuition

The key insight is that Roman numerals follow specific patterns and rules. We can break down any integer into its constituent parts using a greedy approach, starting from the largest possible Roman numeral value and working our way down.

## Approach

1. Create a mapping of integer values to their corresponding Roman numeral symbols, including special cases like 4 (IV), 9 (IX), etc.
2. Start from the largest value (1000) and work down to the smallest (1)
3. For each position:
    - If the current value exists in our mapping, use it directly
    - Otherwise, handle it by combining the appropriate symbols:
        - For values ≥ 5, first add the 5-symbol (V, L, D) and then add the 1-symbol (I, X, C) as needed
        - For values < 5, add the 1-symbol repeatedly
4. Continue this process until we've processed all digits

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Hash Map
- String Manipulation
- Number System Conversion

## Code

```go
func intToRoman(num int) string {
    roman := map[int]string{
        1: "I",
        4: "IV",
        5: "V",
        9: "IX",
        10: "X",
        40: "XL",
        50: "L",
        90: "XC",
        100: "C",
        400: "CD",
        500: "D",
        900: "CM",
        1000: "M",
    }

    ten, ret := 1000, ""
    for num > 0 {
        cur := num / ten
        if v, found := roman[cur * ten]; found {
            ret += v
        } else {
            t := roman[ten]
            if cur >= 5 {
                ret, cur = ret + roman[5 * ten], cur - 5
            }
            for ;cur > 0; cur -= 1 {
                ret += t
            }
        }
        num, ten = num % ten, ten / 10
    }
    return ret
}
```
