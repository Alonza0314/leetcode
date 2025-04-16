# 236. Lowest Common Ancestor of a Binary Tree

## Intuition

 We can use a post-order traversal approach where we first check the left and right subtrees, then process the current node.

## Approach

1. We use a recursive helper function `find` that takes the current node and the two target nodes p and q.
2. The base cases are:
   - If the current node is nil, return nil
   - If the current node is either p or q, return the current node
3. We recursively search in both left and right subtrees
4. The logic for determining the LCA is:
   - If both left and right subtrees return nil, return nil (neither p nor q found)
   - If only one subtree returns a non-nil value, return that value (one of p or q found)
   - If both subtrees return non-nil values, the current node is the LCA

## Complexity

- Time complexity: O(n)
- Space complexity: O(h)

## Keywords

- Binary Tree
- Recursion
- DFS
- Post-order Traversal
- Lowest Common Ancestor

## Code

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func find(node, p, q *TreeNode) *TreeNode {
    if node == nil {
        return nil
    }
    if node == p || node == q {
        return node
    }
    left := find(node.Left, p, q)
    right := find(node.Right, p, q)
    if left == nil && right == nil {
        return nil
    }
    if left != nil && right == nil {
        return left
    }
    if left == nil && right != nil {
        return right
    }
    return node
}
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    return find(root, p, q)
}
```
