# Intuition

When we left rotate an array by `k` positions, we move the first `k` elements to the end of the array while keeping the order the same.

For each query:
* We are **not** modifying the original array.
* We simply compute and return the result of rotating the **original array** by the given query value.

We optimize this by reducing redundant rotations using modulo:
* Rotating by `k` positions where `k >= n` is equivalent to rotating by `k % n`.

---

# Approach

For each query:
1. Compute effective rotation: `rot = query % nums.size()`
2. Create rotated array:
   * Append `nums[rot:] + nums[0:rot]`
3. Store this rotated array in the result list.

---

# Complexity Analysis

* **Time Complexity:** `O(Q * N)`
  (for each query of Q, we process the array of size N once)
* **Space Complexity:** `O(Q * N)`
  (we return Q rotated versions of the array)

---

# Code

```java
import java.util.*;

public class Solution {
    public static List<ArrayList<Integer>> leftRotationsOfArray(List<Integer> nums, List<Integer> queries) {
        List<ArrayList<Integer>> result = new ArrayList<>();
        int n = nums.size();

        for (int query : queries) {
            int rot = query % n;
            ArrayList<Integer> rotated = new ArrayList<>();

            // Add elements from rot to end
            for (int i = rot; i < n; i++) {
                rotated.add(nums.get(i));
            }

            // Add elements from start to rot
            for (int i = 0; i < rot; i++) {
                rotated.add(nums.get(i));
            }

            result.add(rotated);
        }

        return result;
    }
}
```

---

### **Example Walkthrough**

#### Input:

```text
nums = [7, 8, 6, 1, 2]
queries = [8, 4, 3]
```

#### Output:

```text
[1, 2, 7, 8, 6]  // 8 % 5 = 3 → Rotate left by 3
[2, 7, 8, 6, 1]  // 4 % 5 = 4 → Rotate left by 4
[1, 2, 7, 8, 6]  // 3 % 5 = 3 → Rotate left by 3
```

---
