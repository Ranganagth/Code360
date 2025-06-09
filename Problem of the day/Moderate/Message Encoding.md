# Solution

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    static class Node implements Comparable<Node> {
        int freq;
        int index;
        Node left, right;

        Node(int freq, int index) {
            this.freq = freq;
            this.index = index;
        }

        Node(int freq, Node left, Node right) {
            this.freq = freq;
            this.left = left;
            this.right = right;
            this.index = -1;
        }

        public boolean isLeaf() {
            return left == null && right == null;
        }

        public int compareTo(Node other){
            return Integer.compare(this.freq, other.freq);
        }
    }


    public static String[] huffmanCode(int[] arr) {
        int n = arr.length;
        PriorityQueue<Node> pq = new PriorityQueue<>();

        String[] codes = new String[n];

        for (int i = 0; i < n; i++) {
            pq.offer(new Node(arr[i], i));
        }

        while (pq.size() > 1) {
            Node n1 = pq.poll();
            Node n2 = pq.poll();
            Node parent = new Node(n1.freq + n2.freq, n1, n2);
            pq.offer(parent);
        }

        Node root = pq.poll();
        generateCodes(root, "", codes);

        return codes;
    }

    private static void generateCodes(Node node, String code, String[] codes) {
        if (node == null) return;

        if (node.isLeaf()) {
            codes[node.index] = code.length() > 0 ? code : "0";
            return;
        }

        generateCodes(node.left, code + "0", codes);
        generateCodes(node.right, code + "1", codes);
    }

}

```