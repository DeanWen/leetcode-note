> [Sort a linked list in O(n log n) time using constant space complexity](https://oj.leetcode.com/problems/sort-list/)  

思路：  
首先时间复杂度为O(nlogn)的排序算法就3种， merge sort，quick sort，heap sort。这道题用merge sort比较容易想到。实质上是模拟merge sort的 routine，每次找到list的中点, 经典快慢指针找中点模版。然后分别merge sort, 过程可以套用[merge sort two list](https://oj.leetcode.com/problems/merge-two-sorted-lists/)模版。

```java
public ListNode sortList(ListNode head) {
    return mergeSort(head);
}
```

快慢指针，找中点，然后分段merge sort

```java
private ListNode mergeSort(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    //find the mid node
    ListNode fast = head;
    ListNode slow = head;
    while(fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    //mergeSort Left and right respectively
    ListNode head2 = slow.next; //mid point is slow.next
    slow.next = null;
    ListNode head1 = head;
    head1 = mergeSort(head1);
    head2 = mergeSort(head2);
    return merge(head1, head2);
}
```

merge two linked list 的经典模版

```java
private ListNode merge(ListNode head1, ListNode head2) {
    ListNode dummyNode = new ListNode(0);
    ListNode tail;
    tail = dummyNode;

    while (head1 != null && head2 != null){
        if(head1.val < head2.val){
            tail.next = head1;
            head1 = head1.next;
        }else{
            tail.next = head2;
            head2 = head2.next;
        }
        tail = tail.next;
    }
    
    if(head1 != null){
        tail.next = head1;
    }else{
        tail.next = head2;
    }
    
    return dummyNode.next;
}
```
