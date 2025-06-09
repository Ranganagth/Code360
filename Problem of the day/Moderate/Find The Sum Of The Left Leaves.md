# Solution

```java
import java.util.* ;
import java.io.*; 
/*************************************************************
    Following is the Binary Tree node structure:

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

*************************************************************/

public class Solution {
    public static int sumOfLeftLeaves(TreeNode<Integer> root) {
        if (root == null) return 0;

		int sum = 0;

		if (root.left != null && isLeaf(root.left)) {
			sum += root.left.data;
		}

		sum += sumOfLeftLeaves(root.left);
		sum += sumOfLeftLeaves(root.right);

		return sum;
    }

	private static boolean isLeaf(TreeNode<Integer> node) {
		return node.left == null && node.right == null;
	}
}

```