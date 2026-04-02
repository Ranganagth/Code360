# Intuition

You need the **deepest node which is a right child and also a leaf**.

If multiple exist at same depth → choose the **rightmost** → natural with **level order traversal (BFS)**.

---

# Approach

Use BFS with tracking:

* current node
* whether it is a **right child**

Steps:

1. Traverse level by level
2. If node is:

   * leaf (`left == null && right == null`)
   * AND is right child
     → update answer
3. Continue traversal → last valid one will be deepest & rightmost

---

# Complexity

* **Time complexity:**
$$O(N)$$

* **Space complexity:**
$$O(N)$$

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static BinaryTreeNode<Integer> deepestRightLeaf(BinaryTreeNode<Integer> root) {

		if (root == null) return null;

		Queue<Pair> q = new LinkedList<>();
		q.offer(new Pair(root, false));

		BinaryTreeNode<Integer> result = null;

		while (!q.isEmpty()) {

			Pair curr = q.poll();
			BinaryTreeNode<Integer> node = curr.node;
			boolean isRight = curr.isRight;

			// check right leaf
			if (node.left == null && node.right == null && isRight) {
				result = node;
			}

			if (node.left != null) {
				q.offer(new Pair(node.left, false));
			}

			if (node.right != null) {
				q.offer(new Pair(node.right, true));
			}
		}

		return result;
	}

	static class Pair {
		BinaryTreeNode<Integer> node;
		boolean isRight;

		Pair(BinaryTreeNode<Integer> node, boolean isRight) {
			this.node = node;
			this.isRight = isRight;
		}
	}
}
```

---

# Example walkthrough

Tree:

```
    1
   / \
  2   3
```

Leaves:

```
2 (left) 
3 (right) 
```

Answer:

```
3
```
