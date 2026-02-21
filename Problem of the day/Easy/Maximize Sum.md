# Intuition

At every index `i`, two choices exist:

* **Do not swap**

  ```
  (arr1[i], arr2[i])
  ```
* **Swap**

  ```
  (arr2[i], arr1[i])
  ```

The contribution to the total sum depends on:

1. Current pair:

   ```
   |arr1[i] − arr2[i]|
   ```
2. Previous interaction:

   ```
   |arr1[i] − arr2[i-1]|
   ```

The decision at index `i` depends on whether the previous index `(i-1)` was swapped or not.
Therefore use **Dynamic Programming**.

---

# Approach

Maintain:

* `dp0` → maximum sum till index `i` when index `i` is **NOT swapped**
* `dp1` → maximum sum till index `i` when index `i` **IS swapped**

For every index compute:

Base contribution:

```
base = |arr1[i] - arr2[i]|
```

Transition from previous states:

### Case 1: No Swap at i

Two possibilities:

* previous not swapped
* previous swapped

```
dp0 = max(
 prev0 + base + |arr1[i] - arr2[i-1]|,
 prev1 + base + |arr1[i] - arr1[i-1]|
)
```

### Case 2: Swap at i

```
dp1 = max(
 prev0 + base + |arr2[i] - arr2[i-1]|,
 prev1 + base + |arr2[i] - arr1[i-1]|
)
```

Start with:

```
dp0 = dp1 = |arr1[0] - arr2[0]|
```

Return maximum of final states.

---

# Complexity

* **Time:** (O(n))
* **Space:** (O(1))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int calculateMaximisedSum(int[] arr1, int[] arr2, int n) {

        long dp0 = Math.abs(arr1[0] - arr2[0]); // no swap
        long dp1 = dp0;                         // swap

        for (int i = 1; i < n; i++) {

            long base = Math.abs(arr1[i] - arr2[i]);

            long newDp0 = Math.max(
                dp0 + base + Math.abs(arr1[i] - arr2[i - 1]),
                dp1 + base + Math.abs(arr1[i] - arr1[i - 1])
            );

            long newDp1 = Math.max(
                dp0 + base + Math.abs(arr2[i] - arr2[i - 1]),
                dp1 + base + Math.abs(arr2[i] - arr1[i - 1])
            );

            dp0 = newDp0;
            dp1 = newDp1;
        }

        return (int)Math.max(dp0, dp1);
    }
};

```

----

# Example walkthrough with explanation

Example:

```
arr1 = [2,3,4,1]
arr2 = [2,4,1,1]
```

At index `2`, swapping increases cross absolute differences.

After swap:

```
arr1 = [2,3,1,1]
arr2 = [2,4,4,1]
```

Total defined sum:

```
|2-2|
+|3-2|
+|3-4|
+|1-4|
+|1-4|
+|1-4|
+|1-1|
= 11
```

Maximum = **11**.
