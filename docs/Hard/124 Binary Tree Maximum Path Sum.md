# 124. Binary Tree Maximum Path Sum

## Intuition

The maximum path sum in a binary tree can be found by considering all possible paths that pass through each node. For any given node, the maximum path sum could be:

1. The node's value itself
2. The node's value plus the maximum path from its left subtree
3. The node's value plus the maximum path from its right subtree
4. The node's value plus both left and right maximum paths

## Approach

We use a post-order traversal approach to solve this problem:

1. For each node, we recursively calculate the maximum path sum from its left and right subtrees
2. We update the global maximum by considering all possible combinations:
    - The current node's value alone
    - Current node + left path
    - Current node + right path
    - Current node + left path + right path
3. We return the maximum path sum that can be extended from the current node to its parent (which can only include at most one of its children)

## Complexity

- Time complexity: O(n)
- Space complexity: O(h)

## Keywords

- Binary Tree
- Post-order Traversal
- Recursion

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
func maxPathSum(root *TreeNode) int {
    ret := math.MinInt
    var postOrder func(node *TreeNode) int
    postOrder = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        left, right := postOrder(node.Left), postOrder(node.Right)
        ret = max(ret, left + right + node.Val, left + node.Val, right + node.Val, node.Val)
        return max(left + node.Val, right + node.Val, node.Val)
    }
    ret = max(ret, postOrder(root))
    return ret
}
```
