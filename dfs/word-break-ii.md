>Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.  
>Return all such possible sentences.  
>For example, given  
>s = "catsanddog",  
>dict = ["cat", "cats", "and", "sand", "dog"].  
>A solution is ["cats and dog", "cat sand dog"]	  
>[原题链接](https://oj.leetcode.com/problems/word-break-ii/)  

	
解法1  
参照[Code Ganker](http://blog.csdn.net/linhuanmars/article/details/22452163)比较好理解
这道题是[Word Break]的求所有结的版本，[Word Break]只需要判断是存不存在就可以了，这道题还要求出所有合法的string， 一想到求所有，立马联想到DFS，排列组合啊的递归模版。这道题，首先可以用[Word Break]的结果跑一遍，如果字符串在dict中，在进行DFS暴力求解，一定程度上可以优化速度, 而且防止变态超长string 无限dfs栈溢出 
或超时的情况。目前此解法超时，leetcode不知道加了什么鬼test case

```java
public class Solution {
	/*[word break template]*/
    public boolean wordBreakCheck(String s, Set<String> dict) {
        if (s == null || s.length() == 0) {
            return false;
        }
        
        boolean[] result = new boolean[s.length() + 1];//why + 1?
        Arrays.fill(result, false);
        result[0] = true;
        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; j <= i; j++) {
                String str = s.substring(j, i + 1);
                if (result[j] && dict.contains(str)) {
                    result[i + 1] = true;
                    break;
                }
            }
        }
        return result[s.length()];
    }
    /* end [word break template]*/

    public List<String> wordBreak(String s, Set<String> dict) {
        List<String> result = new ArrayList<String>();
        if (s == null || s.length() == 0) {
            return result;
        }
        //First Check if s in the dict
        //if not don't bother to dfs
        if (workBreakCheck(s, dict)) {
            helper(s, dict, 0, "", result);
        }
        return result;
    }
    
    /* DFS Body seems like N Queen*/
    private void helper(String s, Set<String> dict, int start, String item, 
    	ArrayList<String> result) {
        if (start == s.length()) {
            result.add(item);
            return;
        }
        
        StringBuilder str = new StringBuilder();
        for (int i = start; i < s.length(); i++) {
            str.append(s.charAt(i));
            if (dict.contains(str.toString())) {
                String newItem = new String();
                if (item.length() > 0) {
                    newItem = item + " " + str.toString();
                }else {
                    newItem = str.toString();
                }
                helper(s, dict, i + 1, newItem, result);
            }
        }
    }
    /* end DFS Body seems like N Queen*/
}
```

解法2:   
参照[九章](http://www.ninechapter.com/solutions/word-break-ii/) 高大上AC版本

```java
public class Solution {
    public ArrayList<String> wordBreak(String s, Set<String> dict) {
        // Note: The Solution object is instantiated only once 
        // and is reused by each test case.
        Map<String, ArrayList<String>> map = new HashMap<String, ArrayList<String>>();
        return wordBreakHelper(s,dict,map);
    }

    public ArrayList<String> wordBreakHelper(String s, Set<String> dict, 
        Map<String, ArrayList<String>> memo){
        if(memo.containsKey(s)) return memo.get(s);
        ArrayList<String> result = new ArrayList<String>();
        int n = s.length();
        if(n <= 0) return result;
        for(int len = 1; len <= n; ++len){
            String subfix = s.substring(0,len);
            if(dict.contains(subfix)){
                if(len == n){
                    result.add(subfix);
                }else{
                    String prefix = s.substring(len);
                    ArrayList<String> tmp = wordBreakHelper(prefix, dict, memo);
                    for(String item:tmp){
                        item = subfix + " " + item;
                        result.add(item);
                    }
                }
            }
        }
        memo.put(s, result);
        return result;
    }
}
```