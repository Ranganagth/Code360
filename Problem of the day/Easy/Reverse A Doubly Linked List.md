# Intuition

A doubly linked list already gives direct backward links (`prev`).
Reversing it requires swapping each node’s `next` and `prev`.
Once this is done for every node, the **last processed node** becomes the new head.

---

# Approach

1. Traverse the list from head.
2. For each node:

   * Swap `next` and `prev`.
3. Keep moving to the new `prev` (because pointers are swapped).
4. Track the last node visited; that node becomes the new head.

This performs the reversal in-place with O(1) extra space.

---

# Complexity

* **Time complexity:** O(N)
* **Space complexity:** O(1)

---

# Code

```java
public class Solution {
    public static Node reverseDLL(Node head) {
        if (head == null) return head;

        Node curr = head;
        Node newHead = null;

        while (curr != null) {
            // Swap next and prev
            Node temp = curr.next;
            curr.next = curr.prev;
            curr.prev = temp;

            // Move in reversed direction
            newHead = curr;
            curr = curr.prev;
        }

        return newHead;
    }
}
```

---

# Example walkthrough with explanation

Input list:
`4 <-> 3 <-> 2 <-> 1`

Step-by-step:

* For node 4: swap → `prev = 3`, `next = null`
* For node 3: swap → `prev = 2`, `next = 4`
* For node 2: swap → `prev = 1`, `next = 3`
* For node 1: swap → `prev = null`, `next = 2`

Final reversed list:
`1 <-> 2 <-> 3 <-> 4`
