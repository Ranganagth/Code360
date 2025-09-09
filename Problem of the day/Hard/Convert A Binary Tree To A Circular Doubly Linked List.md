# Intuition

We want a **circular doubly linked list** in the **inorder traversal order**.

That means:

* The **left pointer** of each node becomes the **previous node**.
* The **right pointer** of each node becomes the **next node**.
* Finally, the **head’s left** must connect to the **tail**, and the **tail’s right** must connect back to the **head**.

We can do this by performing an **inorder traversal** and linking nodes as we go.

---

# Approach

1. Perform **inorder traversal** of the binary tree.
2. Maintain two pointers:

   * `head`: first node in inorder (smallest).
   * `prev`: last visited node in inorder.
3. During traversal:

   * If `prev` is not null, link `prev.right = curr` and `curr.left = prev`.
   * If `prev` is null, it means this is the first node, so set it as `head`.
   * Move `prev = curr`.
4. After traversal, connect:

   * `head.left = prev`
   * `prev.right = head`
     to make it circular.
5. Return `head`.

This uses **no extra nodes** (just modifies pointers).

---

# Complexity

* **Time complexity:** `O(N)` (one inorder traversal of N nodes).
* **Space complexity:** `O(H)` for recursion stack, where `H` = tree height (`O(logN)` for balanced tree, `O(N)` for skewed tree).

---

# Code

```java
import java.util.* ;
import java.io.*; 

class BinaryTreeNode<T> {
    public T data;
    public BinaryTreeNode<T> left;
    public BinaryTreeNode<T> right;

    BinaryTreeNode(T data) {
        this.data = data;
        left = null;
        right = null;
    }
}

public class Solution {
    private static BinaryTreeNode<Integer> head = null;
    private static BinaryTreeNode<Integer> prev = null;

    public static BinaryTreeNode<Integer> convertInCircularDLL(BinaryTreeNode<Integer> root) {
        if (root == null) return null;

        head = null;
        prev = null;

        inorder(root);

        // connect head and tail
        head.left = prev;
        prev.right = head;

        return head;
    }

    private static void inorder(BinaryTreeNode<Integer> node) {
        if (node == null) return;

        inorder(node.left);

        if (prev == null) {
            head = node; // first node in inorder
        } else {
            prev.right = node;
            node.left = prev;
        }
        prev = node; // move prev forward

        inorder(node.right);
    }
}
```

---

# Example Walkthrough

### Example

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

* Inorder traversal: **4 → 2 → 5 → 1 → 3 → 6**
* Linking step by step:

  * 4 ↔ 2
  * 2 ↔ 5
  * 5 ↔ 1
  * 1 ↔ 3
  * 3 ↔ 6
* Finally, connect head(4) with tail(6).
* Circular DLL: `4 ↔ 2 ↔ 5 ↔ 1 ↔ 3 ↔ 6 ↔ back to 4`.

---
