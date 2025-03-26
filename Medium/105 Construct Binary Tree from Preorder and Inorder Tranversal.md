# 105. Construct Binary Tree from Preorder and Inorder Tranversal

## Intuition

The key insight is that we can use a stack to keep track of nodes we've seen in the preorder traversal. The inorder traversal helps us determine when we need to backtrack and create right subtrees. This approach allows us to build the tree in a single pass through the preorder array.

## Approach

1. Create a stack to keep track of nodes
2. Initialize the root node with the first element from preorder
3. For each subsequent element in preorder:
   - If the current node's value doesn't match the inorder[idx], it's a left child
   - If it matches, we need to backtrack up the stack until we find a mismatch, then create a right child
4. Use the inorder array to determine when to backtrack and create right subtrees

## Complexity

- Time complexity: O(n)
- Space complexity: O(h), h is the tree's height

## Keywords

- Binary Tree
- Stack
- Tree Construction
- Preorder Traversal
- Inorder Traversal

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
type stack []interface{}

func NewStack() *stack {
    s := make(stack, 0)
    return &s
}

func (s *stack) Len() int {
    return len(*s)
}

func (s *stack) IsEmpty() bool {
    return s.Len() == 0
}

func (s *stack) Top() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("Stack is empty.")
    }
    return (*s)[s.Len()-1], nil
}

func (s *stack) Push(item interface{}) {
    (*s) = append((*s), item)
}

func (s *stack) Pop() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("Stack is empty.")
    }
    top := (*s)[s.Len() - 1]
    (*s) = (*s)[:s.Len() - 1]
    return top, nil
}

func NewTreeNode(v int, l, r *TreeNode) *TreeNode {
    return &TreeNode{Val: v, Left: l, Right: r}
}
func buildTree(preorder []int, inorder []int) *TreeNode {
    idx, st, root := 0, NewStack(), NewTreeNode(preorder[0], nil, nil)
    st.Push(root)
    for i := 1; i < len(preorder); i += 1 {
        num := preorder[i]
        top, _ := st.Top()
        if top.(*TreeNode).Val != inorder[idx] {
            top.(*TreeNode).Left = NewTreeNode(num, nil, nil)
            st.Push(top.(*TreeNode).Left)
        } else {
            for !st.IsEmpty() {
                if tt, _ := st.Top(); tt.(*TreeNode).Val != inorder[idx] {
                    break
                }
                top, _ = st.Pop()
                idx += 1
            }
            top.(*TreeNode).Right = NewTreeNode(num, nil, nil)
            st.Push(top.(*TreeNode).Right)
        }
    }
    return root
}
```
