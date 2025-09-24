# Intuition

We want to minimize:

$$
\max \big( |X[i] - Y[j]|, \, |Y[j] - Z[k]|, \, |Z[k] - X[i]| \big)
$$

Key observations:

1. Since arrays are sorted, if we fix one element, moving the other pointers towards it may reduce the max difference.
2. At any point, the **maximum absolute difference** among the three numbers is simply:

   $$
   \max(current) - \min(current)
   $$

   Because the largest difference will always be between the smallest and largest among $X[i], Y[j], Z[k]$.

So the problem reduces to:

* Minimize $\max(x, y, z) - \min(x, y, z)$.

---

# Approach

1. Initialize three pointers `i = 0, j = 0, k = 0`.
2. At each step:

   * Compute `maxVal = max(X[i], Y[j], Z[k])`
   * Compute `minVal = min(X[i], Y[j], Z[k])`
   * Update answer as `min(answer, maxVal - minVal)`.
3. Move the pointer that currently has the **minimum value**.

   * This is because to reduce the gap, we should try to bring the minimum closer to the maximum.
   * Increasing the largest value further will only worsen the difference.
4. Stop when one pointer reaches the end of its array.

---

# Complexity

* Each pointer moves at most once per iteration → **O(A + B + C)**.
* Much faster than brute force (**O(A × B × C)**).

---

# Code

```java
import java.util.*; 
import java.io.*; 

public class Solution {
    public static int threePointer(ArrayList<Integer> X, ArrayList<Integer> Y, ArrayList<Integer> Z) {
        int i = 0, j = 0, k = 0;
        int ans = Integer.MAX_VALUE;

        while (i < X.size() && j < Y.size() && k < Z.size()) {
            int x = X.get(i), y = Y.get(j), z = Z.get(k);

            int maxVal = Math.max(x, Math.max(y, z));
            int minVal = Math.min(x, Math.min(y, z));

            ans = Math.min(ans, maxVal - minVal);

            // Move the pointer at the smallest value
            if (minVal == x) {
                i++;
            } else if (minVal == y) {
                j++;
            } else {
                k++;
            }
        }

        return ans;
    }
};

```

---

### Example Walkthrough

Input:

```
X = [1,2,3,4,5]
Y = [1,3,5,7,9]
Z = [2,4,6]
```

Steps:

* i=0, j=0, k=0 → values (1,1,2), max=2, min=1 → diff=1 → ans=1
* Already minimal possible (cannot go below 0), so final answer = **1**.

---
