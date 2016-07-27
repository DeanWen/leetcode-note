##Dynamic Programming 常见类型

**面试常考DP类型**
-	Matrix DP 10%
-	Sequence 40%
-	Two Sequence 40%
-	Backpack 10%  

>Matrix DP (2维DP)
-	State: f(x, y) 从起点如何走来的(0,  0) -> (x, y)
-	Function:  研究最后一步怎么来的
-	Init：起点(0,0) 
-	Answer 终点 or 指定点
-	时间复杂度：O(n^2)
-	空间复杂度：O(n^2) 可以优化到O(n)

```java
    public int uniquePaths(int m, int n) {
        int[][] grid = new int[m][n];
        for (int i = 0; i < m; i++) {
            grid[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            grid[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                grid[i][j] = grid[i - 1][j] + grid[i][j - 1];   
            }
        }
        return grid[m - 1][n - 1];
    }
```

>Sequence DP (1维DP)
-	State: f(i) 表示前i个位置的字符
-	Function:  f(i) = f(j) j 是i前一个字符
-	Init：起点(0)
-	Answer 终点 or 指定点(n)
-	时间复杂度：O(n)
-	空间复杂度：O(n)

```java
    public int climbStairs(int n) {
        if (n <= 2) return 1;
        int[] res = new int[n + 1];//1 based index
        res[0] = 1;
        res[1] = 1;
        for (int i = 2; i <= n; i++) {
            res[i] = res[i - 1] + res[i - 2];
        }
        return res[n];
    }
```

>Two Sequence DP (2维DP)
-	State: f(i, j) 表示sequence 1的前i个字符 配上sequence 2 的前j个字符
-	Function:  研究s1[i]和s2[j]的关系
-	Init：f(i,0) 和 f (0, j) 
-	Answer f(s1.length(), s2.length())
-	时间复杂度：O(nm) n is length of s1, m is length of s2
-	空间复杂度：O(nm) 

```java
    public int minDistance(String s1, String s2) {
        int[][] res = new int[s1.length() + 1][s2.length() + 1];//1 based index
        for (int i = 0; i <= s1.length(); i++) {
            res[i][0] = i;
        }
        for (int j = 0; j <= s2.length(); j++) {
            res[0][j] = j;
        }
        for (int i = 1; i <= s1.length(); i++) {
            for (int j = 1; j <= s2.length(); j++) {
                if (s1.charAt(i - 1) != s2.charAt(j - 1)) {
                    int insert = res[i][j - 1] + 1;
                    int delete = res[i - 1][j] + 1;
                    int replace = res[i - 1][j - 1] + 1;
                    res[i][j] = Math.min(Math.min(insert, delete), replace);
                }else {
                    res[i][j] = res[i - 1][j - 1];
                }
            }
        }
        return res[s1.length()][s2.length()];
    }
```
>Backpack (背包问题)
-	State: f[i][S] 表示前i个数，取出一些物品能否组成体积和为S的背包
-	Function:  f[i][S] = f[i - 1] [S - num[i]] || f[i - 1][S]
	-	 f[i−1][S−num[i]]: 放入第i个物品，前 i−1 个物品能否取出一些体积和为 S−num[i] 的背包。
	-	 f[i−1][S]: 不放入第i个物品，前 i−1 个物品能否取出一些组成体积和为S的背包。
-	Init: f[1⋯n][0]=true; f[0][1⋯m]=false. 前1~n个物品组成体积和为0的背包始终为真，其他情况为假。
-	Answer 寻找使 f[n][S] 值为true的最大S (1≤S≤m)
-	时间复杂度：O(n)
-	空间复杂度：O(n)

#####[Given n items with size Ai, an integer m denotes the size of a backpack. How full you can fill this backpack?](http://www.lintcode.com/en/problem/backpack/)
 If we have 4 items with size [2, 3, 5, 7], the backpack size is 11, we can select [2, 3, 5], so that the max size we can fill this backpack is 10. If the backpack size is 12. we can select [2, 3, 7] so that we can fulfill the backpack.

```java
public int backpack (int[] nums, int target) {
	boolean[][] record = new boolean[nums.length + 1][target + 1];
	record[0][0] = true;
	for (int i = 1; i <= nums.length; i++) {
		record[i][0] = true;		
	}
	for (int i = 0; i <= nums.length; i++) {
		for (int sum = 0; sum <= target; sum++) {
			if (sum < nums[i]) {
				record[i][sum] = record[i - 1][sum];
			}else {
				record[i][sum] = record[i - 1][sum] || record[i - 1][sum - nums[i]];
			}
		}
	}
	for (int sum = target; sum >= 0; sum--) {
		if (record[nums.length][sum]) {
			return sum;
		}
	}
	return 0;
}
```