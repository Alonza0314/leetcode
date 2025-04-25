# 435. Non-overlapping Intervals

## Intuition

To minimize the number of intervals that need to be removed to make the remaining intervals non-overlapping, we should:

1. Sort the intervals by their end time
2. Keep track of the current end time
3. Remove intervals that overlap with the current end time

## Approach

1. Sort all intervals based on their end time in ascending order
2. Initialize variables:
   - `end`: tracks the end time of the last valid interval (initialized to -50001)
   - `ret`: counts the number of intervals that need to be removed
3. Iterate through the sorted intervals:
   - If current interval's start time is greater than or equal to previous end time:
     - Update end time to current interval's end time
   - Otherwise:
     - Increment the removal counter (ret)
4. Return the total number of intervals that need to be removed

## Complexity

- Time complexity: O(nlogn)
- Space complexity: O(1)

## Keywords

- Array
- Sorting
- Greedy
- Interval Scheduling

## Code

```go
func eraseOverlapIntervals(intervals [][]int) int {
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][1] < intervals[j][1]
    })
    end, ret := -50001, 0
    for _, interval := range intervals {
        if end <= interval[0] {
            end = interval[1]
        } else {
            ret += 1
        }
    }
    return ret
}
```
