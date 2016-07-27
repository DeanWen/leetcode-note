> Given a string s, partition s such that every substring of the partition is a palindrome.  
> Return the minimum cuts needed for a palindrome partitioning of s.  
> For example, given s = "aab",  
> Return 1 since the palindrome partitioning ["aa","b"] could be produced using 1 cut.  

[原题链接](https://oj.leetcode.com/problems/palindrome-partitioning-ii/) 

思路：  

首先设置dp变量 cuts[len+1]。  

cuts[i]表示从第i位置到第len位置（包含，即[i, len])的切割数（第len位置为空, 非空[0, len - 1])  

初始时，是len - i - 1。比如给的例子aab，cuts[0] = 2，就是最坏情况每一个字符都得切割：a\|a\|b  

cuts[1] = 1, 即从i=1位置开始，a\|b  

cuts[2] = 0, 即从i=2位置开始, b  

cuts[3] = -1,即第len位置，为空字符，不需要切割。

上面的这个cuts数组是用来帮助算最小cuts的。

还需要一个dp二维数组matrixs[i][j]表示字符串[i,j]从第i个位置（包含）到第j个位置（包含） 是否是回文。

如何判断字符串[i,j]是不是回文？

 1. matrixs[i+1][j-1]是回文且 s.charAt(i) == s.charAt(j)。

 2. i == j（i，j是用一个字符）

 3. j = i + 1（i，j相邻）且s.charAt(i) == s.charAt(j)

当字符串[i,j]是回文后，说明从第i个位置到字符串第len位置的最小cut数可以被更新了, 那么就是从j+1位置开始到第len位置的最小cut数加上 [i,j] |cut| [j+1,len - 1]中间的这一个cut，操作＋1。即，Math.min(cuts[i], cuts[j+1]+1)   
最后返回cuts[0]。即为最终结果。

```java 
public class Solution {
    public int minCut(String s) { 
        if (s == null || s.length() == 0 || s.length() == 1) {
            return 0;
        }

        int len = s.length();
        int[] cuts = new int[len + 1];//cut nums from i to len => [i,len]
        boolean[][] matrix = new boolean[len][len];//record if sub(i,j) is palindrome 
        
        //初始化cuts，从ith位置到len做多要可以切len － i － 1刀
        //e.g. s = aaaaa len = 5
        //     cut[0] = len - i - 1 = 4 刀，从0位置开始切最多切4刀
        //     a | a | a | a | a 
        for (int i = 0; i <= len; i++) {
            cuts[i] = len - i - 1;
        }
        // 两种情况：
        // 1. s.charAt(i) == s.charAt(j) && (i == j || i + 1 = j)  => j - i < 2
        // 2. s.charAt(i) == s.charAt(j) && matrix[i + 1][j - 1]
        for (int i = len - 1; i >= 0; i--) {
            for (int j = i; j < len; j++) {
                //must put j - i < 2 first, or will index will overflow
                if ((s.charAt(i) == s.charAt(j) && j - i < 2 ) || 
                    (s.charAt(i) == s.charAt(j) && matrix[i + 1][j - 1])) {
                    matrix[i][j] = true;
                    cuts[i] = Math.min(cuts[i], cuts[j + 1] + 1);
                }
            }
        }
        
        return cuts[0];
    }
}
```