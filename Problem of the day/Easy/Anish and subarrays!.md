# Intuition

We are asked to **count the number of subarrays** whose **maximum element lies between X and Y (inclusive)**.

A brute-force approach would check all subarrays and compute their maximums, but this would take **O(N²)** time, which is not feasible for `N ≤ 10^5`.

---

# Approach

We use the **inclusion-exclusion principle**:

* Let:
  * `f(Y)` = number of subarrays where the **maximum element ≤ Y**
  * `f(X - 1)` = number of subarrays where the **maximum element < X**

Then, the number of subarrays whose maximum is in **\[X, Y]** is:

```
Answer = f(Y) - f(X - 1)
```

### How to compute f(K):

Count subarrays where all elements ≤ K. Why?

> Because a subarray where all elements ≤ K automatically has its **maximum ≤ K**.

To do this efficiently:
* Traverse the array.
* Maintain a window of valid elements (`<= K`).
* For each contiguous segment of valid elements of length `len`, it contributes `len * (len + 1) / 2` subarrays.

---

# Complexity Analysis

* `O(N)` per test case.
* Efficient for `N ≤ 10^5`.

---

# Code

```java
public class Solution {

    public static int find(int n, int[] arr, int x, int y) {
        return countSubarraysWithMaxAtMost(arr, y) - countSubarraysWithMaxAtMost(arr, x - 1);
    }

    private static int countSubarraysWithMaxAtMost(int[] arr, int k) {
        int count = 0;
        int length = 0;

        for (int num : arr) {
            if (num <= k) {
                length++;
            } else {
                count += length * (length + 1) / 2;
                length = 0;
            }
        }

        count += length * (length + 1) / 2;
        return count;
    }
}
```

---

### **Example Walkthrough**

For:

```
ARR = [2, 1, 4, 3], X = 2, Y = 3
```

* `f(3)` = subarrays with max ≤ 3:
  * Segments: \[2,1] → 3 subarrays; \[3] → 1 subarray → total = 4
  
* `f(1)` = subarrays with max < 2:
  * \[1] → 1 subarray

**Answer = 4 - 1 = 3**

---
