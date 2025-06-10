# 347. Top K Frequent Elements

## Intuition

The problem asks us to find the k most frequent elements in an array. The key insight is that we need to:

1. Count the frequency of each element
2. Find the k elements with the highest frequencies

A heap (priority queue) is perfect for this task because it allows us to efficiently maintain the top k elements based on their frequencies.

## Approach

1. **Count Frequencies**: Use a hash map to count the frequency of each element in the input array.
2. **Build Max Heap**: Create a max heap where elements are ordered by their frequencies. We implement a custom heap data structure with:
    - A comparator function to define the heap property
    - Standard heap operations: Push, Pop, Top
    - Heap maintenance through up-heap and down-heap operations
3. **Extract Top K**: Push all unique elements with their frequencies into the heap, then pop k times to get the k most frequent elements.

The custom heap implementation uses 1-indexed storage for easier parent-child calculations:

- Parent of node i: i/2
- Left child of node i: 2*i
- Right child of node i: 2*i+1

## Complexity

- Time complexity: O(n log n)
- Space complexity: O(n)

## Keywords

- Hash Map
- Heap (Priority Queue)
- Frequency Counting
- Top K Problems
- Custom Data Structure

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

func topKFrequent(nums []int, k int) []int {
    cnt := make(map[int]int)
    for _, num := range nums {
        cnt[num] += 1
    }
    type unit struct {
        num, freq int
    }
    cmpFunc := func(child, parent interface{}) bool {
        return child.(unit).freq < parent.(unit).freq
    }
    hp, ret := NewHeap(cmpFunc), make([]int, 0)
    for key, val := range cnt {
        hp.Push(unit{key, val})
    }
    for k > 0 {
        top, _ := hp.Pop()
        ret = append(ret, top.(unit).num)
        k -= 1
    }
    return ret
}
```
