# 3442. Maximum Difference Between Even and Odd Frequency I

## Intuition

The problem requires us to find the maximum difference between odd and even frequencies of characters in a string. We can solve this by counting the frequency of each character and then finding the maximum odd frequency and minimum even frequency.

## Approach

1. Create a frequency array `record` of size 26 to store the count of each lowercase letter
2. Count the frequency of each character in the string
3. Iterate through the frequency array:
   - Skip if frequency is 0
   - For odd frequencies, update the maximum odd frequency
   - For even frequencies, update the minimum even frequency
4. Return the difference between maximum odd frequency and minimum even frequency

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Frequency Count
- Array
- Maximum Difference

## Code

```go
func maxDifference(s string) int {
    record := make([]int, 26)

    maxRecord, minRecord := math.MinInt, math.MaxInt
    for _, c := range s {
        record[c - 'a'] += 1
    }

    for _, v := range record {
        if v == 0 {
            continue
        }
        if v & 1 == 1 {
            maxRecord = max(maxRecord, v)
        } else {
            minRecord = min(minRecord, v)
        }
    }


    return maxRecord - minRecord
}
```
