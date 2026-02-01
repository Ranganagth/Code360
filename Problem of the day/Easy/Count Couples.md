# Intuition

* We have **two BSTs**:

  * One represents **men IDs**
  * One represents **women IDs**
* A **married couple** exists if:
$$manID + womanID = X$$

* IDs are **unique inside each tree**, but **can overlap across trees**.
* We must count **all valid pairs**.

This is a classic **Two-Sum across two BSTs** problem.

## Key Insight

If we:

1. Traverse one BST and store its values in a **HashSet**
2. Traverse the other BST and for each value `v`, check if `X - v` exists in the set

We can count all valid couples in **linear time**.

---

# Approach

### Step 1: Traverse first BST (men)

* Store all IDs in a `HashSet`

### Step 2: Traverse second BST (women)

* For each ID `w`, check if `X - w` exists in the set
* If yes → valid couple → increment count

### Step 3: Return total count

> We don’t care about tree structure — only values.

---

# Complexity

* **Time Complexity:**
$$  O(T_1 + T_2)
$$
  where `T1` and `T2` are node counts of both BSTs

* **Space Complexity:**
$$  O(T_1)
$$
  for the HashSet

Works efficiently within constraints
No recursion depth issues

---

# Code

```java
import java.util.*;

public class Solution {

    // Helper to store all values of a BST in a set
    private static void storeValues(TreeNode root, HashSet<Integer> set) {
        if (root == null) return;
        storeValues(root.left, set);
        set.add(root.data);
        storeValues(root.right, set);
    }

    // Helper to count valid pairs
    private static int countPairs(TreeNode root, HashSet<Integer> set, int x) {
        if (root == null) return 0;

        int count = 0;
        count += countPairs(root.left, set, x);

        if (set.contains(x - root.data)) {
            count++;
        }

        count += countPairs(root.right, set, x);
        return count;
    }

    public static int countCouples(TreeNode root1, TreeNode root2, int x) {
        HashSet<Integer> set = new HashSet<>();

        // Store IDs from first BST
        storeValues(root1, set);

        // Count matching IDs from second BST
        return countPairs(root2, set, x);
    }
};

```

---

# Example Walkthrough

### Input

```
Men BST IDs:     [1, 2, 5, 7, 9, 14]
Women BST IDs:  [1, 2, 3, 4, 5]
X = 6
```

### Valid pairs:

* (1, 5)
* (2, 4)
* (5, 1)

### Output:

```
3
```

Each pair sums to `6`
Count = **3**

---

## Why this is optimal

* Avoids nested loops
* Avoids converting both trees to arrays
* Handles up to **200,000 nodes total**
* Clean and interview-ready

---
