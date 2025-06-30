# 594. Longest Harmonious Subsequence

## Intuition

A harmonious subsequence is defined as a subsequence where the difference between the maximum and minimum values is exactly 1. To find the longest harmonious subsequence, we need to count the frequency of each number and then find pairs of adjacent numbers (differing by 1) that give us the maximum combined frequency.

## Approach

1. **Count Frequencies**: Use a hash map to count the frequency of each number in the array.

2. **Extract and Sort Unique Numbers**: Create a new array containing all unique numbers from the frequency map and sort it. This allows us to easily find adjacent numbers that differ by 1.

3. **Find Maximum Harmonious Length**: Iterate through the sorted unique numbers and check consecutive pairs. If two adjacent numbers differ by exactly 1, calculate their combined frequency and update the maximum length.

4. **Return Result**: The maximum combined frequency represents the length of the longest harmonious subsequence.

## Complexity

- Time complexity: O(n log k)
- Space complexity: O(k)

## Keywords

- Hash Map
- Frequency Counting

## Code

```go
func findLHS(nums []int) int {
    freq := make(map[int]int)
    for _, num := range nums {
        freq[num] += 1
    }

    nums, i := make([]int, len(freq)), 0
    for k := range freq {
        nums[i], i = k, i + 1
    }
    sort.Ints(nums)

    ret := 0
    for i := 0; i < len(nums) - 1; i += 1 {
        if nums[i + 1] - nums[i] != 1 {
            continue
        }
        ret = max(ret, freq[nums[i]] + freq[nums[i + 1]])
    }
    return ret
}
```
