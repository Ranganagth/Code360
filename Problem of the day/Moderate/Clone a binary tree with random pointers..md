# Solution

```java
import java.util.* ;
import java.io.*; 
/************************************************************

    Following is the TreeNode class structure

    class TreeNode<T> 
    {
       public:
        T data;
        TreeNode<T> left;
        TreeNode<T> right;
        TreeNode<T> random;

        TreeNode(T data) 
        {
            this.data = data;
            left = null;
            right = null;
            random = null;
        }
    };

*************************************************************/

public class Solution 
{

    private static HashMap<TreeNode<Integer>, TreeNode<Integer>> map;

    private static void createNodesAndMap(TreeNode<Integer> originalNode) {
        if (originalNode == null) {
            return;
        }

        TreeNode<Integer> clonedNode = new TreeNode<>(originalNode.data);
        map.put(originalNode, clonedNode);

        createNodesAndMap(originalNode.left);
        createNodesAndMap(originalNode.right);
    }

    private static void setLeftRightPointers(TreeNode<Integer> originalNode) {
        if (originalNode == null) {
            return;
        }

        TreeNode<Integer> clonedNode = map.get(originalNode);

        if (originalNode.left != null) {
            clonedNode.left = map.get(originalNode.left);
        }

        if (originalNode.right != null) {
            clonedNode.right = map.get(originalNode.right);
        }

        setLeftRightPointers(originalNode.left);
        setLeftRightPointers(originalNode.right);
    }

    private static void setRandomPointers(TreeNode<Integer> originalNode) {
        if (originalNode == null) {
            return;
        }

        TreeNode<Integer> clonedNode = map.get(originalNode);

        if (originalNode.random != null) {
            clonedNode.random = map.get(originalNode.random);
        } else {
            clonedNode.random = null;
        }

        setRandomPointers(originalNode.left);
        setRandomPointers(originalNode.right);
    }

    public static TreeNode<Integer> cloneBinaryTreeRandomPointer(TreeNode<Integer> root)
    {
        if (root == null) {
            return null;
        }

        map = new HashMap<>();

        createNodesAndMap(root);

        setLeftRightPointers(root);

        setRandomPointers(root);

        return map.get(root);
    }
}

```