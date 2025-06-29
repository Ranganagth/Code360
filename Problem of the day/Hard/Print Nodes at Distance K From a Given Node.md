# Intuition:

To solve the problem of finding all nodes at a distance `K` from a given target node in a binary tree, we must be able to:
1. **Traverse downwards** from the target node.
2. **Traverse upwards** from the target node to its ancestors, and then to their other child subtrees.

---

# Approach:

* **Step 1**: Build a **parent mapping** using DFS or BFS to enable upward traversal.
* **Step 2**: Use **BFS starting from the target node**, while keeping track of visited nodes to avoid cycles.
* **Step 3**: Collect all nodes that are exactly at distance `K`.

---

# Code:

```java
import java.util.*;

class BinaryTreeNode<T> {
    T data;
    BinaryTreeNode<T> left, right;

    BinaryTreeNode(T data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

public class Solution {

    public static List<BinaryTreeNode<Integer>> printNodesAtDistanceK(BinaryTreeNode<Integer> root, BinaryTreeNode<Integer> target, int K) {
        List<BinaryTreeNode<Integer>> result = new ArrayList<>();
        if (root == null || target == null) return result;

        // Step 1: Map child -> parent
        Map<BinaryTreeNode<Integer>, BinaryTreeNode<Integer>> parentMap = new HashMap<>();
        buildParentMap(root, null, parentMap);

        // Step 2: BFS from target to distance K
        Set<BinaryTreeNode<Integer>> visited = new HashSet<>();
        Queue<BinaryTreeNode<Integer>> queue = new LinkedList<>();
        queue.add(target);
        visited.add(target);
        int currentDistance = 0;

        while (!queue.isEmpty()) {
            int size = queue.size();

            if (currentDistance == K) {
                // Add all nodes at current distance
                while (!queue.isEmpty()) {
                    result.add(queue.poll());
                }
                break;
            }

            for (int i = 0; i < size; i++) {
                BinaryTreeNode<Integer> current = queue.poll();

                // Check left
                if (current.left != null && !visited.contains(current.left)) {
                    visited.add(current.left);
                    queue.add(current.left);
                }

                // Check right
                if (current.right != null && !visited.contains(current.right)) {
                    visited.add(current.right);
                    queue.add(current.right);
                }

                // Check parent
                BinaryTreeNode<Integer> parent = parentMap.get(current);
                if (parent != null && !visited.contains(parent)) {
                    visited.add(parent);
                    queue.add(parent);
                }
            }
            currentDistance++;
        }

        return result;
    }

    private static void buildParentMap(BinaryTreeNode<Integer> node, BinaryTreeNode<Integer> parent, Map<BinaryTreeNode<Integer>, BinaryTreeNode<Integer>> map) {
        if (node == null) return;
        map.put(node, parent);
        buildParentMap(node.left, node, map);
        buildParentMap(node.right, node, map);
    }
}
```

---

### **Summary:**

* We avoid revisiting nodes using a `Set`.
* `buildParentMap()` helps simulate upward movement from child to parent.
* BFS ensures the shortest path is used and levels are accurately tracked.

---