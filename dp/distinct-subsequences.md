> Given a string S and a string T, count the number of distinct subsequences of T in S. A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).  
> Here is an example:  
> S = "rabbbit", T = "rabbit"  
> Return 3.   
> [原题链接](https://oj.leetcode.com/problems/distinct-subsequences/)  

> ###“When you see string problem that is about subsequence or matching, dynamic programming method should come to your mind naturally. ” ###  

思路：   
可以先尝试做一个二维的表int[][] dp，用来记录匹配子序列的个数（以S ="rabbbit",T = "rabbit"为例）：

```
    r a b b b i t
  1 1 1 1 1 1 1 1
r 0 1 1 1 1 1 1 1
a 0 0 1 1 1 1 1 1
b 0 0 0 1 2 3 3 3
b 0 0 0 0 1 3 3 3
i 0 0 0 0 0 0 3 3
t 0 0 0 0 0 0 0 3  
```

从这个表可以看出，无论T的字符与S的字符是否匹配，dp[i][j] = dp[i][j - 1].  
就是说，假设S已经匹配了j - 1个字符，得到匹配个数为dp[i][j - 1].  
现在无论S[j]是不是和T[i]匹配，匹配的个数至少是dp[i][j - 1]。   
除此之外，当S[j]和T[i]相等时，我们可以让S[j]和T[i]匹配，然后让S[j - 1]和T[i - 1]去匹配。  
所以递推关系为：  

```
dp[0][0] = 1; // T和S都是空串.  
dp[0][1 ... S.length() - 1] = 1; // T是空串，S只有一种子序列匹配。  
dp[1 ... T.length() - 1][0] = 0; // S是空串，T不是空串，S没有子序列匹配。  
dp[i][j] = dp[i][j - 1] + (T[i - 1] == S[j - 1] ? dp[i - 1][j - 1] : 0) 
(1 <= i <= T.length(), 1 <= j <= S.length())  
```

当i=2，j=1时，S 为 ra，T为r，T肯定是S的子串；这时i=2，j=2时，S为ra，T为rs，T现在不是S的子串，当之前一次是子串所以现在计数为1.  

同时，如果字符串S[i-1]和T[j-1]（dp是从1开始计数，字符串是从0开始计数）匹配的话，dp[i][j]还要加上dp[i-1][j-1]  
例如对于例子： S = "rabbbit", T = "rabbit"  

当i=2，j=1时，S 为 ra，T为r，T肯定是S的子串；当i=2，j=2时，S仍为ra，T为ra，这时T也是S的子串，所以子串数在dp[2][1]基础上加dp[1][1]。

```java
public class Solution {
    public int numDistinct(String S, String T) {
    	int dp[][] = new int[T.length() + 1][S.length() + 1];
    	dp[0][0] = 1;
    	
    	//T is empty, so T is subsequence of any string
    	for(int i = 1; i <= S.length(); i++) {
    		dp[0][i] = 1;
    	}
    	
    	//S is empty, nothing can be subsequence of EMPTY String;
    	for(int i = 1; i <= T.length(); i++) {
    		dp[i][0] = 0;
    	}
    	
    	//DP body
    	for (int i = 1; i <= T.length(); i++) {
    		for(int j = 1; j <= S.length(); j++) {
    			dp[i][j] = dp[i][j - 1];
    			if (T.charAt(i - 1) == S.charAt(j - 1)) {
    				dp[i][j] += dp[i - 1][j - 1];
    			}
    		}
    	}
    	
    	return dp[T.length()][S.length()];
    }
}
```
