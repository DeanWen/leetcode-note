首先判断tree是不是BST,如果为BST，按照BST定义去搜索，如果当前遍历时，root比两个点都大，那么就往根的左边找
反之向右找，如果node1 < root < node2 那么当前点就是LCA，  
Corner case： 如果一个点为令一个点的祖先，那么这个点就是LCA  

```java
public TreeNode getLCA(TreeNode root, TreeNode node1, TreeNode node2){
	if (root == null || node1 == null || node2 == null) {
        return null;
    }

    if (root.val < node1.val && root.val < node2.val) {
        return getLCA(root.right, node1, node2);
    }else if (root.val > node1.val && root.val > node2.val) {
        return getLCA(root.left, node1, node2);
    }
    
    return root;
}
```

如果不是BST，看看提不提供parent指针。Divide & Conquer

```java
public TreeNode getLCA(TreeNode root, TreeNode node1, TreeNode node2){
    if(root == null)
        return null
    if(root == node1 || root == node2)
        return root;
    
    TreeNode left = getLCA(root.left, node1, node2);
    TreeNode right = getLCA(root.right, node1, node2);
    
    if(left != null && right != null) {
        return root;
    }

    return left != null ? left : right;
}
```

如果有parent指针，分别遍历left right to root 的路径，找到公共点

```java
private ArrayList<TreeNode> getPath2Root(TreeNode node) {
    ArrayList<TreeNode> list = new ArrayList<TreeNode>();
    while (node != null) {
        list.add(node);
        node = node.parent;
    }
    return list;
}
public TreeNode lowestCommonAncestor(TreeNode node1, TreeNode node2) {
    ArrayList<TreeNode> list1 = getPath2Root(node1);
    ArrayList<TreeNode> list2 = getPath2Root(node2);
    
    int i, j;
    for (i = list1.size() - 1, j = list2.size() - 1; i >= 0 && j >= 0; i--, j--) {
        if (list1.get(i) != list2.get(j)) {
            return list1.get(i).parent;
        }
    }
    return list1.get(i+1);
}
```