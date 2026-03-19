# Intuition

The linked list represents a **binary number** from left to right.

Example:

```
1 → 0 → 1 → 0  → 1010₂ = 10
```

To convert binary to decimal efficiently:

* process from left to right
* multiply current result by 2
* add current bit

This mimics binary evaluation.

---

# Approach

1. Initialize `result = 0`.
2. Traverse the linked list:

   * `result = result * 2 + currentNode.data`
3. Return result.

---

# Complexity

* **Time complexity:** $(O(N))$
* **Space complexity:** $(O(1))$

---

# Code

```java
public class Solution {
    public static int binaryToInteger(int n, Node head) {

        int result = 0;

        Node curr = head;

        while (curr != null) {
            result = result * 2 + curr.data;
            curr = curr.next;
        }

        return result;
    }
};

```

---
# Example walkthrough

Input:

```
1 → 0 → 1
```

Steps:

```
result = 0
→ (0*2 + 1) = 1
→ (1*2 + 0) = 2
→ (2*2 + 1) = 5
```

Output:

```
5
```
