# Intuition

You have a fence with `N` sections and `Q` painters.
Each painter paints a **range** `[L, R]` of sections.

You must **fire any 2 painters** (i.e., choose only `Q-2` painters) so that the **number of painted sections is maximized**.

This means:

* You initially have `Q` painters who together paint some number of sections.
* If you remove any two painters, some sections may become **unpainted** (if those sections were only painted by the removed painters).
* You need to find **which two painters, when removed, cause the least loss** in total painted sections.

---

# Approach

### Step 1. Represent painting coverage

Use an array `paintCount[]` of size `N+1`, where:

* `paintCount[i]` = number of painters that paint section `i`.

We can compute this efficiently using a **difference array technique**:

```java
for each painter [L, R]:
    diff[L] += 1
    diff[R+1] -= 1
```

Then prefix sum over `diff` gives us the final `paintCount`.

### Step 2. Compute total initially painted sections

Count how many indices `i` have `paintCount[i] > 0`.

Let that be `totalPainted`.

### Step 3. Precompute impact of removing each painter

We must know, for each painter:

* Which sections become **unpainted** if that painter is removed.

We can do this by marking how many painters paint each section.
Then for each painter `[L, R]`, count how many of those sections have `paintCount[i] == 1`, because removing that painter alone would make those sections **unpainted**.

Store this count as `uniqueContribution[i]`.

### Step 4. Compute effect of removing any two painters

When we remove two painters `(i, j)`:

* The total loss =

  * `uniqueContribution[i]` (sections only painted by painter i)
  * `uniqueContribution[j]` (sections only painted by painter j)
  * plus sections that become unpainted **only if both painters are removed** (i.e., painted exactly twice and only by i and j).

To get this, we can simulate the removal for all pairs `(i, j)`.

### Step 5. Simulate removal for all pairs (O(Q² * N))

For each pair `(i, j)`:

* Count how many sections `k` satisfy:

  * `paintCount[k] == 1` and are in either i’s or j’s range
  * or `paintCount[k] == 2` and are in **both** i’s and j’s ranges

Loss = number of unpainted sections after removing both.
Painted sections = `totalPainted - loss`.

Keep track of the **maximum** painted sections after removing two painters.

Since `N ≤ 1000` and `Q ≤ 500`,
`O(Q² * N)` = `500² * 1000 = 2.5×10⁸`, which is near limit but fine for 1 second in Java if optimized.

---

# Complexity

* Building paint count: `O(Q + N)`
* For each pair `(i, j)`: checking `N` sections → `O(Q² * N)`
* Total: `O(Q² * N)`
  Works fine for given constraints.

Space: `O(N + Q)`

---

# Code

```java
import java.util.*;

public class Solution {
    public static int paintTheFence(ArrayList<ArrayList<Integer>> ranges, int n, int q) {
        int[] paintCount = new int[n + 2];
        int[][] painterRange = new int[q][2];

        // Step 1: difference array for all painters
        for (int i = 0; i < q; i++) {
            int l = ranges.get(i).get(0);
            int r = ranges.get(i).get(1);
            painterRange[i][0] = l;
            painterRange[i][1] = r;
            paintCount[l]++;
            if (r + 1 <= n) paintCount[r + 1]--;
        }

        // prefix sum to get actual paint count
        for (int i = 1; i <= n; i++) {
            paintCount[i] += paintCount[i - 1];
        }

        // Step 2: total painted sections
        int totalPainted = 0;
        for (int i = 1; i <= n; i++) {
            if (paintCount[i] > 0) totalPainted++;
        }

        int maxPainted = 0;

        // Step 3: Try removing two painters
        for (int i = 0; i < q; i++) {
            for (int j = i + 1; j < q; j++) {
                int[] temp = Arrays.copyOf(paintCount, n + 1);
                // remove painter i
                for (int k = painterRange[i][0]; k <= painterRange[i][1]; k++) temp[k]--;
                // remove painter j
                for (int k = painterRange[j][0]; k <= painterRange[j][1]; k++) temp[k]--;

                int paintedNow = 0;
                for (int k = 1; k <= n; k++) {
                    if (temp[k] > 0) paintedNow++;
                }

                maxPainted = Math.max(maxPainted, paintedNow);
            }
        }

        return maxPainted;
    }
};

```

---

## Example Walkthrough

**Input:**

```
7 5
1 4
4 5
5 6
6 7
3 5
```

### Step 1: Paint coverage

Each section’s paint count:

```
1: 1
2: 1
3: 2
4: 3
5: 3
6: 2
7: 1
```

Total painted = 7

### Step 2: Remove (1,2)

After removing painters 1 & 2:
Sections 1–4 and 4–5 affected → still all 7 painted.

### Step 3: Best combination (1,3,4)

All sections covered → output **7** 
## Output

```
7
```

---
