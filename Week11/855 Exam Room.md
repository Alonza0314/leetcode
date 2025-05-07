# 855. Exam Room

## Intuition

When a new student enters, we need to find the seat that maximizes the minimum distance to any other student. When a student leaves, we need to efficiently remove them from the seating arrangement.

## Approach

1. Use a sorted array to maintain the current seating arrangement
2. For the `Seat()` operation:
   - If the room is empty, place the first student at seat 0
   - Otherwise, find the maximum gap between existing students
   - Consider three cases:
     a. Gap at the beginning (seat 0)
     b. Gaps between existing students
     c. Gap at the end (last seat)
   - Insert the new student at the position that creates the maximum minimum distance
3. For the `Leave()` operation:
   - Use binary search to find the student's position
   - Remove them from the array while maintaining the sorted order

## Complexity

- Time complexity:
  - Seat(): O(n)
  - Leave(): O(log n) for binary search
- Space complexity: O(n)

## Keywords

- Binary Search

## Code

```go
type ExamRoom struct {
    seats int
    students []int
}


func Constructor(n int) ExamRoom {
    return ExamRoom{
        seats: n,
        students: make([]int, 0),
    }
}


func (e *ExamRoom) Seat() int {
    if len(e.students) == 0 {
        e.students = append(e.students, 0)
        return 0
    }

    seat, distance, idx := 0, e.students[0], 0

    for i := 0; i < len(e.students) - 1; i += 1 {
        curDistance := (e.students[i + 1] - e.students[i]) / 2
        if curDistance > distance {
            distance, seat, idx = curDistance, e.students[i] + curDistance, i + 1
        }
    }

    if e.seats - 1 - e.students[len(e.students) - 1] > distance {
        distance = e.seats - 1 - e.students[len(e.students) - 1]
        seat, idx = e.seats - 1, len(e.students)
    }

    e.students = append(e.students, 0)
    copy(e.students[idx + 1:], e.students[idx:])
    e.students[idx] = seat
    return seat
}


func (e *ExamRoom) Leave(p int)  {
    l, r := 0, len(e.students) - 1
    for l <= r {
        m := (l + r) >> 1
        if e.students[m] == p {
            e.students = append(e.students[:m], e.students[m + 1:]...)
            return
        }

        if e.students[m] < p {
            l = m + 1
        } else {
            r = m - 1
        }
    }
}


/**
 * Your ExamRoom object will be instantiated and called as such:
 * obj := Constructor(n);
 * param_1 := obj.Seat();
 * obj.Leave(p);
 */
```
