# 2. Add Two Numbers

## Intuition

The problem requires adding two numbers represented by linked lists, where each digit is stored in reverse order. The key insight is to traverse both lists simultaneously while handling the carry-over value, similar to how we perform manual addition.

## Approach

1. Create a helper struct `HL` (Headed List) to manage the result linked list, with methods to append nodes.
2. Traverse both input linked lists simultaneously until both lists are exhausted and there's no remaining carry value.
3. For each position:
    - Sum the digits from both lists (if available) and the carry value
    - Calculate new carry value (sum / 10)
    - Append the current digit (sum % 10) to result list
4. Return the head of the resulting linked list

## Complexity

- Time complexity: O(max(N, M))
- Space complexity: O(max(N, M))

## Keywords

- Linked List
- Math
- Two Pointers

## Code

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

 type HL struct {
     Head *ListNode
     Tail *ListNode
 }
 func (hl *HL) Append(val int) {
     newNode := &ListNode{Val: val}
     if hl.Head == nil{
         hl.Head = newNode
         hl.Tail = newNode
     } else {
         hl.Tail.Next =  newNode
         hl.Tail = hl.Tail.Next
     }
 }
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    hl := HL{}
    carry_in := 0
    for l1!=nil || l2!=nil || carry_in!=0 {
        tmp := 0
        if l1!=nil {
            tmp += l1.Val
            l1 = l1.Next
        }
        if l2!=nil {
            tmp += l2.Val
            l2 = l2.Next
        }
        tmp += carry_in
        carry_in = tmp/10
        hl.Append(tmp%10)
    }
    return hl.Head
}
```
