> [Given inorder and postorder traversal of a tree, construct the binary tree.](https://oj.leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)  

思路：这道题和[Construct Binary Tree from Inorder and Postorder Traversal]类似，几乎就是完全一样, 只是把后续遍历换成了前序。所以在前续遍历中，root永远都是第一位，所以先在前续遍历中找到root，然后再找到root在中需遍历中的位置，分成左右两边，left and right, 重复上述过程，通过前续遍历找到根节点，然后在中序遍历数据中根据根节点拆分成两个部分，同时将对应的后序遍历的数据也拆分成两个部分，重复递归，就可以得到整个二叉树了。


```java
public class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if (preorder.length != inorder.length) {
            return null;
        }
        
        return buildTreeHelper(inorder, 0, inorder.length - 1,
                               preorder, 0, preorder.length - 1);
    }
    
    private TreeNode buildTreeHelper(int[] inorder, int inStart, int inEnd,
                                     int[] preorder, int preStart, int preEnd) {
        if (inStart > inEnd) {
            return null;
        }               
        
        TreeNode root = new TreeNode(preorder[preStart]);
        int position = findRoot(inorder, inStart, inEnd, root.val);
        root.left = buildTreeHelper(inorder, inStart, position - 1,
                                    preorder, preStart + 1, preStart + position - inStart);
        root.right = buildTreeHelper(inorder, position + 1, inEnd,
                                    preorder, preEnd + position - inEnd + 1, preEnd);
                                    
        return root;
    }
    
    private int findRoot(int[] array, int start, int end, int key) {
        for (int i = start; i <= end; i++) {
            if (array[i] == key) {
                return i;
            }
        }
        return -1;
    }
}
```