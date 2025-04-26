# 2444. Count Subarrays With Fixed Bounds

## Intuition

For a subarray to be valid, it must:
1. Contain both minK and maxK
2. Have all elements between minK and maxK (inclusive)
3. Start after any "bad" number (numbers outside the range [minK, maxK])

## Approach

We use a sliding window technique with three pointers:
1. `minIdx`: Position of the last occurrence of minK
2. `maxIdx`: Position of the last occurrence of maxK
3. `badIdx`: Position of the last number that's outside our bounds

For each position i:
1. Update `badIdx` if current number is out of bounds
2. Update `minIdx` if we see minK
3. Update `maxIdx` if we see maxK
4. If both minK and maxK appear after badIdx, we can form valid subarrays
   - The number of valid subarrays ending at i is: min(minIdx, maxIdx) - badIdx

## Complexity
- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Sliding Window
- Two Pointers
- Subarray Count

## Code

```go
func countSubarrays(nums []int, minK int, maxK int) int64 {
    ret, minIdx, maxIdx, badIdx := int64(0), -1, -1, -1

    for i, num := range nums {
        if num < minK || num > maxK {
            badIdx = i
        }
        if num == minK {
            minIdx = i
        }
        if num == maxK {
            maxIdx = i
        }
        idx := min(minIdx, maxIdx)
        if idx > badIdx {
            ret += int64(idx - badIdx)
        }
    }

    return ret
}
```
