# 24. Swap Nodes in Pairs

## Intuition

The key insight is to maintain proper connections while swapping nodes. We need to keep track of the node before each pair so we can properly reconnect the swapped pairs.

## Approach

1. Create a dummy node (prev) that points to the head to handle the case when the head itself gets swapped
2. Use a current pointer (cur) to track the node before each pair to be swapped
3. For each pair (a, b):
    - Save references to both nodes
    - Update a's next pointer to point to b's next
    - Update b's next pointer to point to a
    - Update cur's next pointer to point to b
    - Move cur two nodes ahead for the next pair
4. Return prev.Next as the new head

## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## Keywords

- Linked List
- Iterative
- In-place Modification

## Code

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func swapPairs(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    prev := &ListNode{Val: 0, Next: head}
    cur := prev
    for cur.Next != nil {
        a, b := cur.Next, cur.Next.Next
        if b == nil {
            break
        }
        a.Next = b.Next
        cur.Next, b.Next = b, a
        cur = cur.Next.Next
    }
    return prev.Next
}
```
