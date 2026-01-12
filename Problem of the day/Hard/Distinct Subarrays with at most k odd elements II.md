# Intuition

The problem has two constraints at the same time:

1. Subarray must contain **at most K odd numbers**.
2. Subarray must be **distinct by content**, not by position.

Brute-force counting of subarrays is easy, but distinctness means we cannot just count ranges. We must **identify subarrays by their sequence of values**.
Therefore, each valid subarray must be represented in a way that allows fast equality checks.

---

# Approach

Use a **double loop + rolling hash** strategy.

**Key ideas:**

* Fix a starting index `i`
* Extend the subarray to the right (`j = i … n-1`)
* Maintain a count of odd numbers
* Stop extending when odd count exceeds `K`
* For each valid subarray, compute a rolling hash
* Store hashes in a `HashSet` to ensure distinctness

**Why rolling hash:**

* Comparing arrays directly would be too slow
* Hashing converts each subarray into a unique fingerprint in `O(1)` time per extension

## Algorithm

1. Initialize an empty `HashSet<Long>` to store subarray hashes.
2. For each start index `i`:

   * `oddCount = 0`
   * `hash = 0`
   * For each end index `j >= i`:

     * If `arr[j]` is odd → increment `oddCount`
     * If `oddCount > K` → break
     * Update rolling hash with `arr[j]`
     * Insert hash into the set
3. Return the size of the set.

Rolling Hash Formula

```
hash = hash * BASE + value
```

Use a large base and 64-bit integer to reduce collision risk.

---

# Complexity

Let `N` be array length.

- **Time Complexity**
	* Outer loop: `O(N)`
	* Inner loop: `O(N)`
	* Total: `O(N²)`

- **Space Complexity**
	* HashSet stores up to `O(N²)` hashes in worst case

This fits within constraints (`N ≤ 3000`).

---

# Code

```java
import java.util.*;

public class Solution {

    public static int distinctSubKOdds(int n, int k, int[] arr) {
        Set<Long> set = new HashSet<>();
        final long BASE = 1000003L;

        for (int i = 0; i < n; i++) {
            int oddCount = 0;
            long hash = 0;

            for (int j = i; j < n; j++) {
                if ((arr[j] & 1) == 1) {
                    oddCount++;
                }
                if (oddCount > k) break;

                hash = hash * BASE + arr[j];
                set.add(hash);
            }
        }
        return set.size();
    }
};

```

---


# Example Walkthrough

**Example**

ARR = `[3, 2, 3]`, K = 1

**Subarrays examined:**

* `[3]` → 1 odd → valid
* `[3,2]` → 1 odd → valid
* `[3,2,3]` → 2 odds → invalid
* `[2]` → 0 odd → valid
* `[2,3]` → 1 odd → valid
* `[3]` → duplicate, ignored

**Distinct valid subarrays:**
`[3], [3,2], [2], [2,3]`

**Answer:** `4`

---
