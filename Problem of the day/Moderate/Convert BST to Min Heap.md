# Intuition

BST inorder traversal produces values in strictly increasing order.
A Min Heap requires the smallest value at the root and increasing values as levels go down, while preserving the complete tree structure.
Because the given BST is already complete, only node values must be reassigned.
Strategy: extract sorted values using inorder, then overwrite the tree using preorder so parents get smaller values before children.

---

# Approach

1. Perform inorder traversal of the BST and store values in a list.
   Result: sorted array of all node values.
2. Perform preorder traversal of the same tree.
3. While traversing preorder, replace each node’s value with the next value from the sorted list.
4. Return the modified root.
5. Preorder traversal of this tree now represents a Min Heap.

**Correctness Argument**
* Inorder traversal of BST yields sorted values.
* Preorder traversal assigns values top-down.
* Therefore, every parent node receives a value smaller than its children.
* Tree structure remains unchanged, so completeness is preserved.
* Hence, the result satisfies all Min Heap properties.

---
# Complexity

**Time Complexity**
* Inorder traversal: `O(N)`
* Preorder traversal: `O(N)`
* Total: `O(N)`

**Space Complexity**
* Extra list to store values: `O(N)`
* Recursion stack: `O(H)` where `H ≤ log N` for complete tree

---

# Code

```java
import java.util.*;

public class Solution {

    static int index;

    public static BinaryTreeNode convertBST(BinaryTreeNode root) {
        List<Integer> inorder = new ArrayList<>();
        storeInorder(root, inorder);
        index = 0;
        buildMinHeap(root, inorder);
        return root;
    }

    private static void storeInorder(BinaryTreeNode root, List<Integer> list) {
        if (root == null) return;
        storeInorder(root.left, list);
        list.add(root.data);
        storeInorder(root.right, list);
    }

    private static void buildMinHeap(BinaryTreeNode root, List<Integer> list) {
        if (root == null) return;
        root.data = list.get(index++);
        buildMinHeap(root.left, list);
        buildMinHeap(root.right, list);
    }
};

```

---

# Example Walkthrough

Input BST (level order):

```
4 2 6 1 3 5 7
```

**Step 1 – Inorder Traversal**

```
[1, 2, 3, 4, 5, 6, 7]
```

**Step 2 – Preorder Assignment**

```
Root  -> 1
Left  -> 2
Left  -> 3
Right -> 4
...
```

**Final Tree (Preorder Output)**

```
1 2 3 4 5 6 7
```

Min Heap property satisfied.
