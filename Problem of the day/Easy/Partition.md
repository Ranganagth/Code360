# Intuition

Partition while **preserving relative order** → stable partition.

Use two lists:

* one for `< X`
* one for `>= X`

Then connect them.

---

# Approach

1. Create two dummy nodes:

   * `lessHead` for nodes `< X`
   * `greaterHead` for nodes `>= X`
2. Traverse original list:

   * append node to respective list
3. Connect:

   ```
   less → greater
   ```
4. Return `lessHead.next`

---

# Complexity

* **Time complexity:**
$$O(N)$$


* **Space complexity:**
$$O(1)$$


---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static Node findPartition(Node head, int X) {

        Node lessHead = new Node(0);
        Node greaterHead = new Node(0);

        Node less = lessHead;
        Node greater = greaterHead;

        Node curr = head;

        while (curr != null) {

            if (curr.data < X) {
                less.next = curr;
                less = less.next;
            } else {
                greater.next = curr;
                greater = greater.next;
            }

            curr = curr.next;
        }

        // important: terminate greater list
        greater.next = null;

        // connect lists
        less.next = greaterHead.next;

        return lessHead.next;
    }
}
```

---

# Key Insight

```
Stable partition → maintain two chains, then merge
```

---

# Example

Input:

```
5 → 4 → 7 → 1 → 2 → 3, X=4
```

Split:

```
<4  → 1 → 2 → 3
>=4 → 5 → 4 → 7
```

Result:

```
1 → 2 → 3 → 5 → 4 → 7
```
