# Intuition

Counting sort relies on **frequency counting instead of comparisons**.

Constraint includes negative numbers → direct indexing not possible.
Shift values using **offset = -minValue** to map all values to non-negative indices.

---

# Approach

1. Find:

   * `min` and `max` in array
2. Compute range:

   * `range = max - min + 1`
3. Create count array of size `range`
4. Fill frequency:

   * `count[arr[i] - min]++`
5. Rebuild sorted array:

   * Traverse count array
   * Place values back using index + min

---

# Complexity

* **Time complexity:**
  $$O(N + K)$$
  (where (K = max - min))

* **Space complexity:**
  $$O(K)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {

    public static int[] sort(int n, int arr[]) {

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for (int num : arr) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        int range = max - min + 1;
        int[] count = new int[range];

        // frequency count
        for (int num : arr) {
            count[num - min]++;
        }

        // rebuild sorted array
        int idx = 0;

        for (int i = 0; i < range; i++) {
            while (count[i] > 0) {
                arr[idx++] = i + min;
                count[i]--;
            }
        }

        return arr;
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

ARR = [-2, 1, 2, -1, 0]

min = -2, max = 2
range = 5

Mapping:
-2 → 0
-1 → 1
0 → 2
1 → 3
2 → 4

Count array:
[1, 1, 1, 1, 1]

Rebuild:
-2, -1, 0, 1, 2

---

## Example 2:

ARR = [1, 1, -1, -1]

min = -1, max = 1
range = 3

Count:
[-1 → 2, 0 → 0, 1 → 2]

Rebuild:
-1, -1, 1, 1
