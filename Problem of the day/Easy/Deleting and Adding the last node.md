# Intuition:

Only the last node must move. Everything else stays in order. The last node should be detached, then re-attached as the new head. Edge cases: empty list or single node — nothing changes.

---

# Approach:

1. If `head == null` or `head.next == null`, return `head`.
2. Traverse to the end while keeping track of the previous node.
3. Detach the last node (`prev.next = null`).
4. Point last node’s `next` to the old head.
5. Return the last node as the new head.

---

# Complexity:

- **Time:** O(n)
- **Space:** O(1)

---

# Code:

```java
public class Solution {
    public static Node delAddLastNode(Node head) {
        if (head == null || head.next == null) return head;

        Node prev = null;
        Node curr = head;

        while (curr.next != null) {
            prev = curr;
            curr = curr.next;
        }

        prev.next = null;
        curr.next = head;
        return curr;
    }
};

```

---

# Example walkthrough:

**List:** 2 → 5 → 3 → 11 → 1
**Traverse to last:** `curr = 1`, `prev = 11`
**Detach:** list becomes 2 → 5 → 3 → 11
**Attach last to front:** 1 → 2 → 5 → 3 → 11
