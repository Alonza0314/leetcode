# 440. K-th Smallest in Lexicographical Order

## Intuition

This problem requires finding the k-th smallest number in lexicographical order from 1 to n. The key insight is that numbers in lexicographical order form a denary (base-10) trie structure. For example, numbers starting with 1 come before numbers starting with 2, and numbers starting with 10 come before numbers starting with 11.

## Approach

1. We use a helper function `countPrefixNums` to count how many numbers with a given prefix exist within range [1, n].
2. Start with prefix = 1 (smallest possible number).
3. For each step:
    - Calculate how many numbers we can skip with current prefix
    - If we can skip more numbers than k remaining, we go deeper by multiplying prefix by 10
    - Otherwise, we move to next prefix by adding 1 and subtract skipped numbers from k
4. Continue this process until k becomes 0

## Complexity

- Time complexity: O(log n * log n)
- Space complexity: O(1)

## Keywords

- Trie
- Lexicographical Order
- Prefix Counting
- Math

## Code

```go
func findKthNumber(n int, k int) int {
    countPrefixNums := func(prefix int) int {
        cur, next, ret := prefix, prefix + 1, 0
        for cur <= n {
            ret += min(n + 1, next) - cur
            cur, next = cur * 10, next * 10
        }
        return ret
    }

    ret := 1
    k -= 1
    for k > 0 {
        skipNums := countPrefixNums(ret)
        if skipNums <= k {
            k, ret = k - skipNums, ret + 1
        } else {
            k, ret = k - 1, ret * 10
        }
    }
    return ret
}
```
