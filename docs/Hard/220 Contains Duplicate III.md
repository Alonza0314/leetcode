# 220. Contains Duplicate III

## Intuition

The problem asks us to find if there are two distinct indices i and j such that:

1. The absolute difference between indices is at most k: |i - j| <= k
2. The absolute difference between values is at most t: |nums[i] - nums[j]| <= t

The key insight is to use a bucket sort approach where we group numbers into buckets of size (t + 1). If two numbers are in the same bucket, their difference is guaranteed to be <= t. We also need to check adjacent buckets since numbers in neighboring buckets might have a difference <= t.

## Approach

1. **Bucket Strategy**: Create buckets of width (t + 1). Numbers in the same bucket will have a difference <= t.
2. **Bucket Index Calculation**:
    - For positive numbers: `bucketIdx = num / width`
    - For negative numbers: `bucketIdx = (num / width) - 1` (to handle negative division correctly)
3. **Three Checks for Each Number**:
    - Same bucket: If current bucket already has a number, return true
    - Right adjacent bucket: Check if |num - bucket[bucketIdx + 1]| <= t
    - Left adjacent bucket: Check if |num - bucket[bucketIdx - 1]| <= t
4. **Sliding Window**: Maintain at most k+1 elements by removing the element that's k+1 positions behind the current element.
5. **Edge Case Handling**: Special handling for negative numbers to ensure correct bucket assignment.

## Complexity

- Time complexity: O(n)
- Space complexity: O(min(n, k))

## Keywords

- Bucket Sort
- Sliding Window
- Hash Map

## Code

```go
func containsNearbyAlmostDuplicate(nums []int, k int, t int) bool {
    abs := func(x int) int {
        if x < 0 {
            return -x
        }
        return x
    }

    bucket, width := make(map[int]int), t + 1
    for i, num := range nums {
        var bucketIdx int

        if num >= 0 {
            bucketIdx = num / width
        } else {
            bucketIdx = (num / width) - 1
        }

        if _, exist := bucket[bucketIdx]; exist {
            return true
        }
        if n, exist := bucket[bucketIdx + 1]; exist && abs(num - n) < width {
            return true
        }
        if n, exist := bucket[bucketIdx - 1]; exist && abs(num - n) < width {
            return true
        }

        bucket[bucketIdx] = num

        if i >= k {
            var delBucketIdx int

            if nums[i - k] >= 0 {
                delBucketIdx = nums[i - k] / width
            } else {
                delBucketIdx = (nums[i - k] / width) - 1
            }

            delete(bucket, delBucketIdx)
        }
    }
    return false
}
```
