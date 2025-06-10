# 881. Boats to Save People

## Intuition

This problem will use two pointer to record the current limit is meet the limit or not and count the total boats we need.

## Approach

1. Sort the array of people's weights in ascending order.
2. Use two pointers: one starting from the lightest person (left) and one from the heaviest person (right).
3. For each iteration, try to pair the lightest and heaviest people:
    - If their combined weight is within the limit, both can share a boat (increment left pointer).
    - If their combined weight exceeds the limit, the heavier person must take a boat alone.
4. In either case, the right pointer decreases and we count one more boat.
5. Continue until all people are assigned to boats.
6. If there's one person left (when left equals right), allocate one more boat.

## Complexity

- Time complexity: O(NlogN)
- Space complexity: O(1)

## Keywords

- Two Pointers
- Greedy Algorithm
- Sorting

## Code

```go
func numRescueBoats(people []int, limit int) int {
    ret, i, j := 0, 0, len(people) - 1
    sort.Ints(people)

    for i < j {
        if people[i] + people[j] <= limit {
            i += 1
        }
        j, ret = j - 1, ret + 1
    }

    if i == j {
        ret += 1
    }
    return ret
}
```
