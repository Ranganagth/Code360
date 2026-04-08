# Intuition

Reordering is based on **position (index)**, not values.

Maintain two separate chains:

* Odd index nodes
* Even index nodes

Then connect:
odd list → even list

No new nodes required. Only pointer manipulation.

---

# Approach

1. Edge case: if list is empty or has one node → return head
2. Initialize:

   * `odd = head`
   * `even = head.next`
   * `evenHead = even` (to reconnect later)
3. Traverse:

   * Connect odd nodes: `odd.next = even.next`
   * Move odd pointer
   * Connect even nodes: `even.next = odd.next`
   * Move even pointer
4. After loop:

   * Attach even list after odd list → `odd.next = evenHead`
5. Return head

---

# Complexity

* **Time complexity:**
  $$O(n)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
public class Solution {
    public static Node oddEvenList(Node head) {

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

# Example Walkthrough

## Example 1: Step by step explanation

Input:
2 → 4 → 6 → 8 → 10

Initial:

* odd = 2
* even = 4

Step 1:

* odd.next = 6 → odd = 6
* even.next = 8 → even = 8

Step 2:

* odd.next = 10 → odd = 10
* even.next = null → even = null

Final connection:

* odd.next = evenHead (4)

Result:
2 → 6 → 10 → 4 → 8


## Example 2:

Input:
21 → 33 → 45 → 57 → 69 → 81

Odd chain: 21 → 45 → 69
Even chain: 33 → 57 → 81

Final:
21 → 45 → 69 → 33 → 57 → 81
