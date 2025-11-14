# Intuition

Every cut multiplies its cost by the **number of segments** already formed in the *other* direction.
To minimize total cost, we always take the **highest cost cut available first** — exactly the same greedy principle as the classical *Minimum Cost to Cut a Board* problem.

The goal is to process the largest cuts when the multiplier is smallest.

---

# Approach

1. You receive two lists:

   * `rowCost` → horizontal cuts (size = L−1)
   * `colCost` → vertical cuts (size = W−1)
2. Sort both lists in **descending order**.
3. Maintain:

   * `horizontalSegments = 1`
   * `verticalSegments = 1`
4. Use two pointers to pick the **largest remaining cost** from either list:

   * If next row cut cost ≥ next column cut cost:

     * total += cost * verticalSegments
     * horizontalSegments++
   * Otherwise:

     * total += cost * horizontalSegments
     * verticalSegments++
5. Continue until all cuts are processed.

This greedy rule ensures minimum total cost.

---

# Complexity

* **Time complexity:** O(L log L + W log W)
  Due to sorting both cost arrays.
* **Space complexity:** O(1) additional space (sorting in-place allowed).

---

# Code

```java
import java.util.*;

public class Solution {

    public static int minimumCost(ArrayList<Integer> rowCost, ArrayList<Integer> colCost, int l, int w) {

        Collections.sort(rowCost, Collections.reverseOrder());
        Collections.sort(colCost, Collections.reverseOrder());

        int i = 0, j = 0;
        int horizontalSegments = 1;
        int verticalSegments = 1;
        long total = 0;

        while (i < rowCost.size() && j < colCost.size()) {
            if (rowCost.get(i) >= colCost.get(j)) {
                total += (long) rowCost.get(i) * verticalSegments;
                horizontalSegments++;
                i++;
            } else {
                total += (long) colCost.get(j) * horizontalSegments;
                verticalSegments++;
                j++;
            }
        }

        while (i < rowCost.size()) {
            total += (long) rowCost.get(i) * verticalSegments;
            i++;
        }

        while (j < colCost.size()) {
            total += (long) colCost.get(j) * horizontalSegments;
            j++;
        }

        return (int) total;
    }
};

```

---

# Example walkthrough with explanation

### **Input**

```
L = 3, W = 3
rowCost = [1, 2]
colCost = [2, 1]
```

After sorting (descending):

* rowCost = [2, 1]
* colCost = [2, 1]

Start:

* horizontalSegments = 1
* verticalSegments = 1
* total = 0

### Step-by-step

1. Pick max(2,2) → row cut = 2
   total += 2 * 1 = 2
   horizontalSegments = 2

2. Next max = col 2
   total += 2 * 2 = 4
   verticalSegments = 2
   total = 6

3. Next max = row 1
   total += 1 * 2 = 2
   horizontalSegments = 3
   total = 8

4. Last = col 1
   total += 1 * 3 = 3
   total = 11

### Final Output
**11**
