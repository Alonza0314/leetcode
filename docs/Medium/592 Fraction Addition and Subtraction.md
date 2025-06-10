# 592 Fraction Addition and Subtraction

## Intuition

Just put the fractions into a map and only extract the non-zero value to do "Add", and divide the numerator and the denominator with their GCD.

## Approach

Before we preform the math operation, we need to do the string manipulation on the input. Using a map to store each fraction with key and value are denominator and numerator, respectively. In this step we can first sum the fractions which have same demonitor.

Then, we will extract the non-zero fraction from the map, i.e., the value(numerator) is not zero and store the key(denominator) to a list.

Go check if there is no any number in the list and return "0/1".

Go check if there is only one number in the list and return this fraction with diving their GCD, since there does not need any "Add" operation.

Otherwise, walk through the list and extract each fraction to perform addition with common denominators on each fraction in the list. After each addition, do a GCD division on this new "fraction".
The top and down variable is refered as the fianl retuen fraction's numerator and denominator.

## Complexity

- Time complexity: O(N)
- Space complexity: O(N)

## Keywords

- String
- GCD
- Math

## Code

```go
func fractionAddition(expression string) string {
    fraction, idx := make(map[int]int), 0
    for idx < len(expression) {
        top, down := "", ""
        for expression[idx] != '/' {
            top += string(expression[idx])
            idx += 1
        }
        idx += 1
        for idx < len(expression) && expression[idx] != '+' && expression[idx] != '-' {
            down += string(expression[idx])
            idx += 1
        }
        t, _ := strconv.Atoi(top)
        d, _ := strconv.Atoi(down)
        fraction[d] += t
    }
    downs := make([]int, 0)
    for k, v := range fraction {
        if v == 0 {
            continue
        }
        downs = append(downs, k)
    }
    if len(downs) == 0 {
        return "0/1"
    }
    gcd := func(a, b int) int {
        for b > 0 {
            t := a % b
            a, b = b, t
        }
        return a
    }
    if len(downs) == 1 {
        g := gcd(int(math.Abs(float64(fraction[downs[0]]))), downs[0])
        return fmt.Sprintf("%v/%v", fraction[downs[0]] / g, downs[0] / g)
    }
    top, down := 0, 1
    for _, val := range downs {
        top, down = (top * val + fraction[val] * down), down * val
        g := gcd(int(math.Abs(float64(top))), down)
        top, down = top / g, down / g
    }
    return fmt.Sprintf("%v/%v", top, down)
}
```
