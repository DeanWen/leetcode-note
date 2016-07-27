>  Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place？  
>  Follow up  
>  Did you use extra space?  
>  A straight forward solution using O(mn) space is probably a bad idea.  
>  A simple improvement uses O(m + n) space, but still not the best solution.  
>  Could you devise a constant space solution?   
>  [原题链接](https://oj.leetcode.com/problems/set-matrix-zeroes/)  

思路：  
using 2 boolean mark if row0 and col0 has 0, then using row0 and col0 as the marker, for other rows and cols. It reduced the extra space O(N + M), very straightforward


```java
public class Solution {
    public void setZeroes(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return;
        }
        
        int row = matrix.length;
        int col = matrix[0].length;
        boolean hasZeroFirstRow = false;
        boolean hasZeroFirstCol = false;
        
        //does first coloumns has zero;
        for (int i = 0; i < row; i++) {
            if (matrix[i][0] == 0) {
                hasZeroFirstCol = true;
                break;
            }
        }
        //does first rows has zero
        for (int i = 0; i < col; i++) {
            if (matrix[0][i] == 0) {
                hasZeroFirstRow = true;
                break;
            }
        }
        //find zeros and store its position (row, col) => (row, 0) (0, col)
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < col; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        //follow the (row, 0) (0, col) to set zero into matrix
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < col; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        //set zero for the first row
        if (hasZeroFirstRow) {
            for (int i = 0; i < col; i++) {
                matrix[0][i] = 0;
            }
        }
        //set zero for the first column
        if(hasZeroFirstCol) {
            for (int i = 0; i < row; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```