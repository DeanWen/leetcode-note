
>输入一个矩阵，从右上角开始按照斜对角线打印矩阵的值，如矩阵为：

```
1, 2,  3,  4
5, 6,  7,  8
9, 10, 11, 12
13,14, 15, 16
```
>输出：

```
4, 3, 8, 2, 7, 12, 1, 6, 11, 16, 5, 10, 15, 9, 14, 13
```
思路：
思路：
将整个输出以最长的斜对角线分为两部分：右上部分和左下部分。
右上部分：对角线的起点在第一行，列数递减，对角线上相邻元素之间横坐标和纵坐标均相差1；
左下部分：对角线的起点在第一列上，行数递减，对角线上相邻元素之间横坐标和纵坐标均相差1；
复杂度：O(n^2)

```java
 /**
 * 以对角线的方式打印n*n矩阵
 * @param data  矩阵数组
 * @param n 矩阵的维度
 */
public void print(int[][] data, int n) {
    // 打印右上部分
    for (int i = n - 1; i >= 0; i--) {
        int row = 0;
        int col = i;
        while (row < n && col < n) {
            System.out.println(data[row][col]);
            row++;
            col++;
        }
    }

    // 打印左下部分
    for (int i = 1; i < n; i++) {
        int row = i;
        int col = 0;
        while (row < n && col < n) {
            System.out.println(data[row][col]);
            row++;
            col++;
        }
    }
}}
```