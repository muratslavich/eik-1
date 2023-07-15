# Binary Tree traverse

### Pre-order

{% tabs %}
{% tab title="Recursive" %}
```
void preOrder(TreeNode node) {
    if (node==null) return;
    
    print(node.val);
    
    preOrder(node.left);
    preOrder(node.right);
}
```
{% endtab %}

{% tab title="Iteration" %}
```
public void preOrder(TreeNode root) {
    Stack<TreeNode> aux = new Stack<>();
    TreeNode cur = root;

    while (cur != null || !aux.empty()) {
        while (cur != null) {
            aux.push(cur);
            print(cur.val);
            cur = cur.left;
        }

        cur = aux.pop();
        cur = cur.right;
    }

    return res;
}
```
{% endtab %}
{% endtabs %}

### In-order

{% tabs %}
{% tab title="Recursive" %}
```
void inOrder(TreeNode node) {
    if (node==null) return;
    
    inOrder(node.left);
    print(node.val);
    inOrder(node.right);
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

### Post-order

{% tabs %}
{% tab title="Recursive" %}
```
void postOrder(TreeNode node) {
    if (node==null) return;
    
    postOrder(node.left);
    postOrder(node.right);
    
    print(node.val);
}
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}
