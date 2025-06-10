# 3727. Find the Count of Good Integers

## Intuition

1. For a palindromic number, we only need to determine the first half of digits, as the second half is a mirror
2. We need to handle odd and even length numbers differently
3. We need to avoid counting duplicates when different digit combinations result in the same frequency pattern
4. Leading zeros should be excluded from the count

## Approach

1. Generate palindromic numbers by:
    - Only generating the first half (up to breakPoint)
    - Mirroring the digits for the second half
    - For odd length numbers, the middle digit is counted once
2. Use a frequency array to track digit counts and avoid duplicates:
    - Store the frequency of each digit (0-9) in a [10]int array
    - Use this as a key in a map to avoid counting duplicate patterns
3. Calculate permutations for each valid pattern:
    - Use factorial to compute total possible arrangements
    - Account for repeated digits by dividing by their factorials
    - Handle leading zeros by subtracting invalid cases
4. Recursively generate all possible first halves and check:
    - If the resulting palindrome is divisible by k
    - If we've seen this digit frequency pattern before

## Complexity

- Time complexity: O(10^(n/2) * n)
- Space complexity: O(n)

## Keywords

- Palindrome
- Combinatorics
- Recursion
- Permutation
- Factorial

## Code

```go
func countGoodIntegers(n int, k int) int64 {
    cnt := int64(0)
    number, record, breakPoint, fac := make([]rune, n), make(map[[10]int]bool), 0, make(map[int]int)
    switch n & 1 == 1 {
    case true:
        breakPoint = n / 2
    case false:
        breakPoint = n / 2 - 1
    }
    fac[0] = 1
    for i := 1; i <= n; i += 1 {
        fac[i] = fac[i - 1] * i
    }

    getPalindromic := func(cur []rune) [10]int {
        ret := [10]int{}
        for i := 0; i <= breakPoint; i += 1 {
            cur[len(cur) - 1 - i] = cur[i]
            ret[cur[i] - '0'] += 2
        }
        if n & 1 == 1 {
            ret[cur[breakPoint] - '0'] -= 1
        }
        return ret
    }

    countPermutations := func(rec [10]int) int {
        total := fac[n]
        for _, ii := range rec {
            total /= fac[ii]
        }
        if rec[0] != 0 {
            total -= ((total * fac[rec[0]] / n) / fac[rec[0] - 1])
        }
        return total
    }
    
    var recur func(cur []rune, i int, nextN rune)
    recur = func(cur []rune, i int, nextN rune) {
        if i > breakPoint {
            return
        }
        cur[i] = nextN
        if i == breakPoint {
            rec := getPalindromic(cur)
            if _, exist := record[rec]; exist {
                return
            }
            palindromicNumber, _ := strconv.Atoi(string(cur))
            if palindromicNumber % k != 0 {
                return
            }
            cnt, record[rec] = cnt + int64(countPermutations(rec)), true
            return
        }
        for j := 0; j <= 9; j += 1 {
            recur(number, i + 1, rune('0' + j))
        }
    }

    for i := 1; i <= 9; i += 1 {
        recur(number, 0, rune('0' + i))
    }
    return cnt
}
```
