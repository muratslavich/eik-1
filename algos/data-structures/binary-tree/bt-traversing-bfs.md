---
description: Breadth-First Search
---

# BT traversing BFS

![](<../../.gitbook/assets/image (6).png>)

```
levelOrder(TreeNode root) {
    if (root == null) return
    
    Queue<TreeNode> queue = new LinkedList<>()
    queue.add(root)
    
    while (queue.isEmpty() == false) {
        TreeNode curr = queue.poll()
        process(curr.data)
        
        if (curr.left != null) queue.add(curr.left)
        if (curr.right != null) queue.add(curr.right)
    }
}
```

```
recursiveLevelOrder(TreeNode root) {
    int h = height(root)
    for (int l=0; l<h; l++) {
        processLevel(root, l)
    }
}

processLevel(TreeNode root, int l) {
    if (root==null) return
    if (l==0) process(root.data)
    else if (l>0) {
        processLevel(root.left, l-1)
        processLevel(root.right, l-1)
    }
}

int height(TreeNode root) {
    if (root==null) return 0
    else {
        int leftHeight = height(root.left)
        int rightHeight = height(root.height)
        
        if (leftHeight > rightHeight) return leftHeight+1
        else return rightHeight+1
    }
}
```
