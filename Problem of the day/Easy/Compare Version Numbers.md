# Intuition

When comparing version numbers like `"1.2.4"` and `"1.2.3"`, we need to break them into segments using `.` as the delimiter. Each segment should then be compared as an integer (not as a string).

* If at any index, the part of version `A` is greater than version `B`, then `A` is newer → return `1`.
* If at any index, the part of version `A` is smaller than version `B`, then `B` is newer → return `-1`.
* If all parts are equal, the versions are the same → return `0`.

We must also handle cases where one version has fewer parts (e.g., `"1.0.0"` vs `"1"` should be treated as equal).

---

# Approach

1. Split both version strings `a` and `b` by `"."`.
2. Convert each part into an integer for comparison.
3. Traverse the parts one by one until the maximum length of both arrays:

   * If one array is shorter, treat missing values as `0`.
   * Compare integers part by part.
   * Return `1` if `a` > `b`, `-1` if `a` < `b`.
4. If all parts are equal, return `0`.

---

# Complexity

* **Time complexity:**
  Splitting takes `O(|A| + |B|)` and comparison is linear to the number of segments.
  So, **`O(|A| + |B|)`**.

* **Space complexity:**
  We store the split segments → **`O(|A| + |B|)`**.

- Let `L = max(|A|, |B|)` (total length of both strings).
- Splitting takes `O(L)`.
- Comparing each part (BigInteger) is proportional to its length, but total across all parts is still `O(L)`.
- Overall complexity: **O(L)**, which works for `10^5` input size.

---

# Code

```java
import java.util.*; 
import java.io.*; 
import java.math.BigInteger;

public class Solution {
    public static int compareVersions(String a, String b) {
        String[] partsA = a.split("\\.");
        String[] partsB = b.split("\\.");

        int n = partsA.length, m = partsB.length;
        int len = Math.max(n, m);

        for (int i = 0; i < len; i++) {
            // If index out of range, treat as 0
            BigInteger valA = (i < n) ? new BigInteger(partsA[i]) : BigInteger.ZERO;
            BigInteger valB = (i < m) ? new BigInteger(partsB[i]) : BigInteger.ZERO;

            int cmp = valA.compareTo(valB);
            if (cmp > 0) return 1;
            if (cmp < 0) return -1;
        }
        return 0;
    }

    // Example test runner
    public static void main(String[] args) {
        System.out.println(compareVersions("1.2.4", "1.2.3")); // 1
        System.out.println(compareVersions("10.2.2", "10.2.2")); // 0
        System.out.println(compareVersions("123.45", "123")); // 1
        System.out.println(compareVersions("1.0.0", "1")); // 0
    }
}
```

---

# Example Walkthrough

### Input:

```
1.2.4  
1.2.3
```

* Split `a = ["1","2","4"]`
* Split `b = ["1","2","3"]`
* Compare part by part:

  * `1 == 1`
  * `2 == 2`
  * `4 > 3` → return `1`

### Output:

```
1
```

---

