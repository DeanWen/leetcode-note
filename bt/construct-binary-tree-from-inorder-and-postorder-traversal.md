> [Given inorder and postorder traversal of a tree, construct the binary tree.](https://oj.leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)  

思路：
要知道如何构建二叉树，首先我们需要知道二叉树的几种遍历方式，譬如有如下的二叉树：
```
        1
--------|-------
    2         3
----|---- ----|----
4       5 6       7
```

前序遍历 1245367  
中序遍历 4251637  
后续遍历 4526731  

可以看到，后续遍历中，root永远都是最后一位，所以先在后续遍历中找到root，然后再找到root在中需遍历中的位置，分成左右两边，left and right, 重复上述过程，通过后续遍历找到根节点，然后在中序遍历数据中根据根节点拆分成两个部分，同时将对应的后序遍历的数据也拆分成两个部分，重复递归，就可以得到整个二叉树了


```java
public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        if (inorder.length != postorder.length) {
            return null;
        }
        return buildTreeHelper(inorder, 0, inorder.length - 1, 
                               postorder, 0, postorder.length - 1);
    }
    
    private TreeNode buildTreeHelper(int[] inorder, int inStart, int inEnd, 
                     int[] postorder, int postStart, int postEnd) {
        if (inStart > inEnd) {
            return null;
        }
        
        TreeNode root = new TreeNode(postorder[postEnd]);
        int position = findRoot(inorder, inStart, inEnd, root.val);
        root.left = buildTreeHelper(inorder, inStart, position - 1, 
                    postorder, postStart, postStart + position - inStart - 1);
        root.right = buildTreeHelper(inorder, position + 1, inEnd,
                     postorder, postStart + position - inStart, postEnd - 1);
        
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