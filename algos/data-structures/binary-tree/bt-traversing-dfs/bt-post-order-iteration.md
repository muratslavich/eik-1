# BT post-order iteration

![](<../../../.gitbook/assets/image (1).png>)

```
postOrder(TreeNode root) {
    if (root == null) return
    
    TreeNode curr = root
    Stack<TreeNode> main = new Stack<>()
    Stack<TreeNode> rights = new Stack<>();
    
    while (main.notEmpty() || curr != null) {
        if (curr != null) {
            if (curr.right != null) rights.push(curr.right)
            main.push(curr)
            curr = curr.left
        } else {
            curr = main.peek()
            if (rights.notEmpty() && rights.peek() == curr.right) {
                curr = rights.pop()
            } else {
                process(curr.data)
                main.pop()
                curr = null
            }
        }
    }
}
```
