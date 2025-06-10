# 23. Merge k Sorted List

## Intuition

To merge k sorted linked lists efficiently, we can use a min heap (priority queue) to always get the smallest element among all lists. This approach allows us to maintain a sorted order while merging the lists.

## Approach

1. Create a min heap to store the head nodes of all k lists
2. Initialize the heap with the first node from each list (if not nil)
3. Create a dummy node to build the result list
4. While the heap is not empty:
    - Extract the minimum node from the heap
    - Add it to the result list
    - If the extracted node has a next node, push it back into the heap
5. Return the merged list

## Complexity

- Time complexity: O(N log k)
- Space complexity: O(k)

## Keywords

- Linked List
- Heap/Priority Queue
- Merge Sort

## Code

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
type Comparator func(child, parent interface{}) bool
type heap struct {
    Storage []interface{}
    CmpFunc Comparator
}

func NewHeap(cmpFunc Comparator) *heap {
    return &heap {
        Storage: append(make([]interface{}, 0), -1),
        CmpFunc: cmpFunc,
    }
}

func (h *heap) Len() int {
    return len(h.Storage) - 1
}

func (h *heap) IsEmpty() bool {
    return h.Len() == 0
}

func (h *heap) cmp(child, parent interface{}) bool {
    return h.CmpFunc(child, parent)
}

func (h *heap) swap(x, y int) {
    h.Storage[x], h.Storage[y] = h.Storage[y], h.Storage[x]
}

func (h *heap) Top() (interface{}, error) {
    if h.IsEmpty() {
        return nil, errors.New("Heap is empty.")
    }
    return h.Storage[1], nil
}

func (h *heap) Push(item interface{}) {
    h.Storage = append(h.Storage, item)
    now := h.Len()
    for now / 2 > 0 && !h.cmp(h.Storage[now], h.Storage[now / 2]) {
        h.swap(now, now / 2)
        now /= 2
    }
}

func (h *heap) Pop() (interface{}, error) {
    top, err := h.Top()
    if err != nil {
        return nil, err
    }
    last := h.Len()
    h.swap(1, last)
    h.Storage = h.Storage[: last]
    now := 1
    for now < last {
        left, right := 0, 0
        if now * 2 < last && !h.cmp(h.Storage[now * 2], h.Storage[now]) {
            left = now * 2
        }
        if now * 2 + 1 < last && !h.cmp(h.Storage[now * 2 + 1], h.Storage[now]) {
            right = now * 2 + 1
        }

        if left == 0 && right == 0 {
            break
        } else if left != 0 && right == 0 {
            h.swap(now, left)
            now = left
        } else if left == 0 && right != 0 {
            h.swap(now, right)
            now = right
        } else {
            if h.cmp(h.Storage[left], h.Storage[right]) {
                h.swap(now, right)
                now = right
            } else {
                h.swap(now, left)
                now = left
            }
        }
    }
    return top, nil
}

func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }
    cmpFunc := func(child, parent interface{}) bool {
        return child.(*ListNode).Val > parent.(*ListNode).Val
    }
    hp := NewHeap(cmpFunc)
    for _, node := range lists {
        if node != nil {
            hp.Push(node)
        }
    }
    prev := &ListNode{Val: 0, Next: nil}
    cur := prev
    for !hp.IsEmpty() {
        top, _ := hp.Pop()
        cur.Next = top.(*ListNode)
        cur = cur.Next
        if next := top.(*ListNode).Next; next != nil {
            hp.Push(next)
        }
    }
    return prev.Next
}
```
