# 42. Trapping Rain Water

## Intuition

The key insight is that the amount of water trapped at any position depends on the minimum of the maximum heights on both sides. We can use two pointers approach to track the maximum heights from both ends and calculate the trapped water accordingly.

## Approach

1. Use two pointers (left and right) starting from both ends of the array
2. Keep track of the maximum heights seen from both sides (minL and minR)
3. Move the pointer pointing to the smaller height
4. For each position:
   - If current height is greater than or equal to the maximum height seen so far, update the maximum
   - Otherwise, add the difference between maximum height and current height to the result
5. Continue until left pointer meets right pointer

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Two Pointers

## Code

```go
func trap(height []int) int {
    if len(height) < 2 {
        return 0
    }
    left, right, ret, minL, minR := 0, len(height) - 1, 0, math.MinInt, math.MinInt
    for left < right {
        if height[left] < height[right] {
            if height[left] >= minL {
                minL = height[left]
            } else {
                ret += minL - height[left]
            }
            left += 1
        } else {
            if height[right] >= minR {
                minR = height[right]
            } else {
                ret += minR - height[right]
            }
            right -= 1
        }
    }
    return ret
}
```
