# 210. Course Schedule II

## Intuition

The problem is essentially about finding a valid topological order of courses given their prerequisites. We need to determine if it's possible to complete all courses and return the order in which they should be taken. This is a classic topological sorting problem where we need to find a linear ordering of vertices such that for every directed edge (u, v), vertex u comes before v in the ordering.

## Approach

1. Create a graph representation using an adjacency list and track the in-degree (number of prerequisites) for each course
2. Initialize a queue with courses that have no prerequisites (in-degree = 0)
3. Perform a BFS-like traversal:
   - Remove a course from the queue and add it to the result
   - For each course that has this course as a prerequisite, decrease their in-degree
   - If a course's in-degree becomes 0, add it to the queue
4. If the length of the result equals the total number of courses, return the result; otherwise, return an empty array indicating it's impossible to complete all courses

## Complexity

- Time complexity: O(V + E)
- Space complexity: O(V + E)

## Keywords

- Topological Sort
- Graph
- BFS

## Code

```go
func findOrder(numCourses int, prerequisites [][]int) []int {
    type node struct {
        ingress int
        used bool
        neighbor []int
    }

    ret, record := make([]int, 0), make([]node, numCourses)

    for _, pre := range prerequisites {
        a, b := pre[0], pre[1]
        if !record[b].used {
            record[b].neighbor = make([]int, 0)
        }
        record[a].used, record[b].used = true, true
        record[a].ingress, record[b].neighbor = record[a].ingress + 1, append(record[b].neighbor, a)
    }

    current := make([]int, 0)
    for i, n := range record {
        if !n.used {
            ret = append(ret, i)
            continue
        }
        if n.ingress == 0 {
            current = append(current, i)
        }
    }

    for len(current) != 0 {
        ret = append(ret, current...)
        n := len(current)
        for _, num := range current {
            for _, nb := range record[num].neighbor {
                record[nb].ingress -= 1
                if record[nb].ingress == 0 {
                    current = append(current, nb)
                }
            }
        }
        current = current[n:]
    }

    if len(ret) != numCourses {
        return []int{}
    }
    return ret
}
```
