# Intuition

The problem ignores tree structure and cares only about **which values exist** in each BST.
BST properties are irrelevant beyond uniqueness.
If both trees contain exactly the same elements, order and shape do not matter.
So the task reduces to **set equality**.

---

# Approach

1. Traverse the first BST and store all node values in a set.
2. Traverse the second BST and store all node values in another set.
3. Compare the two sets.

   * If equal → same elements → return `true`
   * Otherwise → return `false`

Traversal order does not matter because sets discard ordering.

---

# Complexity

Let `N` be the number of nodes in each tree.

- **Time Complexity**
	* Traversal of both trees: `O(N)`

- **Space Complexity**
	* Two sets storing elements: `O(N)`

---

# Code

```java
import java.util.* ;
import java.io.*; 
/*

    Following is the TreeNode structure.

    class TreeNode<T> {
        T data;
        TreeNode<T> left;
        TreeNode<T> right;

        public TreeNode(T data) {
            this.data = data;
        }
    }

*/

public class Solution {

    private static void collectElements(TreeNode<Integer> root, Set<Integer> elements) {
        if (root == null) return;
        elements.add(root.data);
        collectElements(root.left, elements);
        collectElements(root.right, elements);
    }


    public static Boolean checkBSTs(TreeNode<Integer> root1, TreeNode<Integer> root2) {       
        Set<Integer> set1 = new HashSet<>();
        Set<Integer> set2 = new HashSet<>();

        collectElements(root1, set1);
        collectElements(root2, set2);

        return set1.equals(set2);

    }

};

```

---

# Example Walkthrough

## Example 1

**Tree 1 elements:**
`8, 5, 10, 2, 6, 7`

**Tree 2 elements:**
`10, 5, 2, 8, 6`

**Sets comparison:**
`{2,5,6,7,8,10}` ≠ `{2,5,6,8,10}`

**Result:** `false`

## Example 2

**Tree 1 elements:**
`26, 52, 78`

**Tree 2 elements:**
`26, 52, 78`

**Sets comparison:**
`{26,52,78}` = `{26,52,78}`

**Result:** `true`

---