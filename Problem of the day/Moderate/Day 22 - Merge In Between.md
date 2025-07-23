# Intuition

You are given two linked lists:
* `head1`: the original list.
* `head2`: the list to be inserted.

You're asked to:
* Remove nodes **from index `x` to `y`** (both inclusive) in `head1`.
* Insert **entire `head2`** in place of those removed nodes.
* Return the **merged list head**.

---

# Approach

1. Traverse `head1` to:
   * Find `prevX`: node at index `x-1` (just before removal starts).
   * Find `afterY`: node at index `y+1` (just after removal ends).
2. Link `prevX.next` to `head2`.
3. Traverse to the end of `head2`, link its last node’s `.next` to `afterY`.
4. Return `head1` as the merged list.

---

# Complexity

* **Time**: `O(N + M)`
  * `N` for traversing `head1` up to `y+1`.
  * `M` for traversing to the end of `head2`.

* **Space**: `O(1)`
  * No extra space used apart from pointers.

---

# Code

```java
class LinkedListNode {
    public int data;
    public LinkedListNode next;

    LinkedListNode(int data) {
        this.data = data;
        this.next = null;
    }
}

public class Solution {

    public static LinkedListNode mergeInBetween(int x, int y, LinkedListNode head1, LinkedListNode head2) {
        if (head1 == null) return head2;
        if (head2 == null) return head1;

        LinkedListNode prevX = null, afterY = null;
        LinkedListNode current = head1;

        int index = 0;
        while (current != null) {
            if (index == x - 1) prevX = current;
            if (index == y + 1) {
                afterY = current;
                break;
            }
            current = current.next;
            index++;
        }

        // Cut off and insert head2
        if (prevX != null) {
            prevX.next = head2;
        }

        // Traverse to the end of head2
        LinkedListNode tail2 = head2;
        while (tail2.next != null) {
            tail2 = tail2.next;
        }

        // Connect tail of head2 to afterY
        tail2.next = afterY;

        return (x == 0) ? head2 : head1;
    }
}
```

---

### **Example Walkthrough**

#### Test Case:

```
head1: 0 → 1 → 2 → 3 → 4 → 5 → NULL
head2: 10 → 20 → 30 → NULL
x = 2, y = 4
```

**Steps**:

* `prevX` = node with value `1` (at index 1)
* `afterY` = node with value `5` (at index 5)
* Remove 2 → 3 → 4
* Attach `head2` after 1: 1 → 10 → 20 → 30
* Attach `5` after 30: 30 → 5

**Final**:

```
0 → 1 → 10 → 20 → 30 → 5 → NULL
```

---
