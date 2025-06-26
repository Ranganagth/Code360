# Intuition

* In-order traversal of a BST gives elements in **sorted (ascending) order**.
* If two nodes are swapped, this sorted order will be violated **at most twice**.
* During in-order traversal:

  * Track the first node where a violation is observed (`first`).
  * Track the second node (or last) where another violation is observed (`second`).
* After traversal, **swap the values of `first` and `second`** to fix the BST.

---

# Approach

1. Do an in-order traversal and identify the two nodes violating BST properties.
2. Swap their values to restore the correct tree.
3. (Optional) Output the tree in **level-order** format as expected.

---

# Code 

```java
import java.util.*;

class TreeNode<T> {
    public T data;
    public TreeNode<T> left;
    public TreeNode<T> right;

    TreeNode(T data) {
        this.data = data;
        left = null;
        right = null;
    }
}

public class Solution {
    static TreeNode<Integer> first = null, second = null, prev = null;

    public static void recoverTree(TreeNode<Integer> root) {
        first = second = prev = null;
        inOrder(root);
        if (first != null && second != null) {
            // Swap the values
            int temp = first.data;
            first.data = second.data;
            second.data = temp;
        }
    }

    private static void inOrder(TreeNode<Integer> root) {
        if (root == null) return;

        inOrder(root.left);

        if (prev != null && prev.data > root.data) {
            if (first == null) {
                first = prev;
            }
            second = root;
        }
        prev = root;

        inOrder(root.right);
    }
}
```

---

### **Handling Level-Order Input/Output**

If needed, you can implement helper functions like:

* `TreeNode<Integer> buildTree(List<Integer> values)` — To construct a tree from level-order input.
* `List<Integer> levelOrder(TreeNode<Integer> root)` — To output the level-order of the corrected tree.
