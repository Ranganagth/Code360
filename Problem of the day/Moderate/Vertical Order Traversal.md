# Intuition

Each node is positioned at coordinates `(x, y)`, where:

* Left child → `(x - 1, y - 1)`
* Right child → `(x + 1, y - 1)`

We want to traverse the tree and group nodes by their `x` coordinate (vertical level), ordering nodes **top to bottom** (larger `y` to smaller) and **left to right** in case of ties.

---

# Approach:
*Data Structure*

* **`TreeMap<Integer, ArrayList<Integer>>`** to store columns in sorted order by x-coordinate.
* **`Queue<Pair<TreeNode, Integer>>`** to perform BFS. The `Pair` contains node and its current `x` coordinate.

---
# Complexity:

* **Time:** `O(N log N)` due to `TreeMap` insertion per node.
* **Space:** `O(N)` for queue and map storage.

---

# Code:

```java
import java.util.*;

public class Solution {

    static class Pair {
        TreeNode<Integer> node;
        int x;

        Pair(TreeNode<Integer> node, int x) {
            this.node = node;
            this.x = x;
        }
    }

    public static ArrayList<Integer> verticalOrderTraversal(TreeNode<Integer> root) {
        ArrayList<Integer> result = new ArrayList<>();
        if (root == null) return result;

        // TreeMap keeps keys sorted — x-coordinates from left to right
        TreeMap<Integer, ArrayList<Integer>> columnTable = new TreeMap<>();
        Queue<Pair> queue = new LinkedList<>();

        // Start BFS with root at x = 0
        queue.offer(new Pair(root, 0));

        while (!queue.isEmpty()) {
            Pair current = queue.poll();
            TreeNode<Integer> node = current.node;
            int x = current.x;

            columnTable.putIfAbsent(x, new ArrayList<>());
            columnTable.get(x).add(node.data);

            if (node.left != null)
                queue.offer(new Pair(node.left, x - 1));
            if (node.right != null)
                queue.offer(new Pair(node.right, x + 1));
        }

        // Collect all values from TreeMap in order
        for (ArrayList<Integer> col : columnTable.values()) {
            result.addAll(col);
        }

        return result;
    }
}
```

---

### Example:

For tree:

```
    1
   / \
  2   3
 /
4
```

Input as level order: `1 2 3 4 -1 -1 -1 -1 -1`

Output: `4 2 1 3`

---
