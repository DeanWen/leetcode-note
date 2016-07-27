> [Binary Tree Level Order Traversal](https://oj.leetcode.com/problems/binary-tree-level-order-traversal/)

Binary Tree Level Order Traversal 的DFS递归模版

```java
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        travel(root, result, 0);
        return result;
    }
    
    private void travel(TreeNode curr, List<List<Integer>> result, int level) {
        if (curr == null) {
            return;
        }
        
        if (result.size() <= level) {
            List<Integer> newLevel = new LinkedList<Integer>();
            result.add(newLevel);
        }
        List<Integer> collection = result.get(level);
        collection.add(curr.val);
         
        travel(curr.left, result, level + 1);
        travel(curr.right, result, level + 1);
    }
}
```

Binary Tree Level Order Traversal 的Iterator模版
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    List<TreeNode> level = new ArrayList<TreeNode>();
    level.add(root);
    while(true){
        if (level.isEmpty() || level.get(0) == null){
            break;
        }
        List<TreeNode> nextLevel = new ArrayList<TreeNode>();
        List<Integer> currentLevel = new ArrayList<Integer>();

        for (TreeNode node : level){
            currentLevel.add(node.val);
            if (node.left != null) nextLevel.add(node.left);
            if (node.right != null) nextLevel.add(node.right);
        }
        result.add(currentLevel);
        level = nextLevel;
    }
    return result;
}
```
