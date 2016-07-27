>[Given a binary tree, return the postorder traversal of its nodes' values.](https://oj.leetcode.com/problems/binary-tree-postorder-traversal/)  

DFS  
```java
public class Solution {
    //dfs
    public List<Integer> postorderTraversal(TreeNode root) {
		List<Integer> result = new ArrayList<Integer>();
		dfs(root, result);
		return result;
	}

	private void dfs(TreeNode root, List<Integer> result) {
		if (root == null) {
			return;
		}

		dfs(root.left, result);
		dfs(root.right, result);
		result.add(root.val);
	}
}
```

非递归Iterative Method  
post order顺序是 left right root。  
利用linkedlist可以addFirst()可以逆序插入顺序，root right left 就等价于 left right root.   
为什么？因为自顶root 向下叶子节点访问顺序是固定的，即先root － > left || right 
先输出root的value，然后用一个stack来记录左孩子，然后令root为右孩子，如果右孩子为空，那么再从stack 里pop()左孩子并输出。  
为什么？  因为宏观来讲， postorder的 整体顺序应该是  [all left sub tree] [all right sub tree] [root]那么循环的时候始终先  
输出root， 然后先访问右孩子，记录左孩子，当右孩子为空的，在回头找栈顶的左孩子就好了。  

```java
public class Solution {
    //iterative 
    //use linkedlist and stack
    public List<Integer> postorderTraversal(TreeNode root) {
    	LinkedList<Integer> result = new LinkedList<Integer>();
    	if (root == null) {
    		return result;
    	}

    	Stack<TreeNode> leftCollections = new Stack<TreeNode>();
    	TreeNode node = root;
    	while(node != null) {
    		result.addFirst(node.val);
    		if (node.left != null) {
    			leftCollections.push(node.left);
    		}
    		node = node.right;
    		if(node == null && !leftCollections.isEmpty()) {
    			node = leftCollections.pop();
    		}
    	}

    	return result;
    }
}
```

 
 引用自Code ganker：http://blog.csdn.net/linhuanmars/article/details/22009351

“接下来是迭代的做法，本质就是用一个栈来模拟递归的过程，但是相比于Binary Tree Inorder Traversal和Binary Tree Preorder Traversal，后序遍历的情况就复杂多了。我们需要维护当前遍历的cur指针和前一个遍历的pre指针来追溯当前的情况（注意这里是遍历的指针，并不是真正按后序访问顺序的结点）。具体分为几种情况：
（1）如果pre的左孩子或者右孩子是cur，那么说明遍历在往下走，按访问顺序继续，即如果有左孩子，则是左孩子进栈，否则如果有右孩子，则是右孩子进栈，如果左右孩子都没有，则说明该结点是叶子，可以直接访问并把结点出栈了。
（2）如果反过来，cur的左孩子是pre，则说明已经在回溯往上走了，但是我们知道后序遍历要左右孩子走完才可以访问自己，所以这里如果有右孩子还需要把右孩子进栈，否则说明已经到自己了，可以访问并且出栈了。
（3）如果cur的右孩子是pre，那么说明左右孩子都访问结束了，可以轮到自己了，访问并且出栈即可。
算法时间复杂度也是O(n)，空间复杂度是栈的大小O(logn)。实现的代码如下（代码引用自：http://www.programcreek.com/2012/12/leetcode-solution-of-iterative-binary-tree-postorder-traversal-in-java/）：” 

```java
public ArrayList<Integer> postorderTraversal(TreeNode root) {
    ArrayList<Integer> lst = new ArrayList<Integer>();

    if(root == null)
        return lst; 

    Stack<TreeNode> stack = new Stack<TreeNode>();
    stack.push(root);

    TreeNode prev = null;
    while(!stack.empty()){
        TreeNode curr = stack.peek();

        // go down the tree.
        //check if current node is leaf, if so, process it and pop stack,
        //otherwise, keep going down
        if(prev == null || prev.left == curr || prev.right == curr){
            //prev == null is the situation for the root node
            if(curr.left != null){
                stack.push(curr.left);
            }else if(curr.right != null){
                stack.push(curr.right);
            }else{
                stack.pop();
                lst.add(curr.val);
            }

        //go up the tree from left node    
        //need to check if there is a right child
        //if yes, push it to stack
        //otherwise, process parent and pop stack
        }else if(curr.left == prev){
            if(curr.right != null){
                stack.push(curr.right);
            }else{
                stack.pop();
                lst.add(curr.val);
            }

        //go up the tree from right node 
        //after coming back from right node, process parent node and pop stack. 
        }else if(curr.right == prev){
            stack.pop();
            lst.add(curr.val);
        }

        prev = curr;
    }

    return lst;
}
```