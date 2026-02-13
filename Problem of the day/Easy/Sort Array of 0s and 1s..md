# Intuition

Since the array contains only `0` and `1`, sorting in a single traversal can be achieved by moving all `0`s to the left as they are encountered. Maintain the position where the next `0` should be placed and swap whenever a `0` appears.

---

# Approach

1. Maintain pointer `zeroIndex` indicating the next position where a `0` should be placed.
2. Traverse the array once:

   * If the current element is `0`, swap it with the element at `zeroIndex`.
   * Increment `zeroIndex`.
3. This ensures all `0`s move to the left and all `1`s automatically stay on the right.
4. Sorting happens in-place and in one pass.

---

# Complexity

* **Time complexity:** (O(n))
* **Space complexity:** (O(1))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
  
    public static void sort0and1(int n, int[] A) {
    
        int zeroIndex = 0;

        for (int i = 0; i < n; i++) {
            if (A[i] == 0) {
                int temp = A[i];
                A[i] = A[zeroIndex];
                A[zeroIndex] = temp;
                zeroIndex++;
            }
        }
    }
};

```

---

# Example walkthrough with explanation

Input: `[0 1 1 1 0 0 1]`

Process:

* Encounter `0` at index 0 → swap with index 0
* Encounter `0` at index 4 → swap with index 1
* Encounter `0` at index 5 → swap with index 2

Final array: `[0 0 0 1 1 1 1]`.
