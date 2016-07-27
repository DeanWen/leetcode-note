> [Find the contiguous subarray within an array (containing at least one number) which has the largest sum.](https://oj.leetcode.com/problems/maximum-subarray/)

For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

思路：  
why DP？ 因为要求极值  
一维数组，当然是一维DP， 

``` 
State: dp[i] 表示前i个元素的和
Function: dp[i] = Math.max(dp[i - 1] + A[i], A[i]); 用当前数和之前的和比大小，取较大的，然后维护一个max，
always compare with current dp[i], always keep the largest;
初始化： 起点为A[0]
answer：途中max值
```

```java
public class Solution {
    public int maxSubArray(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        
        int[] dp = new int[A.length];
        dp[0] = A[0];//initialize the dp
        int max = A[0];
        for (int i = 1; i < A.length; i++) {
            dp[i] = Math.max(dp[i - 1] + A[i], A[i]);//transit function
            max = Math.max(max, dp[i]);
        }
        
        return max;
    }
}
```