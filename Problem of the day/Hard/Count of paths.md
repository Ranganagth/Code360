# Intuition

The array defines a directed acyclic graph because edges only go from lower index to higher index. A path to index `i` can only come from indices `j < i` where `arr[i] % arr[j] == 0`. The number of paths to `i` is the sum of paths to all such valid `j`. This is pure dynamic programming on a DAG.

Brute-force checking all `j < i` gives `O(N²)` and fails. Optimization comes from the fact that divisibility depends on values, not indices. We aggregate path counts by value and propagate them using divisors.

---

# Approach

1. `dp[i]` = number of paths from index `0` to index `i`.
2. `dp[0] = 1` always.
3. Maintain a map/array `count[v]` = total paths ending at value `v` seen so far.
4. Process indices from left to right:

   * For current value `x = arr[i]`
   * Enumerate all divisors `d` of `x`
   * Add `count[d]` to `dp[i]`
   * If `i == 0`, force `dp[0] = 1`
5. After computing `dp[i]`, update `count[x] += dp[i]`
6. Use modulo `1e9+7`.

Divisor enumeration is efficient because `arr[i] ≤ 1e5`.

---

# Complexity

**Time Complexity**
* Divisor enumeration per element: `O(√ai)`
* Total: `O(N √A)` where `A ≤ 1e5`

**Space Complexity**
* `O(A + N)`

---

# Code

```java
import java.util.*;

public class Solution {
    static final int MOD = 1_000_000_007;

    public static int[] countOfPaths(int n, int k, int[] arr) {
        int[] dp = new int[n];
        Map<Integer, Long> count = new HashMap<>();

        for (int i = 0; i < n; i++) {
            int x = arr[i];
            long ways = 0;

            for (int d = 1; d * d <= x; d++) {
                if (x % d == 0) {
                    if (count.containsKey(d)) {
                        ways = (ways + count.get(d)) % MOD;
                    }
                    int other = x / d;
                    if (other != d && count.containsKey(other)) {
                        ways = (ways + count.get(other)) % MOD;
                    }
                }
            }

            dp[i] = (i == 0) ? 1 : (int) ways;
            count.put(x, (count.getOrDefault(x, 0L) + dp[i]) % MOD);
        }
        return dp;
    }
};

```

---

# Example Walkthrough

**Input:**
`arr = [1, 2, 3, 4, 5, 6]`

Step-by-step:

* `i=0 (1)` → `dp[0]=1`, `count[1]=1`
* `i=1 (2)` → divisors `{1,2}` → paths = `count[1]=1` → `dp[1]=1`
* `i=2 (3)` → divisors `{1,3}` → paths = `count[1]=1` → `dp[2]=1`
* `i=3 (4)` → divisors `{1,2,4}` → paths = `1 + 1 = 2` → `dp[3]=2`
* `i=4 (5)` → divisors `{1,5}` → paths = `1` → `dp[4]=1`
* `i=5 (6)` → divisors `{1,2,3,6}` → paths = `1 + 1 + 1 = 3` → `dp[5]=3`

**Output:**

```
1 1 1 2 1 3
```
