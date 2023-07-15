---
description: pre-order in-order
---

# BT iteration traversing pre-order in-order



![Each node is visited 3 times](<../../../.gitbook/assets/image (5).png>)

1. enter to the node from the top
2. back from left child
3. back from right child

When the node is visited for the first time, we _**push the node into the stack**_ (to process right subtree later) and _**go to the left subtree**_, if there's no left child, we _**pop node**_ from stack and _**go to the right child**_. _**Repeat**_ while we have nodes in the stack.&#x20;

General condition `while(currentNode != null || stack.notEmpty())`

During the iteration we have 2 cases:&#x20;

```
if(currentNode != null) {
    // push to stack for afterwhile right child processing
    stack.push(currnetNode)

    // move further to left child
    currentNode = currentNode.left
}

if(currentNode == null) { 
    // but stack still has nodes to process 
    // pop parent node from stack to process his right subtree 
    prevNode = stack.pop() 
    currentNode = prevNode.right 
}
```



### BT traversing (pre-order in-order)

```
traverse(TreeNode root) {
    if (root == null) return
    
    Stack<TreeNode> stack = new Stack<>()
    TreeNode curr = root
    TreeNode prev = null
    
    while(stack.notEmpty() || curr != null) {
        if (curr != null) {
            //pre-order process(curr.data)
            stack.push(curr)
            curr = curr.left
        } else {
            prev = stack.pop()
            //in-order process(curr.data)
            curr = prev.right
        }
    }
}
```

