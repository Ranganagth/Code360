# Intuition

* You need to break the chocolate into `m × n` pieces (1×1 squares).
* Each time you cut:

  * If you cut **vertically**, you increase the number of vertical pieces by `1`, so **future horizontal cuts will cost more**.
  * If you cut **horizontally**, you increase the number of horizontal pieces by `1`, so **future vertical cuts will cost more**.

So, the order of cuts matters. To minimize cost:

* Always take the **most expensive cut first** (whether horizontal or vertical).
* Because the sooner you make an expensive cut, the fewer multipliers it gets (fewer pieces multiplied).

---

# Approach

1. Sort horizontal costs (`y`) in descending order.
2. Sort vertical costs (`x`) in descending order.
3. Maintain two counters:

   * `hzPieces = 1` (initially one horizontal piece).
   * `vtPieces = 1` (initially one vertical piece).
4. Use a greedy merge-like process:

   * At each step, choose the larger cut (`max(horizontalCost, verticalCost)`).
   * If horizontal cut is chosen: `cost += horizontalCost * vtPieces` and increase `hzPieces++`.
   * If vertical cut is chosen: `cost += verticalCost * hzPieces` and increase `vtPieces++`.
5. Continue until all cuts are consumed.

---

# Complexity

* Sorting: `O((m+n) log(m+n))`.
* Processing: `O(m+n)`.
* Total: **O((m+n) log(m+n))**.
* Space: `O(1)` (ignoring input storage).

---

# Code

```java
import java.util.* ;
import java.io.*; 
import java.util.ArrayList;

public class Solution {
    public static int minChocolatePiece(ArrayList<Integer> x, ArrayList<Integer> y, int m, int n) {
        // Sort in descending order
        Collections.sort(x, Collections.reverseOrder());
        Collections.sort(y, Collections.reverseOrder());

        int i = 0, j = 0;
        int hzPieces = 1, vtPieces = 1;
        int cost = 0;

        // Process cuts greedily
        while (i < x.size() && j < y.size()) {
            if (x.get(i) >= y.get(j)) {
                cost += x.get(i) * hzPieces;
                vtPieces++;
                i++;
            } else {
                cost += y.get(j) * vtPieces;
                hzPieces++;
                j++;
            }
        }

        // Remaining vertical cuts
        while (i < x.size()) {
            cost += x.get(i) * hzPieces;
            vtPieces++;
            i++;
        }

        // Remaining horizontal cuts
        while (j < y.size()) {
            cost += y.get(j) * vtPieces;
            hzPieces++;
            j++;
        }

        return cost;
    }
}
```

---

# Example Walkthrough

Input:

```
m = 6, n = 4
Horizontal = [2, 1, 3, 1, 4]
Vertical   = [4, 1, 2]
```

Step 1: Sort descending:

```
Horizontal: [4, 3, 2, 1, 1]
Vertical:   [4, 2, 1]
```

Step 2: Start cutting:

* Pick 4 (vertical): cost = 4\*1 = 4, vtPieces=2.
* Pick 4 (horizontal): cost = 4\*2=8, hzPieces=2.
* Pick 3 (horizontal): cost = 3\*2=6, hzPieces=3.
* Pick 2 (vertical): cost = 2\*3=6, vtPieces=3.
* Pick 2 (horizontal): cost = 2\*3=6, hzPieces=4.
* Pick 1 (horizontal): cost = 1\*3=3, hzPieces=5.
* Pick 1 (horizontal): cost = 1\*3=3, hzPieces=6.
* Pick 1 (vertical): cost = 1\*6=6, vtPieces=4.

Total = 42

---
