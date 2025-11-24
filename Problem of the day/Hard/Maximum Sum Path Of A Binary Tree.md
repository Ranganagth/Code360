# Intuition

A path can pass through a node in two ways:

1. Extend upward through only one child.
2. Form a “bridge” through both children.
   Only the second case can create a maximum path spanning two sides.
   Track two values at each node:

* best downward path including the node
* best full path using both children
  Return the global maximum.

---

# Approach

Use DFS.
For each node:

* Recursively compute the maximum downward path from left and right.
* Ignore any negative child contribution by clamping to zero.
* Downward path = node value + max(left, right).
* Full path = node value + left + right.
  Update a global maximum.
  Return downward path to the parent.

---

# Complexity

* **Time complexity:** O(n)
* **Space complexity:** O(h)

---

# Code

```java
/************************************************************

 Following is the BinaryTreeNode class structure
 class BinaryTreeNode<T> {
     T data;
     BinaryTreeNode<T> left;
     BinaryTreeNode<T> right;

     BinaryTreeNode(T data) {
         this.data = data;
         left = null;
         right = null;
     }
 };
 ************************************************************/

public class Solution {

    static class Result {
        int max;
        Result(int v) { max = v; }
    }

    private static int dfs(BinaryTreeNode<Integer> node, Result res) {
        if (node == null) return 0;

        int left = dfs(node.left, res);
        int right = dfs(node.right, res);

        left = Math.max(0, left);
        right = Math.max(0, right);

        int through = node.data + left + right;

        if (through > res.max) res.max = through;

        return node.data + Math.max(left, right);
    }

    public static int maxPathSum(BinaryTreeNode<Integer> root) {
        Result res = new Result(Integer.MIN_VALUE);
        dfs(root, res);
        return res.max;
    }
};

```

---

# Example walkthrough with explanation

Tree: 1, 2, 3, 4
At node 4: downward=4, full=4
At node 2: left=4, right=0 → downward=6, full=6
At node 3: downward=3, full=3
At node 1: left=6, right=3 → full path = 1+6+3 = 10
Global max becomes 10
