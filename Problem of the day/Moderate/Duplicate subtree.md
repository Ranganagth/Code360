# Intuition

To find duplicate subtrees in a binary tree, we must identify subtrees with:

1. **Same structure**
2. **Same node values**

To detect them efficiently, we can **serialize** every subtree into a string and use a **HashMap** to count occurrences. When we see the **same serialization again**, we know we’ve found a duplicate.

---

# Approach

1. **Serialize** each subtree as a string using DFS.
2. Use a `HashMap<String, Integer>` to count occurrences of each serialization.
3. When a serialization is seen **exactly twice**, we add the root node of that subtree to the result.

---

# Complexity Analysis

* `O(N)` where `N` is the number of nodes
* Each node is visited once and serialization is built from bottom-up

---

# Code

```java
import java.util.*;

class TreeNode {
    int data;
    TreeNode left;
    TreeNode right;
    TreeNode(int data) {
        this.data = data;
        left = right = null;
    }
}

public class Solution {

    public static ArrayList<TreeNode> duplicate_subtree(TreeNode root) {
        Map<String, Integer> map = new HashMap<>();
        List<TreeNode> result = new ArrayList<>();
        serialize(root, map, result);
        return new ArrayList<>(result);
    }

    private static String serialize(TreeNode node, Map<String, Integer> map, List<TreeNode> result) {
        if (node == null) return "#";

        String left = serialize(node.left, map, result);
        String right = serialize(node.right, map, result);
        String serial = node.data + "," + left + "," + right;

        map.put(serial, map.getOrDefault(serial, 0) + 1);
        if (map.get(serial) == 2) {
            result.add(node);
        }

        return serial;
    }
}
```

---

### **Example Walkthrough**

For the input:

```
Tree: 2
     / \
    2   2
   /     \
  3       3
```

The serializations would look like:

* `3,#,#` (for both leaf nodes)
* `2,3,#,#,#` (for both middle `2`s)
* `2,2,3,#,#,#,2,#,3,#,#` (for root)

The `3,#,#` and `2,3,#,#,#` both appear twice → So `[3]` and `[2 3]` are duplicate subtrees.

---

### **To Convert Level Order Input to Tree**

If you want to test it using level order (e.g., `1 2 3 -1 -1 2 3 -1 -1 -1 -1`), let me know—I can provide the tree building utility too.
