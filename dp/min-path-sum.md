> [Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path](https://oj.leetcode.com/problems/minimum-path-sum/)

思路：  
why DP？ 因为要求极值  
m*n 举证，当然是matrix DP， 

``` 
State: dp[i][j] 表示起点[0,0]到[i,j]位置上的和。
Function: dp[i] = Math.min(dp[i - 1][j], dp[i][j - 1]) + a[i][j]; 
上一步可能来自于上面，或者左边，比较这两个数的大小，取较小的，然后加上当前的值。
初始化： 起点为[0，0]，矩阵DP Awalys输出实话 [0][0... n] 和[0..n][0] 因为要 i-1 和 j－1
answer：[x,y]右下角终点。
```

```java
public class Solution {
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        int m = grid.length;
        int n = grid[0].length;
        int[][] sum = new int[m][n];

        //initialization [0,0] [0, 0..n] [0..n, 0]
        sum[0][0] = grid[0][0];
        for (int i = 1; i < m; i++) {
            sum[i][0] = sum[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < n; j++) {
            sum[0][j] = sum[0][j - 1] + grid[0][j];
        }
        //transit function
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                sum[i][j] = Math.min(sum[i - 1][j], sum[i][j - 1]) + grid[i][j];
            }
        }
        
        return sum[m - 1][n - 1];
    }
}
```