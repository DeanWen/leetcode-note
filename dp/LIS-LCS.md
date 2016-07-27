> Longest Increasing Sequence 典型一维DP    

- State: f[i]表示前i个元素最长的递增子序列是多少  
- Function: f[i] = Math.max(f[0 .. j .. i - 1]) + 1 //if num[i] > num[j];  
- Initialization: f[0] = 1;  
- Answer: f[n]  

```java
public static int lis(int[] num) {
	if (num == null || num.length == 0) {
		return 0;
	}

	int[] dp = new int[num.length];
	dp[0] = 1;
	for(int i = 1; i < num.length; i++) {
		for (int j = 0; j < i; j++) {
			//MAX(dp[0 .. i - 1])
			if (num[j] < num[i] && dp[j] + 1 > dp[i]) {
				dp[i] = dp[j] + 1;
			}
		}
	}
	return dp[num.length - 1];
}
//print the longest increasing sequence
public static int[] getLIS(int[] x) {
    int n = x.length;
    int[] len = new int[n];
    Arrays.fill(len, 1);
    int[] pred = new int[n];
    Arrays.fill(pred, -1);
    for (int i = 1; i < n; i++) {
      	for (int j = 0; j < i; j++) {
        	if (x[j] < x[i] && len[i] < len[j] + 1) {
          		len[i] = len[j] + 1;
          		pred[i] = j;
        	}
      	}
    }
    
    int bi = 0;
    for (int i = 1; i < n; i++) {
      	if (len[bi] < len[i]) {
        	bi = i;
      	}
    }
    
    int cnt = len[bi];
    int[] res = new int[cnt];
    for (int i = bi; i != -1; i = pred[i]) {
      	res[--cnt] = x[i];
    }

    return res;
  }

```

> Longest Common Sequence  典型二维DP    

```
- State: f[i][j]表示前s1前i个元素和s2前几个元素最长的公共序列是多少  
- Function: f[i][j] = f[i - 1][j - 1] + 1 //if s1[i] == s[j];  
				    = Math.max(f[i - 1][j], f[i][j - 1])  
- Initialization: f[0][j] = 0 f[i][0] = 0  
- Answer: f[m][n]  
```

```java
public class Solution {
	//测试用例
	public static void main(String[] args) {
		int[] x = {1, 5, 4, 2, 3, 7, 6, 0};
		int[] y = {2, 7, 1, 3, 5, 4, 6, 0};
		String s1 = "15423760";
		String s2 = "27135460";
		System.out.println(lcs(s1, s2));
		System.out.println(Arrays.toString(getLCS(x,y)));
	}
	
	//Longest Common Sequence
	public static int lcs(String s1, String s2) {
		int m = s1.length();
		int n = s2.length();
		int[][] dp = new int[m + 1][n + 1];
		//s2 is empty
		for (int i = 0; i <= m; i++) {
			dp[i][0] = 0;
		}
		//s1 is empty
		for (int j = 0; j <= n; j++) {
			dp[0][j] = 0;
		}
		//dp body
		for(int i = 1; i <= m; i++) {
			for(int j = 1; j <= n; j++) {
				if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
					dp[i][j] = dp[i - 1][j - 1] + 1;
				}else {
					dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
				}
			}
		}
		return dp[m][n];
	}
	
	//Print Longest Common Sequence
	public static int[] getLCS(int[] x, int[] y) {
	    int m = x.length;
	    int n = y.length;
	    int[][] lcs = new int[m + 1][n + 1];
	    for (int i = 0; i < m; i++) {
	      for (int j = 0; j < n; j++) {
	        if (x[i] == y[j]) {
	          lcs[i + 1][j + 1] = lcs[i][j] + 1;
	        } else {
	          lcs[i + 1][j + 1] = Math.max(lcs[i + 1][j], lcs[i][j + 1]);
	        }
	      }
	    }
	    int cnt = lcs[m][n];
	    int[] res = new int[cnt];
	    for (int i = m - 1, j = n - 1; i >= 0 && j >= 0; ) {
	      if (x[i] == y[j]) {
	        res[--cnt] = x[i];
	        --i;
	        --j;
	      } else if (lcs[i + 1][j] > lcs[i][j + 1]) {
	        --j;
	      } else {
	        --i;
	      }
	    }
	    return res;
	  }
}
```