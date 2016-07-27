
>[Given a binary tree, return the inorder traversal of its nodes' values.](https://oj.leetcode.com/problems/binary-tree-inorder-traversal/)  

DFS  

```java
public class Solution {
    //dfs
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        dfs(root, result);
        return result;
    }

    private void dfs(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }

        dfs(root.left, result);
        result.add(root.val);
        dfs(root.right, result);
    }
}
```

非递归Iterative Method  
in order顺序是 left root right     
如果当前root不为空，压入stack，令其为左孩子，如果其左孩子为空，就pop root，输出root.val, 令其等于有孩子。  
为什么？  


```java
public class Solution {
    //iterative
    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null) {
            return result;
        }
        
        Stack<TreeNode> stack = new Stack<TreeNode>();
        while(root != null || !stack.isEmpty()) {
            if (root != null) {
                stack.push(root);
                root = root.left;
            }else {
                root = stack.pop();
                result.add(root.val);
                root = root.right;
            }
        }
        
        return result;
    }
}
```