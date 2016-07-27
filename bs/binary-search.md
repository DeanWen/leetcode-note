##Binary Search Related Summary

**常见题型** 
- Basic Binary Search (递归 vs 非递归)
> 利用数组有序的特性实现O(logn)
	-  **注意 index bound**
	-  **注意 integer overflow**

```java
public boolean binarySearch(int[] nums, int target) {
	int left = 0;
	int right = nums.length - 1;
	while (left <= right) {//make sure two pointers never cross over
		int mid = left + (right - left) / 2; //avoid integer overflow
		if (nums[mid] == target) {
			return true;
		} else if (nums[mid] > target) {
			left = mid + 1;
		} else {
			right = mid - 1;
		}
	}
	return false;
}
```
- Rotated Sorted Array 
 > 分段有序，分段搜索
	-  **注意 是否有duplicates**
		-  case: [1,1,1,1,1,5] 5 -> [1,1,5,1,1,1] 5, when  nums[left＝0] == nums[mid=3]
			-  nums[left] 到 nums[right] 全部是 1
			-  包括target在内可能还有不同的数存在于nums[left] 到 nums[right]
		- 要move left pointer one by one to check if there is different number
		- The worst case T/C O(n)

``` java
public boolean binarySearch(int[] nums, int target) {
	int left = 0;
	int right = nums.length - 1;
	while (left <= right) {//make sure two pointers never cross over
		int mid = left + (right - left) / 2; //avoid integer overflow
		if (nums[mid] == target) {
			return true;
		}
		//check if left and right part are sorted to do BS respectively
		if (nums[left] < nums[mid]) {// left is sorted
			if (nums[left] <= target && target < nums[mid]) {
				right = mid - 1;
			}else {
				left = mid + 1;
			}
		//when array has NO duplicates
		}else {//right is sorted
			if (nums[mid] < target && target <= nums[right]) {
				left = mid + 1;
			}else {
				right = mid - 1;
			}
		}
		//-----no dups ends-----
		// when array has duplicates
		} else if (nums[mid] < nums[right]){//right is sorted
			if (nums[mid] < target && target <= nums[right]) {
				left = mid + 1;
			}else {
				right = mid - 1;
			}
		} else {
			left++;
		}
		//-------dups ends-----
	}
	return false;
}
```
 > **Tips: 含有duplicates进行binary search, 要区别对待 相等(==)的case, like case [1,1,1,1,5]**

```java
if(nums[left] < nums[mid]){ 
...
} else if (nums[left] > nums[mid]){
...
} else {
	left++;
}
```