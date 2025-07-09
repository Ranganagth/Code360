# Intuition:

To find the **Least Greater Element on the right** for each element in an array, we want to efficiently search and update elements **from right to left** (because we care about the *future* elements).

For each element:
* Find the **smallest element greater than the current element** that appears *after* it.

---

# Approach (Using TreeSet):

Java's `TreeSet` (a self-balancing BST) is ideal here:

* Insert elements from **right to left**
* For each element, use `higher()` from `TreeSet` to find the **least greater** element
* Insert current element into the `TreeSet` so it's available for future comparisons

---
# Complexity Analysis:

* **Time**:
  * Each insertion & lookup in TreeSet: `O(log N)`
  * Total: `O(N log N)` per test case

* **Space**:
  * `O(N)` for TreeSet and result array

---

# Code:

```java
import java.util.*;

public class Solution {
    public static int[] leastGreaterElement(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];
        TreeSet<Integer> bst = new TreeSet<>();

        // Traverse from right to left
        for (int i = n - 1; i >= 0; i--) {
            Integer nextGreater = bst.higher(arr[i]);
            result[i] = (nextGreater != null) ? nextGreater : -1;
            bst.add(arr[i]);
        }

        return result;
    }
}
```

---

### **Example Walkthrough:**

**Input**: `[5, 6, 7, 2]`

1. i = 3: No greater than 2 → `-1`, add 2 → TreeSet = \[2]
2. i = 2: No greater than 7 → `-1`, add 7 → TreeSet = \[2,7]
3. i = 1: greater than 6 → 7, result = 7, add 6 → TreeSet = \[2,6,7]
4. i = 0: greater than 5 → 6, result = 6, add 5 → TreeSet = \[2,5,6,7]

**Output**: `[6, 7, -1, -1]`

---
