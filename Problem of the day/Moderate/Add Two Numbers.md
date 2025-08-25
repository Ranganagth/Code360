# Intuition

We are given two numbers as linked lists, where digits are stored in **reverse order** (least significant digit first).
This allows us to add them the same way we do manually from right to left.

We traverse both lists simultaneously, digit by digit:

* Add corresponding digits along with any carry.
* Store the last digit of the sum in a new node.
* Update carry = sum / 10.
* Continue until both lists and carry are processed.

---

# Approach

1. Initialize a **dummy node** to simplify building the result list.
2. Use a pointer `curr` starting at dummy.
3. Maintain a `carry = 0`.
4. Traverse both lists while either is non-null or carry > 0:

   * Get values from `head1` and `head2` (0 if null).
   * Compute `sum = val1 + val2 + carry`.
   * Create a new node with `sum % 10`.
   * Update `carry = sum / 10`.
   * Move `head1`, `head2`, and `curr` forward.
5. Return `dummy.next` as the head of result.

---

# Complexity

* **Time complexity:** `O(m + n)` → We traverse both lists once.
* **Space complexity:** `O(max(m, n))` for the result list.

---

# Code

```java
public class Solution {
    static LinkedListNode addTwoNumbers(LinkedListNode head1, LinkedListNode head2) {
        LinkedListNode dummy = new LinkedListNode(0);
        LinkedListNode curr = dummy;
        int carry = 0;

        while (head1 != null || head2 != null || carry != 0) {
            int val1 = (head1 != null) ? head1.data : 0;
            int val2 = (head2 != null) ? head2.data : 0;

            int sum = val1 + val2 + carry;
            carry = sum / 10;

            curr.next = new LinkedListNode(sum % 10);
            curr = curr.next;

            if (head1 != null) head1 = head1.next;
            if (head2 != null) head2 = head2.next;
        }

        return dummy.next;
    }
}
```

---

# Example Walkthrough

### Example 1

Input:

```
num1: 1 -> 2 -> 3  (represents 321)
num2: 4 -> 5 -> 6  (represents 654)
```

Step-by-step addition:

* 1 + 4 = 5 (carry 0) → node 5
* 2 + 5 = 7 (carry 0) → node 7
* 3 + 6 = 9 (carry 0) → node 9

Output:

```
5 -> 7 -> 9   (represents 975)
```

---

### Example 2

Input:

```
num1: 2   (represents 2)
num2: 9 -> 9   (represents 99)
```

Step-by-step:

* 2 + 9 = 11 → node 1, carry 1
* 0 + 9 + 1 = 10 → node 0, carry 1
* Extra carry = 1 → node 1

Output:

```
1 -> 0 -> 1   (represents 101)
```

---

