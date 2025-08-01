# Intuition
	
We want to find an index `i` such that:

```
sum of A[0 to i-2] == sum of A[i to N-1]
```

(where indexing is 1-based).

This can be efficiently solved using:
* **Prefix sum tracking**
* **Total array sum**

---

# Approach

1. Calculate the **total sum** of the array: `totalSum = sum(A)`.
2. Initialize `prefixSum = 0`.
3. Iterate through the array:
   * At each index `i`, compute the **right sum** as:
     `rightSum = totalSum - prefixSum - A[i]`
   * If `prefixSum == rightSum`, then index `i+1` is a **beautiful index** (1-based).
   * Return that index immediately (we want the **leftmost**).
   * Otherwise, update `prefixSum += A[i]`.
4. If no such index is found, return `-1`.

---

# Complexity

* **Time complexity:** `O(N)` per test case (linear scan)
* **Space complexity:** `O(1)` (only uses variables)

---

# Code

```java
import java.util.*;

public class Solution {

    public static int beautifulIndex(int N, int[] A) {
        long totalSum = 0;
        for (int num : A) {
            totalSum += num;
        }

        long prefixSum = 0;
        for (int i = 0; i < N; i++) {
            long suffixSum = totalSum - prefixSum - A[i];
            if (prefixSum == suffixSum) {
                return i + 1; // converting to 1-based index
            }
            prefixSum += A[i];
        }

        return -1;
    }
}
```

---

### **Example Walkthrough**

Input:

```
A = [1, 7, 3, 6, 5, 6]
```

* Total sum = 28
* i=0 → prefixSum=0, suffixSum=27
* i=1 → prefixSum=1, suffixSum=20
* i=2 → prefixSum=8, suffixSum=17
* i=3 → prefixSum=11, suffixSum=11 → match found at **index 4** (1-based)

Answer: `4`

---
