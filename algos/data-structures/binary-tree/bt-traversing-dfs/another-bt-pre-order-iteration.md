# Another BT pre-order iteration

We process current node first, then push the right child befor the left, to ensure that the left subtree is processed first.

```
preOrder(TreeNode root) {
    if (root==null) return
    TreeNode curr = root
    Stack<TreeNode> stack = new Stack<>()
    stack.push(curr)
    
    while (stack.notEmpty()) {
        curr = stack.pop()
        process(curr.data)
        
        if (curr.right != null) stack.push(curr.right)
        if (curr.left != null) stack.push(curr.left)
    }
}
```
