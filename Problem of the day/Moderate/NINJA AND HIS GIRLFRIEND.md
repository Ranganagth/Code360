# Intuition

In a **Binary Search Tree**, inorder traversal gives **sorted order**.

Minimum absolute difference will always occur between **adjacent elements in sorted order**, not arbitrary nodes.

Thus:

* Traverse BST in inorder
* Track previous node value
* Compute difference with current node

---

# Approach

1. Perform inorder traversal (Left → Root → Right)
2. Maintain:

   * `prev` → previously visited node value
   * `minDiff` → minimum difference found so far
3. For each node:

   * Compute `current - prev`
   * Update `minDiff`
4. Return `minDiff`

---

# Complexity

* **Time complexity:**  $$O(N)$$

* **Space complexity:**  $$O(H)$$ (recursion stack, worst case (O(N)))

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {

    static Integer prev = null;
    static int minDiff = Integer.MAX_VALUE;

	public static int ninjaGf(TreeNode<Integer> root) {
        prev = null;
        minDiff = Integer.MAX_VALUE;
        inorder(root);
        return minDiff;
	}

    private static void inorder(TreeNode<Integer> node) {
        if (node == null) return;

        inorder(node.left);

        if (prev != null) {
            minDiff = Math.min(minDiff, node.data - prev);
        }
        prev = node.data;

        inorder(node.right);
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

BST:
```
   5
  / 
 3   7
/ \ / 
2  4 6  8
```

Inorder traversal:
2 → 3 → 4 → 5 → 6 → 7 → 8

Differences:
3-2 = 1
4-3 = 1
5-4 = 1
6-5 = 1
7-6 = 1
8-7 = 1

Minimum = 1

## Example 2:

BST:
1

6

Inorder:
1 → 6

Difference:
6 - 1 = 5

Answer = 5
