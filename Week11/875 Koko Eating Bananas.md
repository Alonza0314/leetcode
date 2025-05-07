# 875. Koko Eating Bananas

## Intuition

We can use binary search to find the optimal speed since:

1. If Koko can finish bananas at speed k, she can definitely finish them at any speed > k
2. If Koko cannot finish bananas at speed k, she definitely cannot finish them at any speed < k

## Approach

1. Use binary search to find the minimum eating speed
2. The search range is from 1 to the maximum pile size
3. For each speed k:
   - Check if Koko can finish all piles within h hours
   - If pile size ≤ k, it takes 1 hour
   - If pile size > k, it takes (pile size / k) hours, rounded up
4. Return the minimum speed that allows Koko to finish all bananas

## Complexity

- Time complexity: O(n * log M)
- Space complexity: O(1)

## Keywords

- Binary Search

## Code

```go
func check(mid, h int, piles []int) bool {
    for _, pile := range piles {
        if pile <= mid {
            h -= 1
        } else {
            if pile % mid != 0 {
                h -= 1
            }
            h -= (pile / mid)
        }
        if h < 0 {
            return false
        }
    }
    return true
}

func minEatingSpeed(piles []int, h int) int {
    maxPile := 0
    for _, pile := range piles {
        maxPile = max(maxPile, pile)
    }
    l, r := 1, maxPile
    for l <= r {
        mid := (l + r) / 2
        if check(mid, h, piles) {
            r = mid - 1
        } else {
            l = mid + 1
        }
    }
    return l
}
```
