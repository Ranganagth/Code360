# Intuition

`N ≤ 40` makes classic DP by sum infeasible because `K` can be `10^9`.
Brute force `2^40` is impossible.
This is a **meet-in-the-middle subset sum** problem.

Split the array into two halves.
Enumerate all subset sums of each half.
For every sum from the first half, choose the largest compatible sum from the second half such that total ≤ K.

This reduces `2^40` to `2^20 + 2^20`.

---

# Approach

1. Split array into two halves `A` and `B`
2. Generate all subset sums of `A` → `listA`
3. Generate all subset sums of `B` → `listB`
4. Sort `listB`
5. For each `sumA` in `listA`:

   * If `sumA > K`, skip
   * Binary search in `listB` for the largest `sumB ≤ K - sumA`
   * Update answer with `sumA + sumB`
6. Return maximum valid sum

---

# Complexity

* Subset generation: `O(2^(N/2))`
* Sorting: `O(2^(N/2) log 2^(N/2))`

* **Total Time:** `O(2^(N/2) log 2^(N/2))`
* **Space:** `O(2^(N/2))`

With `N = 40`, this is safe.

---

# Code

```java
import java.util.*;

public class Solution {

    public static int kSumSubset(int[] array, int k) {
        int n = array.length;
        int mid = n / 2;

        int[] left = Arrays.copyOfRange(array, 0, mid);
        int[] right = Arrays.copyOfRange(array, mid, n);

        ArrayList<Long> sumsLeft = generateSums(left);
        ArrayList<Long> sumsRight = generateSums(right);

        Collections.sort(sumsRight);

        long ans = 0;

        for (long s : sumsLeft) {
            if (s > k) continue;
            long remaining = k - s;
            int idx = upperBound(sumsRight, remaining);
            if (idx >= 0) {
                ans = Math.max(ans, s + sumsRight.get(idx));
            }
        }

        return (int) ans;
    }

    private static ArrayList<Long> generateSums(int[] arr) {
        int n = arr.length;
        int total = 1 << n;
        ArrayList<Long> sums = new ArrayList<>(total);

        for (int mask = 0; mask < total; mask++) {
            long sum = 0;
            for (int i = 0; i < n; i++) {
                if ((mask & (1 << i)) != 0) {
                    sum += arr[i];
                }
            }
            sums.add(sum);
        }
        return sums;
    }

    private static int upperBound(ArrayList<Long> list, long target) {
        int lo = 0, hi = list.size() - 1;
        int res = -1;

        while (lo <= hi) {
            int mid = (lo + hi) >>> 1;
            if (list.get(mid) <= target) {
                res = mid;
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        return res;
    }
};

```

---

# Example Walkthrough

**Input:**

```
arr = [1, 3, 5, 9]
K = 16
```

**Split:**

```
A = [1, 3]
B = [5, 9]
```

**Subset sums:**

```
A → [0, 1, 3, 4]
B → [0, 5, 9, 14]
```

**Sorted `B`:**

```
[0, 5, 9, 14]
```

**Check combinations:**

```
A=0 → best B=14 → sum=14
A=1 → best B=14 → sum=15
A=3 → best B=9  → sum=12
A=4 → best B=9  → sum=13
```

Maximum ≤ 16 → **15**

---
