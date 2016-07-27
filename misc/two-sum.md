##Two Sum Related Summary

**常见题型** 
- 返回下标
> 利用HashMap实现O(n) one pass and O(n) space
	-  **注意 0/1 based index**
	-  **注意下标的前后顺序**

```java
/*
* Overall T/C O(n)
* Ovarall S/C O(n)
* Using Set instead of Map when return all combinations
*/
public int[] twoSum(int[] nums, int target) {
	Map<Integer, Integer> map = new HashMap<>();
	for (int i = 0; i < nums.length; i++) {
	    //keep the original order when putting into the map
		if(map.containsKey(target - nums[i])) {
			return new int[]{map.get(target - nums[i]), i};
		}
		map.put(nums[i], i);//0 based index
	}
	throw new IllegalArgumentException("Invalid Input");
}
```
- 返回所有组合 
> 利用Two Pointer实现O(n^k-1) time complexity and O(1) space
	-  **注意消去重复组合和不必要的遍历**
	-  **无关下标顺序，可以排序**
	-  **通解，可以延展到K－Sum**

```java
public List<List<Integer>> fourSum (int[] nums, int target) {
	if (nums == null || nums.length < 4) {
		throw new IllegalArgumentException("Invalid Input");
	}
	List<List<Integer>> res = new LinkedList<List<Integer>>();
	for (int i = 0; i < nums.length - 3; i++) { //outter loop index bound
		if (i > 0 && nums[i] == nums[i - 1]) continue;//avoid dups
		for (int j = 0; j < nums.length - 2; j++) { //inner loop index bound
			if (j > 0 && nums[j] == nums[j - 1]) continue;//avoid dups
			//now convert to basic 2Sum
			int p = j + 1;
			int q = nums.length - 1;
			while (p < q) {
				int sum = nums[i] + nums[j] + nums[p] + nums[q];
				if(sum == target) {
					res.add(Arrays.asList(new Integer[]{nums[i], nums[j], nums[p], nums[q]}));
					//avoid dups
					while(p < q && nums[p] == nums[p + 1]) p++;
					while(p < q && nums[q] == nums[q - 1]) q--;
					p++;
					q--;
				}
			}
		}else if (sum < target) {
			p++;
		}esle {
			q--;
		}
	}
	return res;
}
```
	