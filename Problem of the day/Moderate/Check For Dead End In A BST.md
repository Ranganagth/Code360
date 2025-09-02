# Intuition

In a BST, a **dead end** occurs when a leaf node cannot accommodate any further insertions.
For a leaf node with value `x`:

* If the **range of possible values** allowed at that position is only `{x}`, then it’s a dead end.
* For example, if node `11` has a left bound `11` and right bound `11`, there’s no valid number to insert below it.

So the problem reduces to tracking the **valid range `[min, max]`** for each node as we traverse the BST.

---

# Approach

1. Start with the whole valid range for positive integers: `[1, ∞]`.
2. Traverse the BST recursively:

   * For a node with value `x`, its valid range is `[min, max]`.
   * If `min == max`, then this node is a **dead end**, return `true`.
   * Otherwise:

     * Recurse into the left subtree with updated range `[min, x - 1]`.
     * Recurse into the right subtree with updated range `[x + 1, max]`.
3. If any recursive call finds a dead end, return `true`.
4. If traversal finishes with no dead ends, return `false`.

This works in **O(N) time** since each node is visited once, and **O(H) space** due to recursion stack (`H = tree height`).

---

# Complexity

* **Time Complexity:** `O(N)` (visiting each node once).
* **Space Complexity:** `O(H)` (recursion depth).

---

# Code

```java
import java.util.* ;
import java.io.*; 

class TreeNode<T> {
    T data;
    TreeNode<T> left, right;

    TreeNode(T data) {
        this.data = data;
        left = null;
        right = null;
    }
};

public class Solution {
    public static Boolean isDeadEnd(TreeNode<Integer> root) {
        return helper(root, 1, Integer.MAX_VALUE);
    }

    private static boolean helper(TreeNode<Integer> node, int min, int max) {
        if (node == null) return false;

        // Dead end condition: no number can be inserted
        if (min == max) return true;

        // Check left and right subtrees
        return helper(node.left, min, node.data - 1) ||
               helper(node.right, node.data + 1, max);
    }
}
```

---

# Example Walkthrough

**Input:**

```
10 6 12 2 8 11 15 -1 -1 -1 -1 -1 -1 -1 -1
```

BST looks like:

```
        10
      /    \
     6      12
    / \    /  \
   2   8  11  15
```

* Start with root = 10, range = `[1, ∞]`.
* Node 11 has range `[11, 11]`.
* Since `min == max`, this is a **dead end**.

**Output:**

```
True
```

---
