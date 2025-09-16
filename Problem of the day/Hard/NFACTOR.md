# Intuition

We need to answer multiple queries where each query asks:

* Count of numbers in range `[A, B]` that have exactly `N` unique prime factors.

Naively checking each number for prime factors in every query would be too slow, since:

* `A, B` can be as large as `10^6` (assumed from problem constraints).
* `Q` (queries) can also be large.

So, the trick is **precomputation**:

* Precompute, for every number up to the maximum `B`, how many unique prime factors it has.
* Build prefix frequency arrays for each possible number of unique prime factors.
* Then, each query becomes an `O(1)` lookup.

---

# Approach

1. **Precompute unique prime factors for all numbers**:

   * Use a modified Sieve of Eratosthenes.
   * For every prime `p`, iterate over its multiples and increase their `primeFactorCount[multiple]`.
   * After sieve, `primeFactorCount[x]` gives number of unique prime factors of `x`.

2. **Build prefix counts**:

   * Let `prefix[n][i]` = number of integers ≤ `i` that have exactly `n` unique prime factors.
   * We only need up to around `N=10` (since a number ≤ `10^6` cannot have too many distinct primes).
   * Fill these prefix arrays in one pass.

3. **Answer queries**:

   * For each query `(A, B, N)`:

     ```
     result = prefix[N][B] - prefix[N][A-1]
     ```

---

# Complexity

* **Precomputation** (modified sieve): `O(N log log N)` where `N = max(B)` across queries.
* **Prefix building**: `O(N * maxFactors)` where `maxFactors` is small (≤ 7 for `10^6`).
* **Each query**: `O(1)`.
* **Total**: Efficient for large input sizes.

---

# Code

```java
import java.util.*;

public class Solution {
    
    public static ArrayList<Integer> getNfactor(ArrayList<ArrayList<Integer>> queries) {
        int maxB = 0;
        for (ArrayList<Integer> q : queries) {
            maxB = Math.max(maxB, q.get(1));
        }
        
        // Step 1: Precompute number of unique prime factors for each number
        int[] primeFactorCount = new int[maxB + 1];
        
        for (int i = 2; i <= maxB; i++) {
            if (primeFactorCount[i] == 0) { // prime
                for (int j = i; j <= maxB; j += i) {
                    primeFactorCount[j]++;
                }
            }
        }
        
        // Step 2: Build prefix arrays for up to 10 unique factors (safe upper bound)
        int maxFactors = 10;
        int[][] prefix = new int[maxFactors + 1][maxB + 1];
        
        for (int i = 1; i <= maxB; i++) {
            for (int f = 0; f <= maxFactors; f++) {
                prefix[f][i] = prefix[f][i - 1];
            }
            int cnt = primeFactorCount[i];
            if (cnt <= maxFactors) {
                prefix[cnt][i]++;
            }
        }
        
        // Step 3: Answer queries
        ArrayList<Integer> ans = new ArrayList<>();
        for (ArrayList<Integer> q : queries) {
            int A = q.get(0);
            int B = q.get(1);
            int N = q.get(2);
            
            if (N > maxFactors) {
                ans.add(0);
                continue;
            }
            
            int res = prefix[N][B] - (A > 1 ? prefix[N][A - 1] : 0);
            ans.add(res);
        }
        
        return ans;
    }

    // For local testing
    public static void main(String[] args) {
        ArrayList<ArrayList<Integer>> queries = new ArrayList<>();
        queries.add(new ArrayList<>(Arrays.asList(3, 8, 2)));
        queries.add(new ArrayList<>(Arrays.asList(1, 8, 1)));
        
        System.out.println(getNfactor(queries)); // [1, 6]
    }
}
```

---

## **Example Walkthrough**

Query set:

```
2
3 8 2
1 8 1
```

1. Precompute prime factor counts:

   * 2 → {2} → 1
   * 3 → {3} → 1
   * 4 → {2} → 1
   * 5 → {5} → 1
   * 6 → {2,3} → 2
   * 7 → {7} → 1
   * 8 → {2} → 1

2. Build prefix arrays:

   * prefix\[1]\[8] = count of numbers ≤ 8 with 1 prime factor = 6
   * prefix\[2]\[8] = count of numbers ≤ 8 with 2 prime factors = 1

3. Queries:

   * \[3,8,2] → prefix\[2]\[8] - prefix\[2]\[2] = 1 - 0 = 1
   * \[1,8,1] → prefix\[1]\[8] - prefix\[1]\[0] = 6 - 0 = 6

Correct.

---
