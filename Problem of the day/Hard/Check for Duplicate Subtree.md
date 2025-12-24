# Intuition  

Equal subtrees share two things: identical shape and identical characters at every corresponding node. If we “describe” every subtree in a canonical way, then two subtrees are equal exactly when their descriptions are identical. Detecting equality reduces to detecting duplicate descriptions.

---

# Approach

Use post-order traversal. For every node, build a serialization string that uniquely represents its subtree:

serialization(node) =  
`node.char + "," + serialization(node.left) + "," + serialization(node.right)`

Use `#` for null children to preserve structure.

Store every serialization in a hash map with its frequency.  
Also compute the subtree size while traversing. We only consider a subtree valid for comparison if its size ≥ 2 (this matches the sample where repeating leaf nodes alone do NOT count).

If any serialization with size ≥ 2 appears at least twice, answer = true.

**Why this works:** identical subtree ⇒ identical serialization; different structure/value ⇒ different serialization.

---

# Complexity

- **Time:** O(N) — each node processed once, string operations amortized over tree size  
- **Space:** O(N) — recursion + hash map

---

# Code

```java
import java.util.*;

public class Solution {

    public static boolean similarSubtree(BinaryTreeNode<Character> root) {
        Map<String, Integer> freq = new HashMap<>();
        boolean[] found = new boolean[1];
        dfs(root, freq, found);
        return found[0];
    }

    // returns: serialization, subtree size
    private static Pair dfs(BinaryTreeNode<Character> node,
                            Map<String, Integer> freq,
                            boolean[] found) {
        if (node == null)
            return new Pair("#", 0);

        Pair L = dfs(node.left, freq, found);
        Pair R = dfs(node.right, freq, found);

        String serial = node.data + "," + L.s + "," + R.s;
        int size = 1 + L.sz + R.sz;

        int c = freq.getOrDefault(serial, 0);
        freq.put(serial, c + 1);

        if (!found[0] && size >= 2 && c + 1 >= 2)
            found[0] = true;

        return new Pair(serial, size);
    }

    private static class Pair {
        String s;
        int sz;
        Pair(String s, int sz) { this.s = s; this.sz = sz; }
    }
};

```

---

# Example walkthrough

**Tree (Sample 1):**

```
        a
       / \
      b   a
     /     \
    c       b
           /
          c
```

**Post-order builds serializations:**

- node(c) → `"c,#,#"` size 1
    
- node(b with left c) → `"b,c,#,#,#"` size 2 (store)
    
- right subtree bottom b→same serialization `"b,c,#,#,#"` size 2 (appears again)

The same description appears twice for subtree size ≥ 2 → result `True`.

This detects the first repeated subtree and stops.

---


# Solution in JavaScript

```javascript

/* Declare and implement your function here 
eg: function example(parameter_name1,parameter_name2....){}
Handle the input/output from main()
*/

function Node(val) {
    this.val = val;
    this.left = null;
    this.right = null;
}

function buildTree(arr) {
    if (!arr.length || arr[0] === "#") return null;

    const root = new Node(arr[0]);
    const queue = [root];
    let i = 1;

    while (queue.length && i < arr.length) {
        const current = queue.shift();

        if (arr[i] !== "#") {
            current.left = new Node(arr[i]);
            queue.push(current.left);
        }
        i++;

        if (i < arr.length && arr[i] !== "#") {
            current.right = new Node(arr[i]);
            queue.push(current.right);
        }
        i++;
    }

    return root;
}

function hasDuplicateSubtree(root) {
    const map = new Map();
    let found = false;

    function serialize(node) {
        if (!node) return "#";

        const left = serialize(node.left);
        const right = serialize(node.right);

        const serial = `${node.val},${left},${right}`;

        if (serial !== `${node.val},#,#`) {
            map.set(serial, (map.get(serial) || 0) + 1);
            if (map.get(serial) === 2) found = true;
        }

        return serial;
    }

    serialize(root);
    return found;
}


process.stdin.resume();
process.stdin.setEncoding('ascii');

var input_stdin = "";
var input_stdin_array = "";
var input_currentline = 0;

process.stdin.on('data', function (data) {
    input_stdin += data;
});

process.stdin.on('end', function () {
    input_stdin_array = input_stdin.split("\n");
    main();
});

function readLine() {
    return input_stdin_array[input_currentline++];
}


function main() {

    let T = parseInt(readLine());

    while (T--) {
        let N = parseInt(readLine());
        let values = [];

        while (values.length < N || values[values.length - 1] !== "#" || values.length < 2 * N - 1) {
            const line = readLine();
            if (!line) break;
            values = values.concat(line.trim().split(/\s+/));
        }

        const root = buildTree(values);
        console.log(hasDuplicateSubtree(root) ? "True" : "False");
    }
}

```

---