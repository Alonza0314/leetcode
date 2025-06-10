# 460. LFU Cache

## Intuition

The implementation combines a doubly linked list with hash maps to create an LFU (Least Frequently Used) cache. The main ideas are:

1. Use a hash map `storage` to map keys to nodes
2. Use a hash map `freqMap` to map frequencies to their corresponding frequency lists
3. Each frequency list uses a doubly linked list to store nodes with the same access frequency

## Approach

The implementation consists of several key components:

1. `Get` operation:
    - If key doesn't exist, return -1
    - If key exists, move the node to the next frequency list and return its value

2. `Put` operation:
    - If key exists, update value and increase frequency
    - If key doesn't exist:
        - If capacity not reached, add to frequency-1 list
        - If capacity reached, remove the first node (earliest) from the lowest frequency list, then add new node

3. Frequency list operations:
    - `freqDelete`: Remove a node from a specific frequency list
    - `freqAdd`: Add a node to a specific frequency list
    - `moveToNextFreq`: Move a node to the next higher frequency list

## Complexity

- Time complexity: O(1)
- Space complexity: O(capacity)

## Keywords

- LFU Cache
- Double Linked List
- Hash Map
- Frequency Counter
- Cache Implementation

## Code

```go
type node struct {
    key int
    val int
    freq int
    prev *node
    next *node
}

type frequence struct {
    head *node
    tail *node
}

func newFreq() *frequence {
    return &frequence {
        head: nil,
        tail: nil,
    }
}

func (f *frequence) freqDelete(n *node) {
    if f.head == n {
        if f.tail == n {
            f.head, f.tail = nil, nil
        } else {
            headNext := f.head.next
            headNext.prev = nil
            f.head = headNext
        }
        n.prev, n.next = nil, nil
        return
    }
    if f.tail == n {
        tailPrev := f.tail.prev
        tailPrev.next = nil
        f.tail = tailPrev
        n.prev, n.next = nil, nil
        return
    }
    nPrev, nNext := n.prev, n.next
    nPrev.next, nNext.prev = nNext, nPrev
    n.prev, n.next = nil, nil
}

func (f *frequence) freqAdd(n *node) {
    if f.head == nil {
        f.head, f.tail = n, n
        n.prev, n.next = nil, nil
        return
    }
    n.prev, n.next = f.tail, nil
    f.tail.next = n
    f.tail = n
}

type LFUCache struct {
    cap int
    size int
    storage map[int]*node
    freqMap map[int]*frequence
}


func Constructor(capacity int) LFUCache {
    l := LFUCache {
        cap: capacity,
        size: 0,
        storage: make(map[int]*node),
        freqMap: make(map[int]*frequence),
    }
    l.freqMap[1] = newFreq()
    return l
}

func (l *LFUCache) moveToNextFreq(n *node) {
    l.freqMap[n.freq].freqDelete(n)
    n.freq += 1
    if _, ok := l.freqMap[n.freq]; !ok {
        l.freqMap[n.freq] = newFreq()
    }
    l.freqMap[n.freq].freqAdd(n)
}

func (l *LFUCache) getLF() (int, *node) {
    for i := 1; i <= len(l.freqMap); i += 1 {
        if l.freqMap[i].head != nil {
            return i, l.freqMap[i].head
        }
    }
    return -1, nil
}

func (l *LFUCache) Get(key int) int {
    if _, ok := l.storage[key]; !ok {
        return -1
    }
    l.moveToNextFreq(l.storage[key])
    return l.storage[key].val
}


func (l *LFUCache) Put(key int, value int)  {
    if n, ok := l.storage[key]; ok {
        n.val = value
        l.moveToNextFreq(n)
        return
    }
    n := node{
        key: key,
        val: value,
        freq: 1,
        prev: nil,
        next: nil,
    }
    l.storage[key] = &n
    if l.size < l.cap {
        l.freqMap[1].freqAdd(&n)
        l.size += 1
        return
    }
    f, deleteNode := l.getLF()
    l.freqMap[f].freqDelete(deleteNode)
    l.freqMap[1].freqAdd(&n)
    delete(l.storage, deleteNode.key)
}


/**
 * Your LFUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```
