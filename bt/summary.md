##Binary Tree Related Summary

###Traversal
-	inorder, preorder, postorder
-	morris O(1)空间
-	DFS, BFS

> BFS 常考
-	Two Queue
-	One Queue + One dummyNode
-	One Queue (利用queue.size()移动指针)
	-	e.g. Binary Tree Level Order Traversal

```java
public List<List<Integer>> bfs (TreeNode root) {
	List<List<Integer>> res = new LinkedList<List<Integer>>();
	if (root == null) {
		return res;
	}

	Queue<TreeNode> queue = new LinkedList<>();
	queue.offer(root);
	while (!queue.isEmpty()) {
		List<Integer> list = new LinkedList<>();
		int size = queue.size();//using size as stop condition
		for (int i = 0; i < size; i++) {
			TreeNode curr = queue.poll();
			list.add(curr.val);
			if (curr.left != null) {
				queue.offer(curr.left);
			}
			if (curr.right != null) {
				queue.offer(curr.right);
			}
		}
		res.add(list);
	}

	return res;
}
```

> **preOrder, inOrder, postOrder** (O(n) Time, O(n) Space)
-	考点：DFS vs Iterative
	- DFS模版
	- Iterative利用stack实现
- e.g. postOrder Iterative
	- left right root -> [all left subtree][all right substree][root];
	- 利用stack记录左孩子，因为Tree自顶向下的顺序是一定的 root-> left || right, 用stack记录所有left children，root = root.right 如果right为空，pop 栈顶的左孩子。
	- 利用linkedlist addFirst 逆序插入，从右往左看的话 等价于 root right left 变形preorder，再逆序输出结果。

```java
public List<Integer> postOrder (TreeNode root) {
	List<Integer> res = new LinkedList<>();
	Stack<TreeNode> stack = new Stack<>();
	TreeNode node = root;
	while (node != null) {
		res.addFirst(node.val);
		if (node.left != null) {
			stack.push(node.left);
		}
		node = node.right;
		if (node == null && !leftTree.isEmpty()) {
			node = stack.pop();
		}
	}
	return res; 
}
```

>**Morris Traversal**
-	 O(1)空间复杂度，即只能使用常数空间；
-	 二叉树的形状不能被破坏（中间过程允许改变其形状）
-	 [要使用O(1)空间进行遍历，最大的难点在于，遍历到子节点的时候怎样重新返回到父节点（假设节点中没有指向父节点的p指针），由于不能用栈作为辅助空间。为了解决这个问题，Morris方法用到了线索二叉树（threaded binary tree）的概念。在Morris方法中不需要为每个节点额外分配指针指向其前驱（predecessor）和后继节点（successor），只需要利用叶子节点中的左右空指针指向某种顺序遍历下的前驱节点或后继节点就可以了](http://www.cnblogs.com/AnnieKim/archive/2013/06/15/MorrisTraversal.html)

```java
/*
* 中续遍历
1. 如果当前节点的左孩子为空，则输出当前节点并将其右孩子作为当前节点。
2. 如果当前节点的左孩子不为空，在当前节点的左子树中找到当前节点在中序遍历下的前驱节点。
   a) 如果前驱节点的右孩子为空，将它的右孩子设置为当前节点。当前节点更新为当前节点的左孩子。
   b) 如果前驱节点的右孩子为当前节点，将它的右孩子重新设为空（恢复树的形状）。输出当前节点。当前节点更新为当前节点的右孩子。
3. 重复以上1、2直到当前节点为空。
*/

public List<Integer> morris (TreeNode root) {
	List<Integer> res = new LinkedList<>();
	TreeNode curr = root;
	while (curr != null) {
		if (curr.left == null) {
			res.add(curr.val);
			curr = curr.right;
		} else {
			TreeNode prev = curr.left;
			while (prev.right != null && prev.right != curr) {
				prev = prev.right;
			}
			if (prev.right == null) {//2.a)
				curr = prev.right;
			}
			if (prev.right == curr) {//2.b)
				prev.right = null;
				res.add(curr.val);
				curr = curr.right;
			}
		}	
	}
}
```

###Lowest Common Ancestor
> [Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.](http://www.lintcode.com/en/problem/lowest-common-ancestor/)


**3种情况**
- Both nodes are to the left of the tree.
- Both nodes are to the right of the tree.
- One node is on the left while the other is on the right.
- [Click here for cases like with providing parent pointer, and Binary Search Tree](http://deanwen.com/lessons/2015/01/04/LeeCode-Lowest-Common-Ancestor/)

```java
public TreeNode LCA (TreeNode node1, TreeNode node2, TreeNode root) {
	//find either one or null to stop digging
	if (root == null || root == node1 || root == node2) {
		return root;
	}

	TreeNode left = LCA(node1, node2, root.left);
	TreeNode right = LCA(node1, node2, root.right);

    //One node is on the left while the other is on the right.
	if (left != null && right != null) return root;
	//Both nodes are to the left of the tree
	if (left != null) return left;
	//Both nodes are to the right of the tree
	if (right != null) return right;
	//Only one node exist
	return null;
}
```