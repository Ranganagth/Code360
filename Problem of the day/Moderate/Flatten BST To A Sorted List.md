# Intuition

* A BST’s **in-order traversal** (Left → Root → Right) gives values in **sorted order**.
* If we traverse in-order, and while traversing we keep linking nodes **to the right**, we can flatten the BST into a right-skewed tree.
* At each step:

  * Set `left = null`
  * Connect `right` to the next in-order node

This gives us a sorted right-skewed BST.

---

# Approach

1. Perform **in-order traversal** of BST.
2. Maintain a **previous pointer** (`prev`) to the last processed node.
3. For each node visited:

   * Set `prev.right = current`
   * Set `current.left = null`
   * Update `prev = current`
4. Use a **dummy node** to keep track of the head (first node in sorted order).

---

# Complexity

* **Time:** `O(N)` since each node is visited once.
* **Space:** `O(H)` recursion stack (where `H` = height of tree).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    static TreeNode<Integer> prev;  // keeps track of last node in traversal
    
    public static TreeNode<Integer> flatten(TreeNode<Integer> root) {
        // Dummy node to build result
        TreeNode<Integer> dummy = new TreeNode<>(-1);
        prev = dummy;
        
        inorder(root);
        
        return dummy.right; // head of the new right-skewed BST
    }
    
    private static void inorder(TreeNode<Integer> node) {
        if (node == null) return;
        
        inorder(node.left);
        
        // Process current node
        prev.right = node;
        node.left = null;
        prev = node;
        
        inorder(node.right);
    }
};

```

---

## Example Walkthrough

Input BST:

```
       10
      /  \
     6    12
    / \   / \
   2   8 11 15
```

In-order traversal: `2 → 6 → 8 → 10 → 11 → 12 → 15`

Flattened right-skewed BST:

```
2
 \
  6
   \
    8
     \
     10
       \
       11
         \
         12
           \
           15
```

Output serialization:
`2 -1 6 -1 8 -1 10 -1 11 -1 12 -1 15 -1 -1`

---
