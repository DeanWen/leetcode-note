##Permutation/Combination Related Summary

**常见题型** 
- Permutation 全排列
> 利用DFS 递归的方法去排列所有情况，时间复杂度为n!
	-  **模版程序，辅助递归树理解**
	-  **注意duplicates**

```java
/*
* No duplicates in input, like [1,2,3,4,5]
* The Overall T/C O(n!)
*/
public List<List<Integer>> permutation (int[] nums) throws Exception {
	if (nums == null || nums.length == 0) {
		throw new IllegalArgumentException("Invalid Input");
	}
	List<List<Integer>> res = new LinkedList<List<Integer>>();
	List<Integer> temp = new List<>();
	helper(res, temp, nums);
	return res;
}

private void helper (List<List<Integer>> res, List<Integer> temp, int[] nums) {
	if (temp.size() == nums.length){
		res.add(new LinkedList<Integer>(temp));
		return;
	}

	for (int i = 0; i < nums.length; i++) {
		if (temp.contains(nums[i])) {
			continue;//avoid multiple usage for same integer
		}
		temp.add(nums[i]);
		helper(res, temp, nums);
		temp.remove(temp.size() - 1);
	}
}
```
> 若有duplicates, 需要辅助数组判断每一个integer的可用性
	- **boolean[] visited**
	- **if ( visited[i] )**
	- **if ( i >0 && nums[i] == nums[i - 1] && visited[i - 1] )**

```java
private void helper (List<List<Integer>> res, List<Integer> temp, int[] nums, boolean[] visited) {
	if (temp.size() == nums.length){
		res.add(new LinkedList<Integer>(temp));
		return;
	}

	for (int i = 0; i < nums.length; i++) {
		if (visited[i] || (i > 0 && nums[i] == nums[i - 1] && visited[i - 1])) {
			continue;
		}
		visited[i] = true;
		temp.add(nums[i]);
		helper(res, temp, nums);
		temp.remove(temp.size() - 1);
		visited[i] = false;
	}
}
```

-Combination 组合问题
> 利用DFS 递归的方法去排列所有情况，时间复杂度为n!
	-  **模版程序，辅助递归树理解**
	-  **注意duplicates**

```java
/*
* No duplicates in input, like [1,2,3,4,5]
*/
private void combination (List<List<Integer>> res, List<Integer> temp, int[] nums, int index) {
	res.add(new LinkedList<>(temp));
	
	for (int i = index; i < nums.length; i++) {
		temp.add(nums[i]);
		combination(res, temp, nums, index + 1);
		temp.remove(temp.size() - 1);
	}
}

/*
* Contains duplicates in input, like [1,1,1,1,5]
*/
private void combination (List<List<Integer>> res, List<Integer> temp, int[] nums, int index) {
	res.add(new LinkedList<>(temp));
	
	for (int i = index; i < nums.length; i++) {
		if (i > 0 && i != index && nums[i] == nums[i - 1]) {
			continue;
		}
		temp.add(nums[i]);
		combination(res, temp, nums, index + 1);
		temp.remove(temp.size() - 1);
	}
}
```