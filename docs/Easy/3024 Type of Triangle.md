# 3024. Type of Triangle

## Intuition

To determine the type of triangle, we need to:

1. Check if the three sides can form a valid triangle
2. Count the number of equal sides to determine the triangle type
3. Return the appropriate triangle type based on the conditions

## Approach

1. First define constant strings for different triangle types:

    - equilateral: all sides equal
    - isosceles: two sides equal
    - scalene: no sides equal
    - none: not a valid triangle

2. Create a helper function `checkTriangle` to verify if three sides can form a valid triangle using the triangle inequality theorem:

    - sum of any two sides must be greater than the third side
    - check all three combinations: a+b>c, b+c>a, c+a>b

3. Use a map to count the frequency of each side length:

    - If any number appears 3 times, it's an equilateral triangle
    - If any number appears 2 times, it's potentially an isosceles triangle
    - Otherwise, it's potentially a scalene triangle

4. For isosceles and scalene cases, verify if they can form a valid triangle using `checkTriangle`

## Complexity

- Time complexity: O(1)
- Space complexity: O(1)

## Keywords

- Triangle Inequality Theorem
- Hash Map

## Code

```go
func triangleType(nums []int) string {
    var (
        equ string = "equilateral"
        iso string = "isosceles"
        sca string = "scalene"
        non string = "none"
    )

    checkTriangle := func() bool {
        a, b, c := nums[0], nums[1], nums[2]
        if (a + b > c) && (b + c > a) && (c + a > b) {
            return true
        }
        return false
    }

    record := make(map[int]int)
    for _, num := range nums {
        record[num] += 1
        if record[num] == 3 {
            return equ
        }
    }

    for _, v := range record {
        if v == 2 {
            if checkTriangle() {
                return iso
            }
            return non
        }
    }

    if checkTriangle() {
        return sca
    }

    return non
}
```
