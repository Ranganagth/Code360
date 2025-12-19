# Intuition

Only left **leaf** nodes qualify.
Depth decides priority.
If multiple candidates exist at the same maximum depth, pick the one with the larger value.

Traverse the tree while tracking:
* current depth
* whether the node is a left child
* whether it is a leaf

---

# Approach

Depth-First Search.

Maintain two global trackers:
* `maxDepth` → deepest level found so far for a valid left leaf
* `answer` → node value corresponding to `maxDepth`

DFS parameters:
* current node
* current depth
* boolean flag `isLeftChild`

At each node:
1. Check if it is a **leaf** (`left == null && right == null`)
2. Check if it is a **left child**
3. If valid:
   * Update result if:
     * depth > maxDepth
     * OR depth == maxDepth AND value is larger
4. Recurse left (mark `isLeftChild = true`)
5. Recurse right (mark `isLeftChild = false`)

---

# Complexity

* **Time:** `O(N)`
* **Space:** `O(H)` recursion stack, worst case `O(N)`

---

# Code 

```java
public class Solution {

    private static int maxDepth;
    private static int result;

    public static int deepestLeftLeafNode(BinaryTreeNode<Integer> root) {
        maxDepth = -1;
        result = 0;
        dfs(root, 0, false);
        return result;
    }

    private static void dfs(BinaryTreeNode<Integer> node, int depth, boolean isLeft) {
        if (node == null) return;

        // Check left leaf
        if (isLeft && node.left == null && node.right == null) {
            if (depth > maxDepth || (depth == maxDepth && node.data > result)) {
                maxDepth = depth;
                result = node.data;
            }
        }

        dfs(node.left, depth + 1, true);
        dfs(node.right, depth + 1, false);
    }
};

```

---

# Example Walkthrough

**Tree (partial):**

```
        3
       / \
      5   1
         /
        2
       /
      7
```

**Traversal highlights:**

* Node 7

  * left child
  * leaf
  * depth = 4
* No other left leaf at depth ≥ 4

**Result:**

```
7
```
