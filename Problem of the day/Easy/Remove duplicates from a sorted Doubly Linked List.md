# Intuition

Since the **doubly linked list is already sorted**,
all duplicates will always be **next to each other**.

That means we can just **traverse once**,
and whenever we find consecutive nodes with the same `data`,
we skip over duplicates by adjusting pointers.

---

# Approach

1. Initialize a pointer `current = head`.
2. Traverse the list until `current` becomes `null`.
3. For each node:

   * While `current.next` is not null **and** `current.data == current.next.data`,
     remove the duplicate by updating:

     ```java
     current.next = current.next.next;
     if (current.next != null)
         current.next.prev = current;
     ```
4. Move `current` to `current.next`.
5. Return `head`.

Since each node is visited at most once, this gives **O(n)** time complexity.

---

# Complexity Analysis

* **Time:** O(n) — single traversal of the list
* **Space:** O(1) — in-place modifications

---

# Code

```java
public class Solution {
    public static Node uniqueSortedList(Node head) {
        if (head == null) return null;

        Node current = head;

        while (current != null && current.next != null) {
            if (current.data == current.next.data) {
                // Skip the duplicate node
                current.next = current.next.next;
            } else {
                current = current.next;
            }
        }

        return head;
    }
};

```

---

## Example Walkthrough

**Input:**
`1 <-> 2 <-> 2 <-> 2 <-> 3`

**Steps:**

1. `current = 1` → next is 2 → no duplicate → move ahead.
2. `current = 2` → next = 2 → remove next (skip one duplicate).
3. Again `current = 2` → next = 2 → remove next (skip another duplicate).
4. Now `current = 2`, `next = 3` → different → move ahead.
5. `current = 3` → end.

**Output:**
`1 <-> 2 <-> 3`

---

