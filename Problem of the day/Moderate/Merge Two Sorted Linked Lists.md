# Intuition

We are asked to merge two already **sorted linked lists** into one sorted linked list.
Since both lists are sorted individually:

* The **smallest element** between the two lists is always at the **head** of one of them.
* By comparing the heads and picking the smaller one at each step, we can build the merged list in sorted order **without needing extra sorting**.

This is similar to the **merge step** in **merge sort**.

---

# Approach

1. **Handle edge cases**:

   * If one list is null, just return the other list.

2. **Use a dummy node**:

   * Create a dummy node to make it easier to build the new list.
   * Maintain a `tail` pointer, which always points to the last node in the merged list.

3. **Traverse both lists simultaneously**:

   * Compare the current nodes of both lists (`first` and `second`).
   * Append the **smaller node** to `tail.next`.
   * Move the pointer forward in the list from which the node was taken.
   * Move the `tail` forward.

4. **Attach leftovers**:

   * Once one list is fully traversed, attach the remaining nodes of the other list.

5. **Return result**:

   * The merged list starts at `dummy.next`.

---

# Complexity Analysis

* **Time Complexity**:

  * Each node from both lists is visited **exactly once**.
  * Total nodes = `n + m`
  * **O(n + m)**

* **Space Complexity**:

  * We only use a few pointers (`dummy`, `tail`), no extra lists.
  * **O(1)** (constant auxiliary space).
  * Note: If we solved it recursively, space would be **O(n + m)** because of recursion stack.

---

# Code

```java
import java.io.*;
import java.util.* ;

/************************************************************

    Following is the linked list node structure:
    
    class LinkedListNode<T> {
        T data;
        LinkedListNode<T> next;

        public LinkedListNode(T data) {
            this.data = data;
        }
    }

************************************************************/

public class Solution {
    public static LinkedListNode<Integer> sortTwoLists(LinkedListNode<Integer> first, LinkedListNode<Integer> second) {
        if (first == null) return second;
        if (second == null) return first;

        LinkedListNode<Integer> dummy = new LinkedListNode<>(-1);
        LinkedListNode<Integer> tail = dummy;

        while (first != null && second != null) {
            if (first.data <= second.data) {
                tail.next = first;
                first = first.next;
            } else {
                tail.next = second;
                second = second.next;
            }
            tail = tail.next;
        }

        // Attach remaining nodes
        if (first != null) {
            tail.next = first;
        } else {
            tail.next = second;
        }

        return dummy.next;
    }
}

```

---

## Example Walkthrough

### Input:

```
List1 = 7 -> 8 -> NULL
List2 = 1 -> 3 -> 4 -> 6 -> NULL
```

### Step-by-Step:

1. Compare 7 and 1 → smaller is **1**. Attach `1`.
   Result = `1 ->`
   List1 = `7 -> 8`
   List2 = `3 -> 4 -> 6`

2. Compare 7 and 3 → smaller is **3**. Attach `3`.
   Result = `1 -> 3 ->`
   List1 = `7 -> 8`
   List2 = `4 -> 6`

3. Compare 7 and 4 → smaller is **4**. Attach `4`.
   Result = `1 -> 3 -> 4 ->`
   List1 = `7 -> 8`
   List2 = `6`

4. Compare 7 and 6 → smaller is **6**. Attach `6`.
   Result = `1 -> 3 -> 4 -> 6 ->`
   List1 = `7 -> 8`
   List2 = `NULL`

5. List2 is empty → attach remaining List1.
   Result = `1 -> 3 -> 4 -> 6 -> 7 -> 8 -> NULL`

---

### Final Output:

```
1 -> 3 -> 4 -> 6 -> 7 -> 8 -> NULL
```

---

This ensures a **single traversal** and keeps everything **in-place**.
