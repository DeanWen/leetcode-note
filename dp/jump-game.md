> [Given an array of non-negative integers, you are initially positioned at the first index of the array.](https://oj.leetcode.com/problems/jump-game-ii/)

> Each element in the array represents your maximum jump length at that position.

> Your goal is to reach the last index in the minimum number of jumps.

> For example:
> Given array A = [2,3,1,1,4]

> The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index.)

思路：  
Jump Game I & II 一个是求能否走到，一个求走到的最小值。都符合动归的使用条件。但是DP在这道题的效率不高，worse case 要O(n^2)，大数据超时。先讲解动归训练思维。贪心解法O(n)

State: dp[i] 表示前i个位置需要的最小步数
Function: dp[i] = Math.min(dp[0..i-1] + 1) when A[0..i-1] + [0..i-1] >= i
起点：A[0]
结果：dp[A.length - 1];

DP解法，过不了leetcode新增大数据

```java
public class Solution {
	public int jump(int[] A) {
		if (A == null || A.length == 0) {
			return 0;
		}

		int[] dp = new int[A.length];
		dp[0] = 0;//第一个算数不需要跳
		for(int i = 1; i < A.length; i++) {
		    dp[i] = Integer.MAX_VALUE;
			for (int j = 0; j < i; j++) {
				if (j + A[j] >= i) {
					dp[i] = Math.min(dp[i], dp[j] + 1);
				}
			}
		}

		return dp[A.length - 1];
	}
}
```

贪心解法O(n)思路：  
维护一个最大可达范围edge，如果在当前index i 在edge范围内，则不需要跳，如果当前index超出最大可达范围edge，则step++， 并且更新edge 为当前可达范围current Range，并且维护实时更新一个当前可达范围current Range ＝ Math.max(currRange, i + A[i]);  

AC代码  

```java
public class Solution {
	public int jump(int[] A) {
		if (A == null || A.length == 0) {
			return 0;
		}

		int edge = 0;
		int step = 0;
		int currRange = 0;

		for (int i = 0; i < A.length; i++) {
			if (i > edge) {
				edge = currRange;
				step++;
			}
			currRange = Math.max(currRange, i + A[i]);
		}
		return step;
	}
}
```

附Jump Game I 的两种解法。分析思路同上

```java
//Greedy AC Solution
public class Solution {
    public boolean canJump(int[] A) {
        if (A == null || A.length == 0) {
            return false;
        }
        if (A.length == 1) {
            return true;
        }
        
        int max = A[0];
        for (int i = 0; i < A.length - 1; i++) {
            max = Math.max(max - 1, A[i]);
            if (max == 0) {
                return false;
            }
        }
        
        return true;
    }
}

//DP solution
public class Solution {
    public boolean canJump(int[] A) {
        if (A == null || A.length == 0) {
            return false;
        }
        if (A.length == 1) {
            return true;
        }
        
        boolean[] dp = new boolean[A.length];
        dp[0] = true;
        for (int i = 1; i < A.length; i++) {
            for (int j = 0; j < i; i++) {
            	if (dp[j] && j + A[j] >= i) {
            		dp[i] = true;
            		break;
            	}
            }
        }
        
        return dp[A.length - 1];
    }
}
```
