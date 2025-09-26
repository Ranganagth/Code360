# Intuition

We need to insert all numbers from `keys` into a hash table of size `N = keys.size()` using:

* **Hash function**: `H(x) = x % N`
* **Collision handling**: If the target index is already filled, move forward `(index+1) % N` until an empty slot is found.

So basically, it’s **open addressing with linear probing**.

---

# Approach

1. Initialize an array `hashTable` of size `N`, filled with `-1` (indicating empty).
2. For each key `k` in `keys`:

   * Compute `idx = k % N`.
   * While `hashTable[idx]` is not `-1`, update `idx = (idx + 1) % N`.
   * Place `k` at `hashTable[idx]`.
3. Return the `hashTable`.

---

# Complexity

* Each element is inserted in **O(N)** worst-case (if table is nearly full).
* Total complexity: **O(N²)** in worst-case.
* But since `N ≤ 500`, this is efficient enough.


---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

    public static int[] linearProbing(List<Integer> keys) {
        int n = keys.size();
        int[] hashTable = new int[n];
        Arrays.fill(hashTable, -1);

        for (int key : keys) {
            int idx = key % n;
            // Linear probing
            while (hashTable[idx] != -1) {
                idx = (idx + 1) % n;
            }
            hashTable[idx] = key;
        }

        return hashTable;
    }
}
```

---

## Example walkthrough

Input:

```
N = 4
keys = [1, 5, 3, 7]
```

Steps:

* 1 → `1 % 4 = 1`, put at index 1 → `[ -1, 1, -1, -1 ]`
* 5 → `5 % 4 = 1`, collision → try index 2 → `[ -1, 1, 5, -1 ]`
* 3 → `3 % 4 = 3`, put at index 3 → `[ -1, 1, 5, 3 ]`
* 7 → `7 % 4 = 3`, collision → try index 0 → `[ 7, 1, 5, 3 ]`

Output = `[7, 1, 5, 3]`

---

