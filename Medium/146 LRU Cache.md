# 146. LRU Cache

## Intuition

The implementation uses a combination of a doubly linked list and a hash map to achieve O(1) operations. The doubly linked list maintains the order of elements (most recently used at head, least recently used at tail), while the hash map provides quick access to nodes.

## Approach

The implementation consists of these key components:

1. Data Structures:
   - `node`: Doubly linked list node containing key, value, and pointers
   - `queue`: Custom doubly linked list with head and tail
   - `storage`: Hash map for O(1) lookup of nodes

2. Queue Operations:
   - `insert`: Add new node at head (most recently used)
   - `delete`: Remove node from tail (least recently used)
   - `update`: Move existing node to head and update value

3. Cache Operations:
   - `Get`:
     - If key exists: Update node position to head and return value
     - If key doesn't exist: Return -1

   - `Put`:
     - If key exists: Update value and move to head
     - If key doesn't exist:
       - If capacity reached: Remove LRU (tail) and add new node at head
       - If capacity not reached: Add new node at head

## Complexity

- Time complexity: O(1)
- Space complexity: O(capacity)

## Keywords

- LRU Cache
- Doubly Linked List
- Hash Map

## Code

```go
type node struct {
    key int
    val int
    prev *node
    next *node
}

type queue struct {
    head *node
    tail *node
}

func (q *queue) insert(n *node) {
    if q.head == nil {
        q.head, q.tail = n, n
        return
    }
    q.head.prev, n.next = n, q.head
    q.head = n
}

func (q *queue) delete() {
    if q.head == q.tail {
        q.head, q.tail = nil, nil
        return
    }
    prev := q.tail.prev
    prev.next, q.tail = nil, prev
}

func (q *queue) update(n *node, val int) {
    n.val = val
    if q.head == n {
        return
    }
    if q.tail == n {
        prev := q.tail.prev
        prev.next, q.tail, n.prev = nil, prev, nil

        q.head.prev, n.next = n, q.head
        q.head = n
        return
    }
    prev, next := n.prev, n.next
    n.prev, n.next, prev.next, next.prev, q.head.prev = nil, q.head, next, prev, n
    q.head = n
}

type LRUCache struct {
    cap int
    siz int
    storage map[int]*node
    q *queue
}


func Constructor(capacity int) LRUCache {
    return LRUCache{
        cap: capacity,
        siz: 0,
        storage: make(map[int]*node),
        q: &queue{
            head: nil,
            tail: nil,
        },
    }
}


func (l *LRUCache) Get(key int) int {
    var ret int
    if n, find := l.storage[key]; !find {
        return -1
    } else {
        l.q.update(n, n.val)
        ret = n.val
    }
    return ret
}


func (l *LRUCache) Put(key int, value int)  {
    if n, find := l.storage[key]; find {
        l.q.update(n, value)
        return
    }
    n := &node{
        key: key,
        val: value,
        prev: nil,
        next: nil,
    }

    if l.siz == l.cap {
        delete(l.storage, l.q.tail.key)
        l.q.delete()
        l.siz -= 1
    }
    l.q.insert(n)
    l.storage[key] = n
    l.siz += 1
}


/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```
