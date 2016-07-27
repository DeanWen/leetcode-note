> Reverse Linked List Template

```java
	public static ListNode reverse(ListNode head) {
		if (head == null) {
			return null;
		}

		ListNode pre = null;
		ListNode curr = head;
		while(curr != null) {
			ListNode temp = curr.next;
			curr.next = pre;
			pre = curr;
			curr = temp;
		}

		return pre;
	}
```

> Find Middle Node Template

```java
	public static ListNode findMidNode(ListNode head) {
		if (head == null) {
			return null;
		}

		ListNode slow = head;
		ListNode fast = head;

		//fast.next.next == null 偶数，从slow.next断开，均分
		//fast.next == null 奇数，slow为最中间元素
		while(fast.next != null && fast.next.next != null) {
			fast = fast.next.next;
			slow = slow.next;
		}

		return slow.next;
	}
```


> In-Place Merge Template

```java
	public static ListNode inplaceMerge(ListNode head1, ListNode head2) {
		ListNode dummy = new ListNode(0);
		dummy.next = head1;
		if (head1 == null && head2 == null) {
			return null;
		}
		ListNode temp1 = null;
		ListNode temp2 = null;
		while(head1 != null && head2 != null) {
			temp1 = head1.next;
			head1.next = head2;
			
			temp2 = head2.next;
			if (temp1 != null) {
				head2.next = temp1;
			}
			head1 = temp1;
			head2 = temp2;
		}
		return dummy.next;
	}
```


> Func main() for Testing

```java
	public static void main(String[] args) {
		ListNode l1 = new ListNode(-1);
		l1.next = new ListNode(-2);
		l1.next.next = new ListNode(-3);
		l1.next.next.next = new ListNode(-4);
		
		
		ListNode l2 = new ListNode(1);
		l2.next = new ListNode(2);
		l2.next.next = new ListNode(3);
		
		print(l1);
		print(l2);
		
		ListNode t = inplaceMerge(l1, l2);
		print(t);
		
		ListNode m = findMidNode(l1);
		print(m);
		
		ListNode r = reverse(l2);
		print(r);		
	}
```
