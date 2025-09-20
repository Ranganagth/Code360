# Intuition

- Use **two pointers**:
  - One pointer `insertPos` tells us where to put the next non-zero element.
  - The other pointer `i` traverses the array.
- Every time we see a non-zero, we place it at `insertPos` and increment `insertPos`.
- After finishing, from `insertPos` to end, fill with `0`s.

This ensures:
- Relative order of non-zero elements is maintained.
- Zeroes are shifted to the right.

---

# Approach

1. Initialize `insertPos = 0`.
2. Loop through the array:
   - If `arr[i] != 0`, put `arr[i]` at `arr[insertPos]`, increment `insertPos`.
3. After loop, from `insertPos` to `arr.size()-1`, set values to `0`.

---

# Complexity

- **Time:** `O(n)` (single pass over array + filling zeros).
- **Space:** `O(1)` (in-place, no extra data structures).

---

# Code

```java
import java.util.*;

public class Solution {
    public static void pushZerosAtEnd(ArrayList<Integer> arr) {
        int n = arr.size();
        int insertPos = 0;

        // Place non-zero elements in order
        for (int i = 0; i < n; i++) {
            if (arr.get(i) != 0) {
                arr.set(insertPos, arr.get(i));
                insertPos++;
            }
        }

        // Fill remaining positions with 0
        while (insertPos < n) {
            arr.set(insertPos, 0);
            insertPos++;
        }
    }
}
```

---

## Example Walkthrough

Input:  
`[0, 1, -2, 3, 4, 0, 5, -27, 9, 0]`

Steps:  
- After first pass (shifting non-zeros): `[1, -2, 3, 4, 5, -27, 9, 9, 9, 0]` (garbage values, will fix in next loop).  
- After filling zeroes: `[1, -2, 3, 4, 5, -27, 9, 0, 0, 0]`.

Correct output.

---
