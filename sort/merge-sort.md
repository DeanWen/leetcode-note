##MergeSort List Related Summary

**常见题型** 
- merge sorted linked list in aces/decs order
> 考点：mergesort的merge部分
	-  **注意 是否有duplicates**

``` java
public ListNode merge (ListNode l1, ListNode l2) {
    ListNode curr = new ListNode(0);
    ListNode dummy = curr;
    while (l1 != null && l2 != null){
    	if (l1.val == l2.val) {//remove dups
    		curr.next = l1;
    		l1 = l1.next;
    		l2 = l2.next;
    	}else if (l1.val < l2.val) {
            curr.next = l1;
            l1 = l1.next;
        }else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    
    if (l1 != null) curr.next = l1;
    if (l2 != null) curr.next = l2;
    
    return dummy.next;
}
```
- merge unsorted linked list in aces/decs order
> 考点：merge sort
	-  **利用快慢指针找中点，分别归并排左右**

``` java
public ListNode mergeSort (ListNode l1, ListNode l2) {
	return merge(sort(l1), sort(l2));
}

public ListNode sort (ListNode head) {
	if (head == null || head.next == null) {
		return head;
	}

	ListNode fast = head;
	ListNode slow = head;
	while (fast.next != null && fast.next.next != null) {
		slow = slow.next;
		fast = fast.next.next;
	}
	ListNode left = head;
	ListNode right = slow.next;
	slow.next = null;

	return merge(sort(left), sort(right));
}
```

- merge K sorted linked list
> 考点：递归＋merge
	-  **2分的思想，每次递归取一半, 需要logN 次操作，N 为lists size**

``` java
	/*
	* Overall T/C: O(nlogk) n is the sum of l1 + l2, k is the size of lists
	*/
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        if (lists.length == 1) return lists[0];
        if (lists.length == 2) return merge(lists[0], lists[1]);
        //Arrays.copyOfRange [startIndex, endIndex)
        return merge(mergeKLists(Arrays.copyOfRange(lists, 0, lists.length / 2)), 
                     mergeKLists(Arrays.copyOfRange(lists, lists.length / 2, lists.length)));
    }
```