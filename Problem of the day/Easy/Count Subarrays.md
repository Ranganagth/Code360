# Intuition

Whenever you have a consecutive block of identical elements (say length `k`), the number of subarrays formed **entirely within that block** is:

$$
\text{count} = \frac{k \times (k+1)}{2}
$$

Why?

* A block of size `k` has:

  * `k` subarrays of length `1`,
  * `k-1` subarrays of length `2`,
  * … up to `1` subarray of length `k`.
    So, total = `1 + 2 + … + k = k*(k+1)/2`.

Thus, the task reduces to:

1. Traverse the array.
2. Identify contiguous blocks of 0s or 1s.
3. For each block, add `len * (len+1) / 2` to the answer.

---

# Approach

1. Initialize `count = 0`.
2. Traverse array with a loop.

   * Maintain a `len` counter for consecutive same elements.
   * If current element differs from previous, calculate subarrays for the previous block (`len * (len+1) / 2`) and reset `len = 1`.
3. After the loop ends, don’t forget to add for the **last block**.
4. Return the total.

---

# Complexity

* **Time:** `O(N)` (single pass).
* **Space:** `O(1)` (constant extra memory).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int numberofSubarrays(int n, int[] arr) {
        long result = 0; // use long to avoid overflow for large n

        int len = 1; // length of current block
        for (int i = 1; i < n; i++) {
            if (arr[i] == arr[i - 1]) {
                len++;
            } else {
                // finish previous block
                result += (long) len * (len + 1) / 2;
                len = 1;
            }
        }
        // add last block
        result += (long) len * (len + 1) / 2;

        return (int) result; // constraints fit in int
    }
}
```

---

### Example Walkthrough

Input:

```
n = 7  
arr = [1, 0, 0, 0, 1, 0, 1]
```

* Block1: `[1]` → len=1 → contributes `1`.
* Block2: `[0,0,0]` → len=3 → contributes `3*4/2 = 6`.
* Block3: `[1]` → len=1 → contributes `1`.
* Block4: `[0]` → len=1 → contributes `1`.
* Block5: `[1]` → len=1 → contributes `1`.

Total = `1 + 6 + 1 + 1 + 1 = 10`.

Matches sample output.

---

# Code 2

```java
import java.util.*;

public class Solution {
    public static int numberofSubarrays(int n, int[] arr) {
        int total = 0;
        int count = 1;

        for (int i = 1; i < n; i++) {
            if (arr[i] == arr[i - 1]) {
                count++;
            } else {
                total += (count * (count + 1)) / 2;
                count = 1;
            }
        }

        total += (count * (count + 1)) / 2; // for last segment
        return total;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            int n = sc.nextInt();
            int[] arr = new int[n];

            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextInt();
            }

            System.out.println(numberofSubarrays(n, arr));
        }
    }
}
```
