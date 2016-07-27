> [Binary Tree Zigzag Level Order Traversal](https://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)  

```
For example:
Given binary tree {3,9,20,#,#,15,7},
    3
   / \
  9  20
    /  \
   15   7
return its zigzag level order traversal as:
[
  [3],
  [20,9],
  [15,7]
]
```

思路：
这道题实质上还是[Binary Tree Level Order Traversal]的变种，总体思想用DFS去遍历每一层，需要注意的地方就是Zigzag 要求奇数层逆序输出，偶数层正场输出，用LinkedList来存每一层的结果，因为可以从头插入，也可以从未插入，插入复杂度为O(1)。总体复杂度为O(n)，最后在用个ArrayList存每一层的结果就好了。  

Zigzag关键要在这里判断当前层为奇数还是偶数，然后按要求输出
```java
//reference to current list, directly manipulate
List<Integer> collection  = result.get(level);
//odd reverse output, oven output normally
if (level % 2 == 0) {
    collection.add(curr.val);
}else {
    collection.add(0, curr.val);
}
```

最后AC结果
```java
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        travel(result, root, 0);
        return result;
    }
    
    private void travel(List<List<Integer>> result, TreeNode curr, int level) {
        if (curr == null) {
            return;
        }
        
        if (result.size() <= level) {
            List<Integer> newLevel = new LinkedList<Integer>();
            result.add(newLevel);
        }
        
        //reference to current list, directly manipulate
        List<Integer> collection  = result.get(level);
        //odd reverse output, oven output normally
        if (level % 2 == 0) {
            collection.add(curr.val);
        }else {
            collection.add(0, curr.val);
        }

        //dfs left and right respectively;
        travel(result, curr.left, level + 1);
        travel(result, curr.right, level + 1);
    }
}
```
