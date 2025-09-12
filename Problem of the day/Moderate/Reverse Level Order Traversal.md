# Intuition

Normal **level order traversal** (BFS) goes from **top to bottom** and **left to right**.
For **reverse level order traversal**, we need the opposite:

* Traverse from **bottom to top**,
* and within each level, from **right to left**.

We can achieve this by:

1. Doing a **normal BFS** but instead of left → right, we enqueue **right → left**.
2. While traversing, we push each visited node into a **stack** (or just append to a list and reverse at the end).
3. Finally, reverse the collected order.

---

# Approach

1. If the tree is empty (`root == null`), return an empty list.
2. Use a **queue** for level-order traversal.

   * Enqueue root.
   * While queue is not empty:

     * Dequeue a node.
     * Store its data in a list (or push to stack).
     * Enqueue **right child first, then left child** (so when reversed, left comes before right).
3. Finally, **reverse the list** and return it.

---

# Complexity Analysis

* **Time Complexity:** `O(N)`
  Each node is visited exactly once.
* **Space Complexity:** `O(N)`
  Queue + output storage.

---

Good catch. The discrepancy comes from **the order we are enqueuing children**.

In my earlier code, we enqueued **right first, then left**, which works if you just want bottom-to-top, but it changes the left–right order within each level.

For **reverse level order**, we actually need:

* Traverse level order normally (left → right),
* Collect all nodes,
* Then reverse at the end.

That way, the **per-level left–right order remains correct**, but when reversed, it becomes bottom-to-top as required.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> reverseLevelOrder(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            result.add(node.data);

            // enqueue left first, then right
            if (node.left != null) queue.add(node.left);
            if (node.right != null) queue.add(node.right);
        }

        Collections.reverse(result);
        return result;
    }
}
```

---

##  Example Walkthrough

Tree:

```
        1
      /   \
     2     3
    /     / \
   4     5   6
    \
     7
```

**BFS order (left → right):**

`[1, 2, 3, 4, 5, 6, 7]`

**Reverse:**

`[7, 6, 5, 4, 3, 2, 1]`

Which matches the expected output.

---
