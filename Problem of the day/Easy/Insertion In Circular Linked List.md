# Intuition:

Circular lists preserve continuity: the last node points to the head. Inserting at index `k` means locating the node currently at position `k-1` and adjusting one pointer. Special case: inserting at index `0` requires updating the tail pointer to point to the new head.

---

# Approach:

If `k == 0`, traverse to the last node, insert new node before head, and make it new head.
Else, iterate `k-1` steps from head, insert new node after the located node.

---

# Complexity:

- **Time Complexity:** `O(k)` traversal.
- **Space Complexity:** `O(1)`.

---

# Code:

```java
class Node {
    int data;
    Node next;
    Node(int d) { data = d; next = null; }
}

public class Solution {
    public static Node insert(int k, int val, Node head) {
        Node newNode = new Node(val);

        if (head == null) {
            newNode.next = newNode;
            return newNode;
        }

        if (k == 0) {
            Node tail = head;
            while (tail.next != head) tail = tail.next;
            newNode.next = head;
            tail.next = newNode;
            return newNode;
        }

        Node curr = head;
        for (int i = 0; i < k - 1; i++) curr = curr.next;

        newNode.next = curr.next;
        curr.next = newNode;

        return head;
    }
};

```

---

# Example Walkthrough:

**Input:** k = 1, val = 5, list = 1 → 2 → 3 → 4 → back to head

Traverse `k-1 = 0` steps: position at node with value `1`.
Link new node between `1` and `2`.

**Final circular order:** `1 → 5 → 2 → 3 → 4 → back to head`.
