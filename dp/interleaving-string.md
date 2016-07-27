> [Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.](https://oj.leetcode.com/problems/interleaving-string/) 

```
For example,
Given:
s1 = "aabcc",
s2 = "dbbca",

When s3 = "aadbbcbcac", return true.
When s3 = "aadbbbaccc", return false.  
```

> ###“When you see string problem that is about subsequence or matching, dynamic programming method should come to your mind naturally. ” ###  

思路：[讲解来自, 还有DFS版](http://blog.csdn.net/u011095253/article/details/9248073)  
```
	s1 = "aabcc"
	s2 = "dbbca"
	s3 = "aadbbcbcac"

	    d b b c a 
	  1 0 0 0 0 0 
	a 1 x x x x x  
	a 1 x x x x x  
	b 0 x x x x x  
	c 0 x x x x x  
	c 0 x x x x x  
```

dp[i][j]表示s1取前i位，s2取前j位，是否能组成s3的前i+j位
举个列子，注意左上角那一对箭头指向的格子dp[1][1], 表示s1取第1位a, s2取第1位d，是否能组成s3的前两位aa

从dp[0][1] 往下的箭头表示，s1目前取了0位，s2目前取了1位，我们添加s1的第1位，看看它是不是等于s3的第2位，( i + j 位）

从dp[1][0] 往右的箭头表示，s1目前取了1位，s2目前取了0位，我们添加s2的第1位，看看它是不是等于s3的第2位，( i + j 位)


那什么时候取True，什么时候取False呢？

False很直观，如果不等就是False了嘛。

那True呢？首先第一个条件，新添加的字符，要等于s3里面对应的位( i + j 位)，第二个条件，之前那个格子也要等于True

举个简单的例子s1 = ab, s2 = c, s3 = bbc ，假设s1已经取了2位，c还没取，此时是False（ab!=bb），我们取s2的新的一位c，即便和s3中的c相等，但是之前是False，所以这一位也是False

同理，如果s1 = ab, s2 = c, s3=abc ，同样的假设，s1取了2位，c还没取，此时是True（ab==ab），我们取s2的新的一位c,和s3中的c相等，且之前这一位就是True，此时我们可以放心置True （abc==abc）


还有一点需要注意的是，String 的index是0 base的, 我们以dp[m+1][n+1] 正序遍历字符创造的矩阵是1 base的

```java 
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1 == null || s2 == null || s3 == null) {
            return false;
        }
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        
        boolean[][] dp = new boolean[s1.length() + 1][s2.length() + 1];
        dp[0][0] = true;
        
        //check if s3 consist from s2 
        for (int i = 1; i <= s2.length(); i++) {
            if (s2.charAt(i - 1) == s3.charAt(i - 1) && dp[0][i - 1]) {
                dp[0][i] = true;
            }
        }
        //check if s3 consist from s1
        for (int i = 1; i <= s1.length(); i++) {
            if (s1.charAt(i - 1) == s3.charAt(i - 1) && dp[i - 1][0]) {
                dp[i][0] = true;
            }
        }
        //check combination
        for (int i = 1; i <= s1.length(); i++) {
            for (int j = 1; j <= s2.length(); j++) {
                if (s1.charAt(i - 1) == s3.charAt(i + j - 1) && dp[i - 1][j]) {
                    dp[i][j] = true;
                }
                if (s2.charAt(j - 1) == s3.charAt(i + j - 1) && dp[i][j - 1]) {
                    dp[i][j] = true;
                }
            }
        }
        
        return dp[s1.length()][s2.length()];
    }
}
```