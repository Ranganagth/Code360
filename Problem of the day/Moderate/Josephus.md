# Intuition
**Josephus Problem Explanation**

The Josephus problem can be defined recursively:

If there are `n` people in a circle and every `k`-th person is eliminated, the position of the survivor is:

$$
\text{Josephus}(n, k) = 
\begin{cases}
0 & \text{if } n = 1 \\
(\text{Josephus}(n-1, k) + k) \mod n & \text{if } n > 1
\end{cases}
$$

This formula gives **0-based indexing**. So, to get **1-based result**, you add **1** to the final result.

---

# Approach

We can implement this recursively or iteratively. For large values of `n` and `k` (up to 10^4), an **iterative** approach is more efficient and avoids stack overflow.

---
# Complexity

* **Time Complexity:** O(N) per test case
* **Space Complexity:** O(1)

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    public static int josephus(int n, int k) {
        int res = 0;  // Start with 0 for base case n = 1 (0-indexed)
        for (int i = 2; i <= n; i++) {
            res = (res + k) % i;
        }
        return res + 1;  // Convert to 1-based index
    }
}
```

---

### **Example Execution**

#### Input:

```
2
4 2
7 3
```

#### Output:

```
1
4
```

---
