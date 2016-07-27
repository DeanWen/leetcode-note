[A message containing letters from A-Z is being encoded to numbers using the following mapping:](https://oj.leetcode.com/problems/decode-ways/)
</pre>
'A' -> 1
'B' -> 2
...
'Z' -> 26
</pre>
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.

思路：  
Why DP? 因为要求total ways 方案数目是要想到用DP  [状态转移方程详细解析](http://blog.csdn.net/shiquxinkong/article/details/17918953)
<pre>
State: dp[i]表示前i个字符一共有多少个方案  
Function: dp[i] = dp[i - 1] 
                = dp[i - 1] + dp[i - 2]  
Initialization: dp[0] = 1;  
                dp[1] = 1;//当s.charAt(0) != '0'的时候；  
answer: dp[s.length()]  
</pre>

Tips: 注意s以0为base，dp以1位base

```java
public class Solution {
    public int numDecodings(String s) { 
    	if(s == null || s.length() == 0 || s.equals("0")) {
    		return 0;
    	} 

    	int[] dp = new int[s.length() + 1];
    	dp[0] = 1;
    	dp[1] = s.charAt(0) != '0' ? 1 : 0;

    	for (int i = 2; i <= s.length(); i++) {
    		if (isValid(s.substring(i - 1, i))) {
    			dp[i] = dp[i - 1];
    		}
    		if (isValid(s.substring(i - 2, i))) {
    			dp[i] += dp[i - 2];
    		}
    	}

    	return dp[s.length()];
    }

    private boolean isValid(String s) {
        if (s.charAt(0) == '0') {
            return false;
        }
    	int code = Integer.parseInt(s);
    	if (code >= 1 && code <= 26) {
    		return true;
    	}else {
    		return false;
    	}
    }
}
```