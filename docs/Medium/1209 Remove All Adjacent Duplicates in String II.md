# 1209. Remove All Adjacent Duplicates in String II

## Intuition

The problem requires us to remove k adjacent duplicate characters from a string. We can use a stack-based approach where we keep track of both the character and its count. When we encounter k consecutive same characters, we remove them from the stack.

## Approach

1. Create a stack to store pairs of (character, count)
2. Iterate through each character in the string:
    - If stack is empty, push the character with count 1
    - If current character matches the top of stack:
        - Increment the count
        - If count reaches k, pop the element from stack
    - If current character is different, push it with count 1
3. Finally, reconstruct the string from the stack by repeating each character according to its count

## Complexity

- Time complexity: O(n)
- Space complexity: O(n)

## Keywords

- Stack
- String Manipulation

## Code

```go
func removeDuplicates(s string, k int) string {
    type unit struct {
        char rune
        cnt int
    }

    st := make([]unit, 0)

    for _, char := range s {
        if len(st) == 0 {
            st = append(st, unit{char: char, cnt: 1})
            continue
        }
        if st[len(st) - 1].char == char {
            st[len(st) - 1].cnt += 1
            if st[len(st) - 1].cnt == k {
                st = st[:len(st) - 1]
            }
            continue
        }
        st = append(st, unit{char: char, cnt: 1})
    }

    ret := ""
    for _, u := range st {
        for i := 0; i < u.cnt; i += 1 {
            ret += string(u.char)
        }
    }
    return ret
}
```
