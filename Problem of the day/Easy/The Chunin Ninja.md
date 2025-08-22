# Intuition

We need to find all Ninjas who are simultaneously:

1. **Minimum in their row**  
   (no one in the same row has a smaller skill level).  

2. **Maximum in their column**  
   (no one in the same column has a larger skill level).  

This is a typical "matrix saddle point" check.

---

# Approach

1. For each **row**, find the **minimum element(s)**.  
   - We store their values and positions `(i, j)`.  

2. For each **column**, precompute the **maximum element**.  
   - This allows O(1) checking later.  

3. For each candidate `(i, j)` that is **minimum in row**, check if `arr[i][j]` equals the **column maximum at j**.  
   - If yes, it’s a **Chunin Ninja**.  

4. Collect all such elements.  

---

# Complexity

- Finding all row minimums → **O(N*M)**.  
- Finding all column maximums → **O(N*M)**.  
- Checking candidates → ≤ O(N*M).  
- **Total: O(N*M)**, which is fine since sum of N*M ≤ 10^5.  

Space: **O(N + M)** (storing row min and column max).

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static Vector<Integer> chuninNinja(int n, int m, int arr[][]) {
        Vector<Integer> ans = new Vector<>();

        // Step 1: compute maximum in each column
        int[] colMax = new int[m];
        Arrays.fill(colMax, Integer.MIN_VALUE);

        for (int j = 0; j < m; j++) {
            for (int i = 0; i < n; i++) {
                colMax[j] = Math.max(colMax[j], arr[i][j]);
            }
        }

        // Step 2: For each row, check its minimum elements
        for (int i = 0; i < n; i++) {
            int rowMin = Integer.MAX_VALUE;
            for (int j = 0; j < m; j++) {
                rowMin = Math.min(rowMin, arr[i][j]);
            }

            // Step 3: Find all occurrences of rowMin in this row
            for (int j = 0; j < m; j++) {
                if (arr[i][j] == rowMin && arr[i][j] == colMax[j]) {
                    ans.add(arr[i][j]);  // Chunin Ninja found
                }
            }
        }

        return ans;
    }
}
```

---

## **Example Walkthrough**

### Example 1:
```
N = 3, M = 3
ARR = [ [3, 4, 5], 
        [2, 7, 6], 
        [1, 2, 4] ]
```

- Column maximums:  
  col0 = 3, col1 = 7, col2 = 6  

- Row 0 min = 3 → check position (0,0):  
  arr[0][0] = 3, colMax[0] = 3 → valid Chunin.  

- Row 1 min = 2 → arr[1][0] = 2 but colMax[0] = 3 → not valid.  

- Row 2 min = 1 → arr[2][0] = 1 but colMax[0] = 3 → not valid.  

**Answer = [3]**  

---

### Example 2:
```
N = 2, M = 3
ARR = [ [3, 4, 5],
        [4, 5, 6] ]
```

- Column maximums:  
  col0 = 4, col1 = 5, col2 = 6  

- Row 0 min = 3 → check arr[0][0]=3 but colMax[0]=4 → not valid.  
- Row 1 min = 4 → check arr[1][0]=4, colMax[0]=4 → valid Chunin.  

**Answer = [4]**

---
