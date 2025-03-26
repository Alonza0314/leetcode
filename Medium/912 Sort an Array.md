# 912. Sort an Array

## Intuition
We can use a counting sort approach with bucket sort, which is efficient for this specific range of numbers.

## Approach

1. Create a bucket array of size 100001 (to accommodate numbers from -50000 to 50000)
2. Count the frequency of each number by adding 50000 to the index (to handle negative numbers)
3. Reconstruct the sorted array by iterating through the bucket array and placing numbers back in their original positions

## Complexity

- Time complexity: O(n + k)
- Space complexity: O(k)

## Keywords

- Sorting
- Counting Sort
- Bucket Sort

## Code

```go
func sortArray(nums []int) []int {
    bucket := make([]int, 100001)
    for _, num := range nums {
        bucket[num + 50000] += 1
    }
    idx := 0
    for i, cnt := range bucket {
        for cnt != 0 {
            nums[idx] = i - 50000
            idx, cnt = idx + 1, cnt - 1
        }
    }
    return nums
}
```
