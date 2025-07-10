> Partially accepted 5/6 Test Cases
### Problem Summary:

You are given a binary tree. Count the number of **unival subtrees**.

* A **unival subtree** is a subtree where **all nodes have the same value**.
* Every **leaf node** is a unival subtree by itself.

---
# Intuition:

We use **post-order traversal**:
* Traverse left and right
* Check if both left and right subtrees are unival
* If they are, and the current node matches their values (if present), mark the current subtree as unival

---

# Approach:

1. Define a helper function that returns:
   * Whether the current subtree is unival
   * Increment a counter if it is
2. Use **post-order** traversal
3. Return the count

---

# Complexity Analysis:

* **Time:** O(N), because we visit each node once
* **Space:** O(H), where H is the height of the tree (O(log N) for balanced, O(N) worst case)

---

# Code:

```java
/*******************************************************
	Following is the BinaryTreeNode class structure
	
	class BinaryTreeNode<T> {
		T data;
		BinaryTreeNode<T> left;
		BinaryTreeNode<T> right;
		
		public BinaryTreeNode(T data) {
			this.data = data;
		}
	}
*******************************************************/

public class Solution {

	private static class Result {
		int count;
	}

	private static boolean isUnival(BinaryTreeNode<Integer> node, Result res) {
		if (node == null) return true;

		boolean leftUnival = isUnival(node.left, res);
		boolean rightUnival = isUnival(node.right, res);

		if (!leftUnival || !rightUnival) {
			return false;
		}

		if (node.left != null && node.left.data != node.data) return false;
		if (node.right != null && node.right.data != node.data) return false;

		// current subtree is unival
		res.count++;
		return true;
	}

	public static int countUnivalTrees(BinaryTreeNode<Integer> root) {
		Result res = new Result();
		isUnival(root, res);
		return res.count;
	}
}
```

---

###  **Example:**

For the tree:

```
    1
   / \
  1   1
     / \
    1   1
```

The unival subtrees are:
* All 3 leaf nodes (3)
* The subtree rooted at the right child (4)
* The whole right subtree (5)

Total = 5 unival subtrees.

### **Walkthrough:**

Input:

```
1 2 3 2 -1 3 4 -1 -1 3 3 -1 -1 -1 -1 -1 -1
```

Resulting tree:

```
        1
       / \
      2   3
     /   / \
    2   3   4
       /
      3
```

**Unival Subtrees**:

* Nodes with values: leaf(2), leaf(3), leaf(3), leaf(4), subtree rooted at 3 (with 3 as child), and the subtree rooted at 2
* Total: 6

Output: `6`

---
