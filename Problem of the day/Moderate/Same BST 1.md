# Intuition

Only the set of values matters. Shape is irrelevant. A BST guarantees uniqueness. Extract all values from each tree, then compare the resulting sets.

---

# Approach

Traverse each tree with DFS. Insert values into two hash sets. Compare sizes, then compare membership.

---

# Complexity

* **Time complexity:** O(n + m)
* **Space complexity:** O(n + m)

---

# Code

```java
import java.util.*;

public class Solution {

    private static void collect(TreeNode<Integer> node, Set<Integer> set) {
        if (node == null) return;
        set.add(node.data);
        collect(node.left, set);
        collect(node.right, set);
    }

    public static Boolean checkBSTs(TreeNode<Integer> root1, TreeNode<Integer> root2) {
        Set<Integer> s1 = new HashSet<>();
        Set<Integer> s2 = new HashSet<>();

        collect(root1, s1);
        collect(root2, s2);

        if (s1.size() != s2.size()) return false;
        for (int x : s1) {
            if (!s2.contains(x)) return false;
        }
        return true;
    }
};

```

---

# Example walkthrough with explanation

For root1 containing {8,5,10,2,6,7} and root2 containing {10,5,2,8,6}:

Traversal of tree1 inserts all six elements.
Traversal of tree2 inserts five elements.
Sizes differ → sets not equal → return false.
