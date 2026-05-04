# Intuition

Width of a level = number of **non-null nodes between leftmost and rightmost non-null nodes**, ignoring gaps.

This reduces to:

* Count number of nodes at each level
* Take maximum

No need for index-based width (like complete tree problems), since null gaps are excluded.

---

# Approach

1. Use **level order traversal (BFS)**
2. For each level:

   * Count nodes in queue (`size`)
3. Track maximum size across levels
4. Return maximum

---

# Complexity

* **Time complexity:**
  $$O(N)$$

* **Space complexity:**
  $$O(N)$$ (worst case queue)

---

# Code

```Java
import java.util.*;

public class Solution {
    public static int getMaxWidth(TreeNode root) {

        if (root == null) return 0;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        int maxWidth = 0;

        while (!q.isEmpty()) {

            int size = q.size();
            maxWidth = Math.max(maxWidth, size);

            for (int i = 0; i < size; i++) {

                TreeNode node = q.poll();

                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
        }

        return maxWidth;
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Tree:
1
/ 
2   3
/   / 
4   5   6

7

Levels:

* Level 1 → [1] → width = 1
* Level 2 → [2,3] → width = 2
* Level 3 → [4,5,6] → width = 3
* Level 4 → [7] → width = 1

Max = 3

---

## Example 2:

Tree:
2
/ 
7   5
/ \    
2   6    9
/ 
5  11

Levels:

* Level 1 → 1
* Level 2 → 2
* Level 3 → 3
* Level 4 → 3

Max = 3
