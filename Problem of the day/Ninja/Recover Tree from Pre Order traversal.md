# Intuition:

Given:
- A **preorder traversal** string of a binary tree.
- Node value is preceded by `D` dashes where `D` is its depth.
- **If a node has one child, it is always a left child**.

Example:  
`"1-2--3--4-5--6--7"`  
Means:
```
Depth 0: 1  
  Depth 1: 2  
    Depth 2: 3  
    Depth 2: 4  
  Depth 1: 5  
    Depth 2: 6  
    Depth 2: 7  
```

---

# Approach:

1. **Parse the string** to extract nodes with their respective depths.
2. Use a **stack** to manage the parent-child relationships:
   - If the depth is greater than the stack size, it means it's a child of the last node in stack.
   - If the depth is smaller or equal, **pop** from the stack until you reach the correct parent.
3. Attach the new node as left or right based on if left is already filled.

---

# Code:

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
    public static TreeNode<Integer> recoverFromPreorder(String S) {
        Stack<Pair> stack = new Stack<>();
        int i = 0;
        int n = S.length();

        while (i < n) {
            int depth = 0;
            // Count dashes to determine depth
            while (i < n && S.charAt(i) == '-') {
                depth++;
                i++;
            }

            // Read the number
            int num = 0;
            while (i < n && Character.isDigit(S.charAt(i))) {
                num = num * 10 + (S.charAt(i) - '0');
                i++;
            }

            TreeNode<Integer> node = new TreeNode<>(num);

            // Maintain stack to track parent-child relationships
            while (stack.size() > depth) {
                stack.pop();
            }

            if (!stack.isEmpty()) {
                TreeNode<Integer> parent = stack.peek().node;
                if (parent.left == null)
                    parent.left = node;
                else
                    parent.right = node;
            }

            stack.push(new Pair(node, depth));
        }

        // Root node is at the bottom of the stack
        while (stack.size() > 1)
            stack.pop();

        return stack.peek().node;
    }

    private static class Pair {
        TreeNode<Integer> node;
        int depth;

        Pair(TreeNode<Integer> node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }
}
```

---

### **Example**

For input: `"1-2--3--4-5--6--7"`  
The tree reconstructed will be:
```
       1
     /   \
    2     5
   / \   / \
  3   4 6   7
```

---
