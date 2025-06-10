# 3362 Zero Array Transformation III

## Intuition

This problem requires us to transform an array to all zeros using a set of queries, where each query [i, j] allows us to decrease all elements from index i to j by 1. The key insight is to use a max heap to track the available ranges and greedily apply them to reduce numbers to zero.

## Approach

1. For each position i, we maintain a list of queries that start at i (start[i])
2. We process the array from left to right:
    - For each position i, add all queries starting at i to the max heap
    - Calculate how many decrements we need at current position (need)
    - Use the available ranges from heap to satisfy the need:
        - Remove expired ranges (end < current position)
        - Apply ranges greedily, tracking when each range's effect expires
3. If we can't satisfy the need at any position, return -1
4. Return the number of unused queries

## Complexity

- Time complexity: O(N * log Q)
- Space complexity: O(Q)

## Keywords

- Priority Queue (Heap)
- Greedy Algorithm

## Code

```go
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
		return nil, errors.New("heap is empty")
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

func maxRemoval(nums []int, queries [][]int) int {
	cmpFunc := func(child, parent interface{}) bool {
		return child.(int) < parent.(int)
	}
	hp, start, expireAt := NewHeap(cmpFunc), make([][]int, len(nums)), make([]int, len(nums))
	for _, q := range queries {
		start[q[0]] = append(start[q[0]], q[1])
	}

	chose, expire := 0, 0
	for i, num := range nums {
		for _, r := range start[i] {
			hp.Push(r)
		}

		if i > 0 {
			expire += expireAt[i - 1]
		}

		need := num - (chose - expire)
		for need > 0 {
			for !hp.IsEmpty() {
				top, _ := hp.Top()
				if top.(int) < i {
					hp.Pop()
				} else {
					break
				}
			}
			if hp.IsEmpty() {
				return -1
			}

			top, _ := hp.Pop()
			expireAt[top.(int)] += 1
			chose, need = chose + 1, need - 1
		}
	}

	return len(queries) - chose
}
```
