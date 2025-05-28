# 2071. Maximum Number of Tasks You Can Assign

## Intuition

The problem asks us to find the maximum number of tasks we can assign to workers, where each worker can complete at most one task. Workers have different strengths, tasks have different requirements, and we have a limited number of pills that can boost a worker's strength by a fixed amount.

The key insight is that this is an optimization problem where we want to maximize the number of completed tasks. We can use binary search on the answer - if we can complete k tasks, we might be able to complete k+1 tasks, but if we can't complete k tasks, we definitely can't complete more than k tasks.

## Approach

1. **Sort both arrays**: Sort tasks in ascending order and workers in descending order of their strength.
2. **Binary search on the answer**: Use binary search to find the maximum number of tasks we can complete. The search space is from 0 to min(len(tasks), len(workers)).
3. **Greedy assignment strategy**: For a given number k of tasks to complete:
   - Take the k easiest tasks (first k tasks after sorting)
   - Take the k strongest workers (last k workers after sorting)
   - Process tasks from hardest to easiest among the selected k tasks
   - For each task, try to assign it using the greedy strategy:
     - First, try to assign the strongest available worker without using a pill
     - If that's not possible, find the weakest worker who can complete the task with a pill
     - If no worker can complete the task even with a pill, return false
4. **Optimization with pills**: When we need to use a pill, we should use it on the weakest possible worker who can complete the task. This preserves stronger workers for harder tasks.

The greedy strategy works because:

- We process harder tasks first, ensuring they get priority for stronger workers
- When using pills, we use them on the weakest capable worker to preserve stronger workers
- This maximizes our chances of completing all k tasks

## Complexity

- Time complexity: O(n log n + m log m + log(min(n,m)) × m log m)
- Space complexity: O(m)

## Keywords

- Binary Search
- Greedy Algorithm

## Code

```go
func maxTaskAssign(tasks []int, workers []int, pills int, strength int) int {
    sort.Ints(tasks)
    sort.Ints(workers)

    canAssign := func(k int) bool {
        avail, remain := append(make([]int, 0), workers[len(workers) - k:]...), pills
        for i := k - 1; i >= 0; i -= 1 {
            require := tasks[i]
            if len(avail) > 0 && avail[len(avail) - 1] >= require {
                avail = avail[: len(avail) - 1]
            } else {
                if remain <= 0 {
                    return false
                }
                thresold := require - strength
                idx := sort.Search(len(avail), func(j int) bool {
                    return avail[j] >= thresold
                })
                if idx == len(avail) {
                    return false
                }
                avail = append(avail[:idx], avail[idx + 1:]...)
                remain -= 1
            }
        }
        return true
    }

    left, right, complete := 0, min(len(tasks), len(workers)), 0
    for left <= right {
        mid := (left + right) / 2
        if canAssign(mid) {
            complete, left = mid, mid + 1
        } else {
            right = mid - 1
        }
    }
    return complete
}
```
