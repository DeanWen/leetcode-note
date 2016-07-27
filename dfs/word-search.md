>Given a 2D board and a word, find if the word exists in the grid.

>The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.[原题链接](https://oj.leetcode.com/problems/word-search/) 

```
For example,
Given board =

[
  ["ABCE"],
  ["SFCS"],
  ["ADEE"]
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.
```

思路：  
这道题分析看，就是一个词，在一行出现也是true，一列出现也是true，一行往下拐弯也是true，一行往上拐弯也是true，一列往左拐弯也是true，一列往右拐弯也是true。所以是要考虑到所有可能性，基本思路是使用DFS来对一个起点字母上下左右搜索，看是不是含有给定的Word。还要维护一个visited数组，表示从当前这个元素是否已经被访问过了，过了这一轮visited要回false，因为对于下一个元素，当前这个元素也应该是可以被访问的

```java
public class Solution {
    public boolean exist(char[][] board, String word) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return false;
        }
        if (word.length() == 0) {
            return true;
        }
        boolean[][] visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(board, i, j, word, 0, visited)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    private boolean dfs(char[][] board, int i, int j, 
    	String word, int start, boolean[][] visited) {
        if (start == word.length()) {
            return true;
        }
        if (i < 0 || i>= board.length || j < 0 || j >= board[0].length ) {
            return false;
        }
        if (board[i][j] != word.charAt(start)){
            return false;
	    }
	    if (visited[i][j]) {
	        return false;
	    }
	    
	    visited[i][j] = true;
	    boolean result = dfs(board, i - 1, j, word, start + 1, visited) || 
	                     dfs(board, i + 1, j, word, start + 1, visited) ||
	                     dfs(board, i, j - 1, word, start + 1, visited) ||
	                     dfs(board, i, j + 1, word, start + 1, visited);
	    visited[i][j] = false;
	    return result;
    }
}
```