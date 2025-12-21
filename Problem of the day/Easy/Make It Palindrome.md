# Intuition

Only adjacent merges are allowed. Merging increases a value and reduces length. To form a palindrome with minimum merges, always resolve mismatch from the ends inward. Greedy works: compare left and right sums and merge the smaller side to catch up.

---

# Approach

Use two pointers:

* `l` at start, `r` at end.
* If `A[l] == A[r]`: move both inward.
* If `A[l] < A[r]`: merge `A[l]` with `A[l+1]`, increment operations, move `l` forward.
* If `A[l] > A[r]`: merge `A[r]` with `A[r-1]`, increment operations, move `r` backward.
  Continue until `l >= r`.

This minimizes operations because each merge strictly reduces imbalance at the boundary.

---

# Complexity

* **Time:** `O(N)`
* **Space:** `O(1)`

---

# Code

```java
public class Solution 
{
    public static int palindrome(int[] A)
    {
        int n = A.length;
        long[] arr = new long[n];

        for (int i = 0; i < n; i++) {
            arr[i] = A[i];
        }

        int l = 0;
        int r = n - 1;
        int ops = 0;

        while (l < r) {
            if (arr[l] == arr[r]) {
                l++;
                r--;
            } else if (arr[l] < arr[r]) {
                arr[l + 1] += arr[l];
                l++;
                ops++;
            } else {
                arr[r - 1] += arr[r];
                r--;
                ops++;
            }
        }
        return ops;
    }
}


```

---

# Example Walkthrough

**Input:** `[1, 2, 3, 4, 1]`

* `l=0, r=4` → `1 == 1` → move inward
* `l=1, r=3` → `2 < 4` → merge left → `[1, 5, 4, 1]`, ops=1
* `l=2, r=3` → `5 > 4` → merge right → `[1, 9, 1]`, ops=2
* Palindrome achieved

**Output:** `2`

---