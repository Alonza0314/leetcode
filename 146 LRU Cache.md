# 146. LRU Cache

## Intuition

Use a map to record each key-value pair with a timestamp(round).

## Approach

For `GET`, check the key is in our storage map or not. If not exists, return -1; otherwise, return the corresponding value and update the timestamp.

For `PUT`, also check the key is in our storage map or not. If exists, update the timestamp and return the value.
If not exists, which means we need to put the pair into the storage. Once the capacity is reached the max capacity, we need to remove the least used one, i.e, the smallest timestamp. Then we can insert the destination key-value pair.

## Complexity

- Time complexity: O(N)
- Space complexity: O(N)

## Keywords

- LRU
- Map
- Cache

## Code

```go
type node struct {
    val int
    round int
}

type LRUCache struct {
    roundCnt int
    capacity int
    size int
    storage map[int]*node
}


func Constructor(capacity int) LRUCache {
    return LRUCache{
        roundCnt: 0,
        capacity: capacity,
        size: 0,
        storage: make(map[int]*node),
    }
}


func (l *LRUCache) Get(key int) int {
    l.roundCnt += 1
    if _, ok := l.storage[key]; !ok {
        return -1
    }
    l.storage[key].round = l.roundCnt
    return l.storage[key].val
}


func (l *LRUCache) Put(key int, value int)  {
    l.roundCnt += 1
    if _, ok := l.storage[key]; ok {
        l.storage[key].val, l.storage[key].round = value, l.roundCnt
        return
    }
    if l.size < l.capacity {
        l.size += 1
        l.storage[key] = &node{
            val: value,
            round: l.roundCnt,
        }
        return
    }
    leastUsedKey, leastUsedRound:= -1, math.MaxInt
    for k, v := range l.storage {
        if v.round < leastUsedRound {
            leastUsedKey, leastUsedRound = k, v.round
        }
    }
    delete(l.storage, leastUsedKey)
    l.storage[key] = &node{
        val: value,
        round: l.roundCnt,
    }
}


/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```
