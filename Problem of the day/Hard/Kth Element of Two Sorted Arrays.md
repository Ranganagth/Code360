# Intuition

Each person receives the smallest available plate among the two front plates.
This is equivalent to repeatedly removing the minimum element from two sorted arrays.
The sequence of plates served is exactly the **merged sorted order** of `ROW1` and `ROW2`.
Therefore, the K-th person gets the **K-th smallest element** from the union of both rows.

Do not simulate plate picking. That is linear per test and will TLE for large inputs.

---

# Approach

Use the classic **two-pointer merge** logic, but stop as soon as the K-th element is reached.

Steps:

1. Initialize two pointers `i` for `row1`, `j` for `row2`
2. Repeat K times:
   * If one array is exhausted, take from the other
   * Otherwise take the smaller of `row1[i]` and `row2[j]`
3. The last picked value is the answer

This directly matches the serving rule.

---

# Complexity

* **Time:** `O(K)`
* **Space:** `O(1)`

Optimal under constraints since `K ≤ N + M`.

---

# Code

```java
public class Solution {
    public static int ninjaAndLadoos(int row1[], int row2[], int m, int n, int k) {

        int i = 0, j = 0;
        int count = 0;
        int ans = 0;

        while (count < k) {

            if (i < m && j < n) {
                if (row1[i] <= row2[j]) {
                    ans = row1[i];
                    i++;
                } else {
                    ans = row2[j];
                    j++;
                }
            } else if (i < m) {
                ans = row1[i];
                i++;
            } else {
                ans = row2[j];
                j++;
            }

            count++;
        }

        return ans;
    }
};

```

---

# Example Walkthrough

`ROW1 = [3, 11, 23, 45, 52]`
`ROW2 = [4, 12, 14, 18]`
`K = 3`

**Merged order progression:**

1. min(3,4) → **3**
2. min(11,4) → **4**
3. min(11,12) → **11**

**Output:**

```
11
```
