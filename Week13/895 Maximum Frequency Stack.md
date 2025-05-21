# 895. Maximum Frequency Stack

## Intuition

The problem requires us to implement a stack-like data structure that can track the frequency of elements and pop the most frequent element. When multiple elements have the same frequency, we need to pop the one that was pushed most recently. The key insight is to maintain a frequency map and a stack of stacks, where each inner stack contains elements of the same frequency.

## Approach

1. Use a `freqMap` to track the frequency of each element
2. Use a `freqStack` which is a 2D slice where:
   - Each inner slice represents elements of the same frequency
   - The index of the inner slice represents the frequency level
   - Elements in the same inner slice maintain their push order
3. For Push operation:
   - If element is new or has frequency -1, add it to frequency level 0
   - Otherwise, increment its frequency and add it to the corresponding level
4. For Pop operation:
   - Remove the last element from the highest frequency level
   - Decrement its frequency in the map
   - Remove the frequency level if it becomes empty

## Complexity

- Time complexity: O(1)
- Space complexity: O(n)

## Keywords

- Stack
- Hash Map

## Code

```go
type FreqStack struct {
    freqMap map[int]int
    freqStack [][]int
}


func Constructor() FreqStack {
    f := FreqStack{
        freqMap: make(map[int]int),
        freqStack: make([][]int, 0),
    }
    f.freqStack = append(f.freqStack, make([]int, 0))
    return f
}


func (f *FreqStack) Push(val int)  {
    if level, found := f.freqMap[val]; !found || level == -1 {
        f.freqStack[0], f.freqMap[val] = append(f.freqStack[0], val), 0
    } else {
        if len(f.freqStack) - 1 == level {
            f.freqStack = append(f.freqStack, make([]int, 0))
        }
        f.freqStack[level + 1], f.freqMap[val] = append(f.freqStack[level + 1], val), level + 1
    }
}


func (f *FreqStack) Pop() int {
    tgt := f.freqStack[len(f.freqStack) - 1][len(f.freqStack[len(f.freqStack) - 1]) - 1]
    f.freqStack[len(f.freqStack) - 1], f.freqMap[tgt] = f.freqStack[len(f.freqStack) - 1][: len(f.freqStack[len(f.freqStack) - 1]) - 1], f.freqMap[tgt] - 1
    if len(f.freqStack[len(f.freqStack) - 1]) == 0 {
        f.freqStack = f.freqStack[: len(f.freqStack) - 1]
    }
    return tgt
}


/**
 * Your FreqStack object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(val);
 * param_2 := obj.Pop();
 */
```
