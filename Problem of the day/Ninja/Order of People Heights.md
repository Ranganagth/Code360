
## Core invariant: 

Taller people dominate constraints. Place people from tallest to shortest. For each person, their `infront` value equals the index they must occupy among remaining empty slots.

---

# Algorithm:

1. Pair `(height, infront)`.
2. Sort pairs by `height` descending.
3. Maintain a list (or array) of size `N` initialized with empty slots.
4. For each person in sorted order, insert their height at index `infront` among current list positions.
5. Resulting list is the queue.

This works because when processing a person, all already-placed people are taller, so `infront` directly maps to position.

---
# Complexity:

- `O(N log N)` due to sorting and list insertions.
- `N ≤ 10^4` fits.

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> findOrder(ArrayList<Integer> height, ArrayList<Integer> infront) {
        int n = height.size();

        List<int[]> people = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            people.add(new int[]{height.get(i), infront.get(i)});
        }

        // Sort by height descending
        people.sort((a, b) -> b[0] - a[0]);

        ArrayList<Integer> result = new ArrayList<>();

        for (int[] p : people) {
            result.add(p[1], p[0]);
        }

        return result;
    }
};

```

