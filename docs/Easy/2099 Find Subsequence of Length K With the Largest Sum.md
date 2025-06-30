# 2099. Find Subsequence of Length K With the Largest Sum

## Intuition

To find a subsequence with the largest sum and length k, we need to select the k largest numbers from the array while maintaining their relative order in the original array. This suggests we need both sorting and index tracking.

## Approach

1. Create a structure to store both the number and its original index
2. Convert the input array into an array of this structure
3. Sort the array based on the numbers in ascending order
4. Take the last k elements (largest numbers)
5. Sort these k elements based on their original indices to maintain the relative order
6. Extract just the numbers from the sorted result

## Complexity

- Time complexity: O(nlogn)
- Space complexity: O(n)

## Keywords

- Sorting
- Index tracking
- Subsequence

## Code

```go
func maxSubsequence(nums []int, k int) []int {
    type unit struct {
        n int
        i int
    }
    ns := make([]unit, len(nums))
    for i, n := range nums {
        ns[i].n = n
        ns[i].i = i
    }
    sort.Slice(ns, func(i, j int) bool {
        return ns[i].n < ns[j].n
    })
    
    tgtNs := ns[len(nums) - k:]
    sort.Slice(tgtNs, func(i, j int) bool {
        return tgtNs[i].i < tgtNs[j].i
    })
    ret := make([]int, len(tgtNs))
    for i, u := range tgtNs {
        ret[i] = u.n
    }

    return ret
}
```
