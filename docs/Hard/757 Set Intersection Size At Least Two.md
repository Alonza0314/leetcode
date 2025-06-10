# 757. Set Intersection Size At Least Two

## Intuition

The key insight is to greedily select elements that can satisfy multiple intervals simultaneously. By sorting intervals by their end points, we can process them in an order that allows us to make optimal choices.

## Approach

1. Sort the intervals by their end points. If two intervals have the same end point, sort them by their start points in descending order (larger start point first).
2. Initialize variables to track the two most recently added elements to our set S and the size of S.
3. Iterate through the sorted intervals:
    - If the current interval doesn't overlap with our existing elements (prevEnd < start), add the two largest possible elements from this interval (end-1 and end).
    - If the current interval partially overlaps (prevStart < start ≤ prevEnd), we need to add one more element (the end of the current interval).
    - If the current interval fully contains our existing elements (start ≤ prevStart), we don't need to add any new elements.
4. Return the total size of set S.

## Complexity

- Time complexity: O(NlogN)
- Space complexity: O(logN)

## Keywords

- Greedy Algorithm
- Interval Problems
- Sorting

## Code

```go
func intersectionSizeTwo(intervals [][]int) int {
    sort.Slice(intervals, func(i, j int) bool {
        if intervals[i][1] == intervals[j][1] {
            return intervals[i][0] > intervals[j][0]
        }
        return intervals[i][1] < intervals[j][1]
    })
    
    prevStart, prevEnd, ret := -1, -1, 0
    for _, interval := range intervals {
        start, end := interval[0], interval[1]

        if prevEnd < start {
            prevStart, prevEnd = end - 1, end
            ret += 2
            continue
        }
        if prevStart < start && start <= prevEnd {
            prevStart, prevEnd = prevEnd, end
            ret += 1
        }
    }

    return ret
}
```
