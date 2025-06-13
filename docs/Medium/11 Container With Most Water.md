# 11. Container With Most Water

## Intuition

The key insight is that the area of water contained between two lines is determined by:

1. The distance between the lines (width)
2. The height of the shorter line (as water can only be contained up to the height of the shorter line)

## Approach

1. Use two pointers technique, starting from both ends of the array
2. Calculate the area between the two pointers
3. Move the pointer pointing to the shorter line inward, as this is the only way we might find a larger area
4. Keep track of the maximum area found so far

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Two Pointers

## Code

```go
func maxArea(height []int) int {
    l, r := 0, len(height) - 1
    ret := math.MinInt
    for l < r {
        ret = max(ret, (r - l) * min(height[l], height[r]))
        if height[l] < height[r] {
            l += 1
        } else {
            r -= 1
        }
    }
    return ret
}
```
