# Intuition

Sort the ranges so containment checks become monotonic.
A range contains another if it starts earlier and ends later.
Sorting by start ascending and end descending groups large (outer) ranges first, making it simple to detect which contain others and which are contained.

---

# Approach

1. Store each range as **[start, end, index]** to preserve original ordering.
2. Sort by:

   * start ascending
   * if tie: end descending
3. Traverse sorted list:

   * Maintain **maxEndSeen**, the maximum end encountered so far.
   * If a range’s end is **<= maxEndSeen**, it is **contained by** a previous range.
   * Else update maxEndSeen.
4. To find which ranges **contain** others:

   * Traverse **from right to left** maintaining **minEndSeen**.
   * If current end is **>= minEndSeen**, it **contains** some later (smaller) range.
5. Restore answers to original order using stored indices.

---

# Complexity

* Time complexity:
  **O(N log N)** due to sorting
* Space complexity:
  **O(N)** for augmented range storage and result arrays

---

# Code

```java
import java.util.*;

public class Solution {
    public static int[][] nestedRangesCheck(int[][] ranges, int n) {

        int[][] arr = new int[n][3];
        for (int i = 0; i < n; i++) {
            arr[i][0] = ranges[i][0]; // start
            arr[i][1] = ranges[i][1]; // end
            arr[i][2] = i;            // original index
        }

        Arrays.sort(arr, (a, b) -> {
            if (a[0] != b[0]) return a[0] - b[0]; // start asc
            return b[1] - a[1];                  // end desc
        });

        int[] contains = new int[n];
        int[] containedBy = new int[n];

        int maxEndSeen = -1;
        for (int i = 0; i < n; i++) {
            int end = arr[i][1];
            int idx = arr[i][2];

            if (end <= maxEndSeen) {
                containedBy[idx] = 1;
            } else {
                maxEndSeen = end;
            }
        }

        int minEndSeen = Integer.MAX_VALUE;
        for (int i = n - 1; i >= 0; i--) {
            int end = arr[i][1];
            int idx = arr[i][2];

            if (end >= minEndSeen) {
                contains[idx] = 1;
            } else {
                minEndSeen = end;
            }
        }

        return new int[][]{contains, containedBy};
    }
};

```

---

# Example Walkthrough

Consider ranges:
`[3,8], [1,3], [7,8]`

### Step 1: Add indices

`[3,8,0], [1,3,1], [7,8,2]`

### Step 2: Sort by (start asc, end desc)

Sorted becomes:
`[1,3,1], [3,8,0], [7,8,2]`

### Step 3: Detect "contained by"

* maxEndSeen = -1
* [1,3]: end 3 > -1 → update maxEndSeen = 3
* [3,8]: end 8 > 3 → update maxEndSeen = 8
* [7,8]: end 8 ≤ 8 → mark containedBy[2] = 1

### Step 4: Detect "contains"

Traverse from right

* minEndSeen = ∞
* [7,8]: update minEndSeen = 8
* [3,8]: end 8 ≥ 8 → contains[0] = 1
* [1,3]: end 3 < 8 → update minEndSeen = 3

### Step 5: Restore order

contains  = `[1,0,0]`
containedBy = `[0,0,1]`
