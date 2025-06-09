# Solution

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

}

```