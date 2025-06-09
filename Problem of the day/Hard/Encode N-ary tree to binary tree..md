# Solution

```java
import java.util.* ;
import java.io.*; 
/*************************************************************
    Binary tree node class for reference:

    class BTreeNode<T> {
        public T data;
        public BTreeNode<T> left;
        public BTreeNode<T> right;

        BTreeNode(T data) {
            this.data = data;
            left = null;
            right = null;
        }
    }

    Nary tree node class for reference:

    class NTreeNode<T> {
        public T data;
        public ArrayList<NTreeNode<T>> child;

        NTreeNode(T data) {
            this.data = data;
            child = new ArrayList();
        }
    }

*************************************************************/

public class Solution {
    public static BTreeNode<Integer> encode(NTreeNode<Integer> root) {
       if (root == null) {
		   return null;
	   }

	   BTreeNode<Integer> bRoot = new BTreeNode<>(root.data);

	   if (root.child.size() > 0) {
		   bRoot.left = encode(root.child.get(0));
	   }

	   BTreeNode<Integer> current = bRoot.left;
	   for (int i = 1; i < root.child.size(); i++) {
		   current.right = encode(root.child.get(i));
		   current = current.right;
	   }

	   return bRoot;
    }

    public static NTreeNode<Integer> decode(BTreeNode<Integer> root) {
        if (root == null) {
			return null;
		}

		NTreeNode<Integer> nRoot = new NTreeNode<>(root.data);

		BTreeNode<Integer> current = root.left;

		while (current != null) {
			nRoot.child.add(decode(current));
			current = current.right;
		}
		return nRoot;
    }
}

```