> [Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.](https://oj.leetcode.com/problems/triangle/)

> For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]	
[
	[2],
	[3,4],
	[6,5,7],
	[4,1,8,3]
]
```

思路：  
很典型了一道动态规划题，相当于半个 Matrix DP。拿到这道题，先左对其，然后按照matrix DP 找坐标规律。  
Matrix DP 的常规套路

```
state： dp[i][j] 表示在从（x,y）到（i，j）位置上的最小和sum的值。
Function： 一般Matrix DP 可以从上往下也可以从下往上推，这里我们采用从下往上推   
dp[i][j] = Math.min(dp[i+1][j], dp[i+1][j+1]) + a[i][j]
起点：[a.length - 1][a[0].length - 1], 最右边那个点
Answer：[0][0]  
```

常规矩阵DP的解法： 

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle == null || triangle.size() == 0) {
            return 0;
        }
        int row = triangle.size();
        int[][] dp = new int[row][triangle.get(row - 1).size()];
        //initialize the last row
        for (int i = 0; i < triangle.get(row - 1).size(); i++) {
            dp[row - 1][i] = triangle.get(row - 1).get(i);
        }
        //transit function is 
        //dp[i][j] = Math.min(dp[i+1][j], dp[i+1][j+1]) + a[i][j]
        for (int i = row - 2; i >= 0; i--) {
            for (int j = triangle.get(i).size() - 1; j >= 0; j--) {
                dp[i][j] = Math.min(dp[i + 1][j], dp[i + 1][j + 1]) 
                		   + triangle.get(i).get(j);
            }
        }
        
        return dp[0][0];
    }
}
```

优化空间到一维DP： 只开最后一列数组用来储存。

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if(triangle.size()==1)
            return triangle.get(0).get(0);
            
        int[] dp = new int[triangle.size()];
    
        //initial by last row 
        for (int i = 0; i < triangle.get(triangle.size() - 1).size(); i++) {
            dp[i] = triangle.get(triangle.size() - 1).get(i);
        }
     
        // iterate from last second row
        for (int i = triangle.size() - 2; i >= 0; i--) {
            for (int j = 0; j < triangle.get(i).size(); j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
            }
        }
     
        return dp[0];
    }
}
```

最优解，in place，不要额外空间，直接在原矩阵上修改。  

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        if (triangle == null || triangle.size() == 0) {
            return 0;
        }
        
        int row = triangle.size();
        for (int i = row - 2; i >= 0; i--) {
            for (int j = triangle.get(i).size() - 1; j >= 0; j--) {
                int minSum = Math.min(triangle.get(i + 1).get(j), 
                                      triangle.get(i + 1).get(j + 1)) 
                            + triangle.get(i).get(j);
                triangle.get(i).set(j, minSum);
            }
        }
        
        return triangle.get(0).get(0);
    }
}
```