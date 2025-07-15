# Intuition

The idea is to separate the nodes by their position index (odd or even), **not by their values**.

* Group all **odd-positioned nodes** together (1st, 3rd, 5th, ...),
* Then append all **even-positioned nodes** (2nd, 4th, 6th, ...) to the end,
* Do it **in-place** (without using extra space for new lists or arrays),
* Maintain the **relative order** of nodes within both odd and even groups.

---

# Approach

1. **Edge case check**:
   * If the list is empty or has only one node, return it as is.

2. **Initialize pointers**:
   * `odd` pointer to track the head of the odd-positioned list.
   * `even` pointer to track the head of the even-positioned list.
   * `evenHead` to keep reference to the starting point of even list (for attaching later).

3. **Traverse and rewire**:
   * Move odd pointer by skipping the next (even) node.
   * Move even pointer by skipping the next (odd) node.
   * This maintains relative order of nodes and rewires the list in-place.

4. **Connect last node of odd list to head of even list**.

---

# Complexity Analysis

* **Time Complexity**: `O(N)` — Traverse the list once.
* **Space Complexity**: `O(1)` — No extra space used, rearrangement is in-place.

---

# Code

```java
class Node {
    public int value;
    public Node next;

    Node(int value) {
        this.value = value;
        this.next = null;
    }
}

public class Solution {
    public static Node oddEvenLinkedList(Node head) {
        if (head == null || head.next == null) return head;

        Node odd = head;
        Node even = head.next;
        Node evenHead = even;

        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;

            even.next = odd.next;
            even = even.next;
        }

        odd.next = evenHead;
        return head;
    }
};

```

---

### Example Walkthrough

#### Input:

`1 -> 2 -> 3 -> -4 -> 5 -> 6`

* Odd positions: 1, 3, 5 → `1 -> 3 -> 5`
* Even positions: 2, -4, 6 → `2 -> -4 -> 6`

#### Output:

`1 -> 3 -> 5 -> 2 -> -4 -> 6`

---
