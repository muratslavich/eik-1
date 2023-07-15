---
description: Pre-order In-order Post-order Depth First Search
---

# BT traversing DFS

```
TreeNode {
    data
    TreeNode left
    TreeNode right
}
```

### Pre-order (root-left-right)

![](<../../../.gitbook/assets/image (2).png>)

```
preOrder(TreeNode node) {
    if (node==null) return
    process(node.data)
    preOrder(node.left)
    preOrder(node.right)
}
```

### In-order (left-root-right)

![](<../../../.gitbook/assets/image (3).png>)

```
inOrder(TreeNode node) {
    if (node==null) return
    inOrder(node.left)
    process(node.data)
    inOrder(node.right)
}
```

### Post-order (left-right-root)

![](<../../../.gitbook/assets/image (4).png>)

```
postOrder(TreeNode node) {
    if (node==null) return
    postOrder(node.left)
    postOrder(node.right)
    process(node.data)
}
```
