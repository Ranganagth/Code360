# Intuition

We’re given:

* A doubly linked list (`head`).
* An index `k` (0-indexed).
* A value `val`.

We need to insert a new node with value `val` at position `k`.

Key points:

* If `k == 0`: Insert at the **head**.
* If `k == length`: Insert at the **end**.
* Otherwise: Insert at the correct middle position by traversing.

Since this is a **doubly linked list**, we must update **both `next` and `prev` pointers**.

---

# Approach

1. Create a new node with data `val`.
2. If `k == 0`:

   * New node becomes new head.
   * Update old head’s `prev` pointer.
3. Otherwise:

   * Traverse to the node at index `k-1`.
   * Insert the new node after it:

     * `newNode.next = curr.next`
     * `newNode.prev = curr`
     * If `curr.next != null`, set `curr.next.prev = newNode`
     * `curr.next = newNode`
4. Return head.

---

# Complexity

* **Time Complexity:** `O(N)` (need to traverse to index `k`).
* **Space Complexity:** `O(1)` (only extra node allocated).

---

# Code

```java
import java.util.*;

public class Solution {

    static Node insert(int k, int val, Node head) {
        Node newNode = new Node(val);

        // Case 1: insert at head
        if (k == 0) {
            newNode.next = head;
            if (head != null) {
                head.prev = newNode;
            }
            return newNode; // new node is new head
        }

        // Traverse to position k-1
        Node curr = head;
        for (int i = 0; i < k - 1 && curr != null; i++) {
            curr = curr.next;
        }

        // Insert newNode after curr
        newNode.next = curr.next;
        newNode.prev = curr;

        if (curr.next != null) {
            curr.next.prev = newNode;
        }

        curr.next = newNode;

        return head;
    }
}
```

---

## Example Walkthrough

Input:
`k = 2, val = 4`
List: `10 <-> 11 <-> 5`

Steps:

1. `head = 10`, move 1 step → at `11` (`k-1` node).
2. Insert `4` after `11`.
3. Update pointers:

   * `newNode.prev = 11`
   * `newNode.next = 5`
   * `11.next = 4`
   * `5.prev = 4`

Final List: `10 <-> 11 <-> 4 <-> 5`
Output: `10 11 4 5`

---

