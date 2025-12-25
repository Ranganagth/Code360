# Intuition

Treat both interval lists like two sorted timelines. Walk through them from left to right. Whenever two intervals overlap, their common region forms one intersection. Because both lists are sorted and non-overlapping internally, a two-pointer sweep captures every intersection in linear time.

# Approach

1. Use two indices `i` and `j` for the two lists.
2. For current intervals `[a1, a2] = interval1[i]` and `[b1, b2] = interval2[j]`, the overlap (if any) is
   `start = max(a1, b1)` and `end = min(a2, b2)`.
   If `start <= end`, record `[start, end]`.
3. Advance the pointer whose interval ends first (smaller `a2` vs `b2`).
4. Continue until one list is exhausted.

---

# Complexity

- **Time:** `O(n1 + n2)`
- **Space:** `O(k)` for output, where `k` is number of intersections.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static ArrayList<ArrayList<Integer>> intersectionIntervals(
            ArrayList<ArrayList<Integer>> interval1,
            ArrayList<ArrayList<Integer>> interval2,
            int n1, int n2) {

        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        int i = 0, j = 0;

        while (i < n1 && j < n2) {
            int a1 = interval1.get(i).get(0);
            int a2 = interval1.get(i).get(1);
            int b1 = interval2.get(j).get(0);
            int b2 = interval2.get(j).get(1);

            int start = Math.max(a1, b1);
            int end   = Math.min(a2, b2);

            if (start <= end) {
                ArrayList<Integer> cur = new ArrayList<>();
                cur.add(start);
                cur.add(end);
                res.add(cur);
            }

            if (a2 < b2) i++;
            else j++;
        }

        return res;
    }
};

```

---

# Example walkthrough

**INTERVAL1** = [[2, 8], [12, 16]]
**INTERVAL2** = [[5, 9], [10, 12], [14, 15]]

* Compare [2,8] & [5,9] → overlap [5,8]. Move from [2,8] (ends first).
* Compare [12,16] & [5,9] → no overlap, move from [5,9].
* Compare [12,16] & [10,12] → overlap [12,12]. Move from [10,12].
* Compare [12,16] & [14,15] → overlap [14,15]. Move from [14,15].

**Result:** [[5,8], [12,12], [14,15]]
