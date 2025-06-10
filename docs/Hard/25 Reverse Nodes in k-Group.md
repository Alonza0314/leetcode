# 25. Reverse Nodes in k-Group

## Intuition

To reverse nodes in k-group, we need to:

1. Check if there are at least k nodes left in the list
2. If yes, reverse those k nodes as a group
3. Connect the previous part with the reversed group and the remaining list
4. Continue this process until we can't form groups of k nodes

## Approach

1. Create a dummy node as the previous node of the head to handle edge cases
2. Define a closure function `Reverse` that:
    - Takes a node and reverses the next k nodes
    - Returns the new start and end of the reversed group
3. Iterate through the list:
    - Check if there are k nodes ahead
    - If yes, reverse them using the `Reverse` function
    - Update pointers to connect the reversed group
    - Move to the end of the current group
4. Return the next node of dummy (the new head)

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Linked List
- Reversal

## Code

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
    prev := &ListNode{Val: 0, Next: head}
    cur := prev
    Reverse := func(now *ListNode) (*ListNode, *ListNode) {
        end, head := now.Next, now.Next.Next
        rec, n := end, k - 1
        rec.Next = nil
        for n > 0 {
            tmp := head.Next
            head.Next = end
            end = head
            head = tmp
            n -= 1
        }
        rec.Next = head
        return end, rec
    }
    for cur != nil {
        n, tmp := k, cur
        for n > 0 {
            tmp = tmp.Next
            if tmp == nil {
                goto RET
            }
            n -= 1
        }
        tmp = tmp.Next
        var start *ListNode
        cur.Next, start = Reverse(cur)
        cur = start
    }
RET:
    return prev.Next
}
```
