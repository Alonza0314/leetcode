# 4. Median of Two Sorted Arrays

## Intuition

The problem requires finding the median of two sorted arrays. The key insight is that we only need to merge the arrays until we reach the middle position(s) to find the median, rather than merging the entire arrays. This saves both time and space complexity.

## Approach

1. Create a new array `nums3` to store the merged elements up to the median position
2. Use two pointers `i` and `j` to track positions in `nums1` and `nums2` respectively
3. Compare elements from both arrays and add the smaller one to `nums3`
4. Continue this process until we reach the middle position(s)
5. For odd length total, return the middle element
6. For even length total, return average of two middle elements

## Complexity

- Time complexity: O(m+n)
- Space complexity: O((m+n)/2)

## Keywords

- Two Pointers
- Merge Sort
- Median

## Code

```go
func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    m, n := len(nums1), len(nums2)
    l := m+n
    i, j := 0, 0
    var nums3 []int
    for i<m && j<n && (i+j)<=(l/2) {
        if nums1[i]<nums2[j] {
            nums3 = append(nums3, nums1[i])
            i += 1
        } else {
            nums3 = append(nums3, nums2[j])
            j += 1
        }
    }
    for (i+j)<=(l/2) && i<m {
        nums3 = append(nums3, nums1[i])
        i += 1
    }
    for (i+j)<=(l/2) && j<n {
        nums3 = append(nums3, nums2[j])
        j += 1
    }
    length := len(nums3)
    if l%2 == 0 {
        return (float64(nums3[length-1])+float64(nums3[length-2]))/float64(2)
    }
    return float64(nums3[length-1])
}
```
