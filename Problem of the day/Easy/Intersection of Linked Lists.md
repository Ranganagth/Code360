# Intuition

We are given **two sorted linked lists** `L1` and `L2`, and we need to form a **new linked list** that contains **only the common elements**, also in **sorted order**.

Since both lists are already sorted, we can leverage a **two-pointer traversal** approach — similar to the **merge step of merge sort**, but instead of merging, we look for **equal elements**.

---

# Approach

1. **Initialize pointers**:

   * `p1` pointing to the head of `L1`
   * `p2` pointing to the head of `L2`

2. **Initialize a dummy node**:

   * This helps to easily build the resultant linked list.
   * Maintain a `tail` pointer to attach new nodes.

3. **Traverse both lists**:

   * If `p1.data == p2.data`:
     → It’s a common element.
     → Add it to the result list and move **both** pointers forward.
   * If `p1.data < p2.data`:
     → `p1`’s value is smaller, move `p1` forward.
   * Else (`p2.data < p1.data`):
     → Move `p2` forward.

4. **Stop when either list ends**:

   * No more common elements possible.

5. **Return**:

   * `dummy.next` → head of the new intersected list.
   * If no intersection, return `null` (or `-1` in print format).

---

# Complexity Analysis

| Type                 | Complexity   | Explanation                                                                                       |
| -------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| **Time Complexity**  | **O(N + M)** | Each list is traversed once, where N and M are lengths of `L1` and `L2`.                          |
| **Space Complexity** | **O(K)**     | For the new linked list (K = number of common elements). No extra auxiliary data structures used. |

---

# Code

```java
public class Solution {

    public static Node intersect_ll(Node L1, Node L2) {
        Node dummy = new Node(-1);
        Node tail = dummy;

        Node p1 = L1, p2 = L2;

        while (p1 != null && p2 != null) {
            if (p1.data == p2.data) {
                tail.next = new Node(p1.data);
                tail = tail.next;
                p1 = p1.next;
                p2 = p2.next;
            } else if (p1.data < p2.data) {
                p1 = p1.next;
            } else {
                p2 = p2.next;
            }
        }

        // If no intersection, dummy.next will be null
        return dummy.next;
    }
};

```

---

## Example Walkthrough

**Example:**

```
L1 = 1 → 2 → 3 → 4 → 7
L2 = 2 → 4 → 6 → 7
```

| Step | p1 | p2 | Action          | Result    |
| ---- | -- | -- | --------------- | --------- |
| 1    | 1  | 2  | 1 < 2 → move p1 | —         |
| 2    | 2  | 2  | Equal → add 2   | 2         |
| 3    | 3  | 4  | 3 < 4 → move p1 | 2         |
| 4    | 4  | 4  | Equal → add 4   | 2 → 4     |
| 5    | 7  | 6  | 6 < 7 → move p2 | 2 → 4     |
| 6    | 7  | 7  | Equal → add 7   | 2 → 4 → 7 |

Final Answer: **2 → 4 → 7**

---

## Summary

* The function **creates a new list**; it does not modify the original ones.
* You can print `-1` if the returned head is `null` while printing output.
* Works efficiently even for **large inputs (10^5 nodes)**.

---
