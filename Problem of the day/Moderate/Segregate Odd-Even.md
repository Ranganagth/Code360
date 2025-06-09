# Solution

```java
/*************************************************************

	Following is the class structure of the Node class:

    class Node {
		public int data;
		public Node next;

		Node(int data) {
			this.data = data;
			this.next = null;
	  	}
    }

*************************************************************/

public class Solution {
	public static Node segregateOddEven(Node head) {
		if (head == null || head.next == null) {
			return head;
		}

		Node oddHead = null, oddTail = null;
		Node evenHead = null, evenTail = null;

		Node current = head;

		while (current != null) {
			if (current.data % 2 == 0) {
				if (evenHead == null) {
					evenHead = current;
					evenTail = current;
				} else {
					evenTail.next = current;
					evenTail = evenTail.next;
				}
			} else {
				if(oddHead == null) {
					oddHead = current;
					oddTail = current;
				} else {
					oddTail.next = current;
					oddTail = oddTail.next;
				}
			}
			current = current.next;
		}

		if(oddHead == null) {
			return evenHead;
		}
		if(evenHead == null) {
			return oddHead;
		}

		oddTail.next = evenHead;
		evenTail.next = null;

		return oddHead;
	}
}

```