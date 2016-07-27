> Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)  

> You have the following 3 operations permitted on a word:  

> a) Insert a character  
> b) Delete a character  
> c) Replace a character     
>  [原题链接](https://oj.leetcode.com/problems/edit-distance/)  


> ###“When you see string problem that is about subsequence or matching, dynamic programming method should come to your mind naturally. ” ###  


思路：  
首先要了解什么是edit distance，from wiki  
The Levenshtein distance between "kitten" and "sitting" is 3. The minimal edit script that transforms the former into the latter is:  

kitten → sitten (substitution of "s" for "k")  
sitten → sittin (substitution of "i" for "e")  
sittin → sitting (insertion of "g" at the end).  

LCS distance(insertions and deletions only) gives a different distance and minimal edit script:  

delete k at 0  
insert s at 0  
delete e at 4  
insert i at 4  
insert g at 6  

for a total cost/distance of 5 operations.   

###DP模型###   
  
动态数组dp[word1.length+1][word2.length+1]  
dp[i][j]表示从word1前i个字符转换到word2前j个字符最少的步骤数。  
假设word1现在遍历到字符x，word2遍历到字符y（word1当前遍历到的长度为i，word2为j）。  
以下两种可能性：  

```
1. x==y，那么不用做任何编辑操作，所以dp[i][j] = dp[i-1][j-1]  

2. x != y  

   (1) 在word1插入y， 那么dp[i][j] = dp[i][j-1] + 1  

   (2) 在word1删除x， 那么dp[i][j] = dp[i-1][j] + 1  

   (3) 把word1中的x用y来替换，那么dp[i][j] = dp[i-1][j-1] + 1  

 最少的步骤就是取这三个中的最小值。  
```

最后返回 dp[word1.length+1][word2.length+1] 即可。所以在dp的2D矩阵中 

```
    k i t t e n
  0 1 2 3 4 5 6 
s 1 1 2 3 4 5 6 
i 2 2 2 3 4 5 6 
t 3 3 3 2 3 4 5 
t 4 4 4 3 2 3 4 
e 5 5 5 4 3 2 3 
n 6 6 6 5 4 3 2 
```

可以看到如果从null 变道 word1，所需要的操作就是dp[0][i] -> {0123456} 表示一次插入6个字符，同理dp[0][j]  
当遍历到word1.charAt(1)和word2.charAt(1)是，不匹配，按以上三种情况风别计算cost，取最小的赋值给dp[i][j]  

```java
public class Solution {
    public int minDistance(String word1, String word2) {
    	int len1 = word1.length();
    	int len2 = word2.length();
    	int[][] dp = new int[len1 + 1][len2 + 1];

    	for(int i = 0; i <= len1; i++) {
    		dp[i][0] = i;
    	}
    	for(int j = 0; j <= len2; j++) {
    		dp[0][j] = j;
    	}
    	
    	//iterate and check
    	for (int i = 1; i <= len1; i ++) {
    		char c1 = word1.charAt(i - 1);
    		for (int j = 1; j <= len2; j++) {
    			char c2 = word2.charAt(j - 1);
    			if (c1 == c2) {
    				dp[i][j] = dp[i - 1][j - 1];
    			}else {
    				int insert = dp[i][j - 1] + 1;
    				int delete = dp[i - 1][j] + 1;
    				int replace = dp[i - 1][j - 1] + 1;
    				int min = Math.min(insert, Math.min(delete, replace));
    				dp[i][j] = min;
     			}
    		}
    	}

    	return dp[len1][len2];
    }
}
```