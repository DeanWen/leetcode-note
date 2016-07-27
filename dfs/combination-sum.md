> [Combination Sum](https://oj.leetcode.com/problems/combination-sum/)  
Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T. The same repeated number may be chosen from C unlimited number of times.

Note:  
- All numbers (including target) will be positive integers.  
- Elements in a combination (a1, a2, … , ak) must be in non-descending order. (ie, a1 ≤ a2 ≤ … ≤ ak).  
- The solution set must not contain duplicate combinations.

```
For example, given candidate set 2,3,6,7 and target 7, 
A solution set is: 
[7] 
[2, 2, 3] 
```

思路：  
此题套用排列组合模版，是用DFS找解决方案问题，值得注意这道题： The same repeated number may be chosen from C unlimited number of times. 所以，每次跳进递归不用都往后挪一个，还可以利用当前的元素尝试。
同时，这道题还要判断重复解。两个方法解决

```java
 1. if(i>0 && candidates[i] == candidates[i-1])//deal with dupicate
        continue; 

 2. if(!result.contains(item)) 
        result.add(new ArrayList<Integer>(item));   
```

完整代码如下  

```java
public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> item = new ArrayList<Integer>();
        if (candidates == null || candidates.length == 0) {
            return result;
        }
        
        Arrays.sort(candidates);
        helper(result, item, candidates, target, 0);
        return result;
    }
    
    private void helper(List<List<Integer>> result, List<Integer> item, 
                        int[] candidates, int target, int start) {
        if (target < 0) {
            return;
        }
        if (target == 0) {
            result.add(new ArrayList<Integer>(item));
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            if(i > 0 && candidates[i] == candidates[i-1]) {//deal with dupicate
                continue;
            }
            item.add(candidates[i]);
            int newTarget = target - candidates[i];
            helper(result, item, candidates, newTarget, i);
            item.remove(item.size() - 1);
        }
    }
}
```

此题还有的follow up版[Combination Sum II](https://oj.leetcode.com/problems/combination-sum-ii/)要求，组合中每个元素只能用一次，就排除了之前每个元素可以无限次使用的情况。就可以参照全排列的模版。用一个visited[]去标记元素是否被使用过。

完整代码如下

```java
public class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> item = new ArrayList<Integer>();
        if (candidates == null || candidates.length == 0) {
            return result;
        }
        
        boolean visited[] = new boolean[candidates.length];
        Arrays.fill(visited, false);
        Arrays.sort(candidates);
        helper(result, item, candidates, target, 0, visited);
        return result;
    }
    
    private void helper(List<List<Integer>> result, List<Integer> item, 
                        int[] candidates, int target, int start, boolean[] visited) {
        if (target < 0) {
            return;
        }
        if (target == 0) {
            result.add(new ArrayList<Integer>(item));
            return;
        }
        
        for (int i = start; i < candidates.length; i++) {
            if (i > 0 && candidates[i] == candidates[i - 1] && visited[i - 1] == false) {
                continue;
            }
            
            visited[i] = true;
            item.add(candidates[i]);
            int newTarget = target - candidates[i];
            helper(result, item, candidates, newTarget, i + 1, visited);
            visited[i] = false;
            item.remove(item.size() - 1);
        }
    }
}
```