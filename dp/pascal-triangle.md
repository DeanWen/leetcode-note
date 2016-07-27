> [Pascal's Triangle](https://oj.leetcode.com/problems/pascals-triangle/)  
Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

思路：  
首先观察一下三角形的生成规律。
假设我们已经有了G[n-1](0) ~ G[n-1](n-2), 我们想要生成G[n](0) ~ G[n](n - 1)
那么通过观察我们可以发现如下规则：

```
G[n](0) = G[n-1](0)  = 1
G[n ](n - 1) = G[n - 1](n - 2) = 1
对于任何i 属于[1, n-2],
G[n](i) = G[n - 1](i － 1) + G[n - 1](i)。
```

完整代码如下：
```java
public class Solution {
    public ArrayList<ArrayList<Integer>> generate(int numRows) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (numRows < 1) {
            return result;
        }
        
        for (int i = 0; i < numRows; i++) {
            ArrayList<Integer> temp = new ArrayList<Integer>();
            temp.add(1);
            if (i > 0) {
                for (int j = 0; j < result.get(i - 1).size() - 1; j++) {
                    temp.add(result.get(i - 1).get(j) + result.get(i - 1).get(j + 1));
                }
                temp.add(1);
            }
            result.add(temp);
        }
        
        return result;
    }
}
```

相似问题[Pascal's Triangle II](https://oj.leetcode.com/problems/pascals-triangle-ii/)给定K， 只需要求出第kth行的内容。

```java
public class Solution {
    public ArrayList<Integer> getRow(int rowIndex) {
        if (rowIndex < 0) {
            return null;
        }
        
        int[] array = new int[rowIndex + 1];
        for (int i = 0; i < rowIndex + 1; i++) {
            for (int j = i; j >= 0; j--) {
                if (j == 0 || j == i) {
                    array[j] = 1;
                }else {
                    array[j] += array[j - 1];
                }
            }
        }
        
        //convert result to list 
        ArrayList<Integer> result = new ArrayList<Integer>();
        for (int i = 0 ; i < array.length; i++) {
            result.add(array[i]);
        }
        return result;
    }
}
```