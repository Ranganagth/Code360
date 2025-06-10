# Intuition

In a Binary Search Tree (BST), for any node:

* All nodes in the left subtree have smaller values.
* All nodes in the right subtree have larger values.

We can leverage this property to find the **floor value** (greatest node ≤ X). The idea is to:

* Traverse the tree starting from the root.
* If the current node’s value is less than or equal to `X`, it could be a potential answer. But there might be a closer one in the right subtree.
* If the current node’s value is greater than `X`, discard the current node and move left.

---

# Approach

* Initialize a variable `floor` as `-1` or any invalid value initially.
* While traversing:

  * If `node.data <= X`, update `floor = node.data` and move right.
  * Else move left.
* At the end of traversal, `floor` holds the largest value less than or equal to `X`.

---

# Complexity

* **Time complexity:**
  $O(h)$ where $h$ is the height of the tree. In the worst case (skewed tree), it becomes $O(n)$, but for balanced trees, it's $O(\log n)$.

* **Space complexity:**
  $O(1)$ — Iterative traversal without additional data structures.

---

# Code

```java
public class Solution {

    public static int floorInBST(TreeNode<Integer> root, int X) {
        int floor = -1;
        TreeNode<Integer> current = root;

        while (current != null) {
            if (current.data == X) {
                return current.data;
            } else if (current.data < X) {
                floor = current.data;
                current = current.right;
            } else {
                current = current.left;
            }
        }

        return floor;
    }
}

```
