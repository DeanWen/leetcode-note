> [N-Queens](https://oj.leetcode.com/problems/n-queens/)  
Given an integer n, return all distinct solutions to the n-queens puzzle.
Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

```
For example,
There exist two distinct solutions to the 4-queens puzzle:

[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

思路：  
把棋盘存储为一个N维数组a[N][N]，数组中第i个元素的值a[i]代表第i行的皇后位置，这样便可以把问题的空间规模压缩为一维O(N)，在判断是否冲突时也很简单，首先每行只有一个皇后，且在数组中只占据一个元素的位置，行冲突就不存在了，其次是列冲突，判断一下是否有a[i]与当前要放置皇后的列j相等即可。至于斜线冲突，通过观察可以发现所有在斜线上冲突的皇后的位置都有规律即它们所在的行列互减的绝对值相等，即| row – i | = | col – a[i] |.这样某个位置是否可以放置皇后的问题已经解决。然后运用递归的思想去寻找n个皇后的解，伪代码思想如下：

```
public void queen(int row) {
	if (n == row) {    //如果已经找到结果，则打印结果
		print_result();
	}else {
		for (col = 0 to N) { //试探第row行每一个列
			if (can_place(row, col) { 
				place(row, col);   //放置皇后
				queen(row + 1);  //继续探测下一行
			}
		}
	}
}
```

完整代码如下

```java
public class Solution {
    public List<String[]> solveNQueens(int n) {
        ArrayList<String[]> result = new ArrayList<String[]>(); 
        if (n <= 0) {
            return result;
        }
        int[] columnIndex = new int[n];
        dfs(n, 0, columnIndex, result);  
        return result; 
    }
    
    public void dfs(int n, int row, int[] columnIndex, ArrayList<String[]> result)  {
        if (row == n) {
            String[] unit = new String[n];
            for (int i = 0; i < n; i++) {
                StringBuilder s = new StringBuilder();
                for (int j = 0; j < n; j++) {
                    if (j == columnIndex[i]) {
                        s.append("Q");
                    }else {
                        s.append(".");
                    }
                }
                unit[i] = s.toString();
            }
            result.add(unit);
        }else {
            for (int i = 0; i < n; i++) {
                columnIndex[row] = i; //columnIndex[row] => (row,column)
                if (isValid(row, columnIndex)) {
                    dfs(n, row + 1, columnIndex, result);
                }
            }
        }
    }
    
    public boolean isValid(int row, int[] columnIndex) {
        for (int i = 0; i < row; i++) {
            if (columnIndex[row] == columnIndex[i] || 
                Math.abs(columnIndex[row] - columnIndex[i]) == row - i) {
                    return false;
                }
        }
        return true;
    }
}
```

相似题目[N-Queens II](https://oj.leetcode.com/problems/n-queens/) 解法完全一样，只需要返回result.size()就好了