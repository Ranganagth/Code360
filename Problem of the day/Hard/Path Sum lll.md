# Intuition

You need to count **all downward paths (parent → child)** whose sum = `k`.

This is not limited to root-to-leaf.
A valid path can start from **any node** and end at any descendant.

Use **prefix sum + hashmap**:

* Track cumulative sum from root
* If `currentSum - k` exists before → valid path

---

# Approach

1. Use DFS traversal
2. Maintain:

   * `currentSum`
   * `HashMap<prefixSum, count>`
3. At each node:

   * `currentSum += node.val`
   * check if `(currentSum - k)` exists → add count
4. Update map
5. Recurse left & right
6. Backtrack (remove current sum)

---

# Complexity

* **Time complexity:**
$$O(N)
$$
* **Space complexity:**

$$O(N)
$$
---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int noWays(TreeNode<Integer> root, int k) {

        HashMap<Long, Integer> map = new HashMap<>();
        map.put(0L, 1); // base case

        return dfs(root, 0, k, map);
    }

    private static int dfs(TreeNode<Integer> node, long currSum, int k,
                           HashMap<Long, Integer> map) {

        if (node == null) return 0;

        currSum += node.data;

        int count = map.getOrDefault(currSum - k, 0);

        map.put(currSum, map.getOrDefault(currSum, 0) + 1);

        count += dfs(node.left, currSum, k, map);
        count += dfs(node.right, currSum, k, map);

        // backtrack
        map.put(currSum, map.get(currSum) - 1);

        return count;
    }
};

```

---

# Example walkthrough

For path:

```text
8 → 6 → -4
```

Prefix sums:

```text
8
14
10  → match (10 - k = 0 exists)
```

Count incremented.

---

# Key Insight

```text
Path sum = k
⇔ prefixSum[j] - prefixSum[i] = k
```

So track previous prefix sums to count efficiently.
