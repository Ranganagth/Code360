# Intuition

A cycle means:

* While traversing the linked list, you **visit the same node again**.

We need:

* **O(n)** time
* **O(1)** extra space

The best approach is **Floyd’s Cycle Detection Algorithm** (also called **Tortoise and Hare**).

## Key Idea (Floyd’s Algorithm)

Use **two pointers**:

* `slow` → moves **1 step**
* `fast` → moves **2 steps**

### What happens?

* If there is **no cycle**, `fast` will reach `null`.
* If there **is a cycle**, `fast` will eventually **meet** `slow` inside the cycle.

This works because `fast` gains one node per step over `slow` inside a loop.

---

# Algorithm

1. Initialize:

   * `slow = head`
   * `fast = head`
2. While `fast` and `fast.next` are not `null`:

   * Move `slow` by 1
   * Move `fast` by 2
   * If `slow == fast` → **cycle exists**
3. If loop ends → **no cycle**

## Correctness

* Guaranteed to detect cycles if present
* Does not modify the list
* Uses constant extra space

---

# Complexity

* **Time:** `O(n)`
* **Space:** `O(1)`

---

# Code

```java
public class Solution {

    public static boolean detectCycle(Node head) {
        if (head == null || head.next == null) {
            return false;
        }

        Node slow = head;
        Node fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;          // move 1 step
            fast = fast.next.next;    // move 2 steps

            if (slow == fast) {
                return true;           // cycle detected
            }
        }

        return false;                  // no cycle
    }
};

```

---

# Example Walkthrough

### Example 1

```
List: 1 → 2 → 3 → 4
              ↑   ↓
              ← ← ←
```

* `slow` and `fast` eventually meet → **true**

### Example 3

```
List: 5 → null
```

* `fast` reaches `null` → **false**

---

## Edge Cases Handled

* Empty list → `false`
* Single node without cycle → `false`
* Self-loop (`node.next = node`) → `true`
* Large list (up to 10⁶ nodes) → efficient

---
