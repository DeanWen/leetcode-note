>  Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.  
For example,  
>	Given n = 3, a solution set is: "((()))", "(()())", "(())()", "()(())", "()()()"  
>  [原题链接](https://oj.leetcode.com/problems/generate-parentheses/)  

思路：  
一看到all combination 就要想到排列组合，想到DFS，先想想能不能利用排列组合的模版。其实这道题不用排列组合那个复杂，只需要利用递归模拟一下过程就可以，要点是左括号数目必须大于等于又括号的数目，因为类似“)(” 这种解是不合法的

```java
public ArrayList<String> generateParenthesis(int n) {
    ArrayList<String> result = new ArrayList<String>();
    String item = new String();
    if (n <= 0) {
        return result;
    }
    
    dfs(result, item, n, n);
    return result;
}

private void dfs (ArrayList<String> result, String item, int left, int right){
    /*Check if left */
    if (left > right) {
        return;
    }
    
    if (left == 0 && right == 0) {
        result.add(new String(item));
        return;
    }
    
    if (left > 0) {
        dfs(result,item + '(', left - 1,right);
    }
    if (right > 0) {
        dfs(result, item + ')',left, right - 1);
    }
}
```

