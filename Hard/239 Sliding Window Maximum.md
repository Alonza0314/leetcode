# 239. Sliding Window Maximum

## Intuition

The key concepts of the problem is to maintain a "MAX" list.

## Approach

For each round, you need to remove the indexes from queue if the corresponding element in `nums` are smaller than curent number.

After remove all the smaller elements, append the current number to the queue.

Check the indexes in queue is in `k` range; otherwise, remove them.

Finally, take the first index in `queue` and append the corresponding number to the return slice from `nums`.

(`queue` will maintain a large to small list.)

## Complexity

- Time complexity: O(N)
- Space complexity: O(k)

## Keywords

- Sliding Window
- Array

## Code

```go
func maxSlidingWindow(nums []int, k int) []int {
    queue, ret := make([]int, 0), make([]int, 0)

    for i, num := range nums {
        for len(queue) > 0 && nums[queue[len(queue) - 1]] < num {
            queue = queue[: len(queue) - 1]
        }
        queue = append(queue, i)

        if queue[0] <= i - k {
            queue = queue[1:]
        }

        if i >= k - 1 {
            ret = append(ret, nums[queue[0]])
        }
    }

    return ret
}
```
