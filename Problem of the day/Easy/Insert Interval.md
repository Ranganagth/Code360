# Intuition

We are given a sorted list of **non-overlapping intervals**. When inserting a new interval, three cases arise:

1. **No Overlap Before**: If the new interval ends before the current interval starts, we simply add the new interval and then the rest.
2. **No Overlap After**: If the new interval starts after the current interval ends, we keep adding the current interval.
3. **Overlap Exists**: If the intervals overlap, we merge them by updating the new interval’s start as the minimum of both starts and its end as the maximum of both ends.

At the end, if the new interval hasn’t been added, we append it.

---

# Approach

1. Initialize a result list.
2. Traverse through the given intervals:

   * If current interval ends before the new interval starts → add current interval to result.
   * Else if current interval starts after the new interval ends → add new interval to result (only once) and continue adding remaining intervals.
   * Else (overlap case) → merge intervals by updating newInterval’s start and end.
3. After traversal, if the newInterval is not yet added, append it.
4. Return the result.

---

# Complexity

* **Time Complexity**: `O(N)` – Each interval is processed once.
* **Space Complexity**: `O(N)` – For storing the result.

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static ArrayList<ArrayList<Integer>> insertInterval(ArrayList<ArrayList<Integer>> intervals, ArrayList<Integer> newInterval) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        int i = 0, n = intervals.size();
        int newStart = newInterval.get(0), newEnd = newInterval.get(1);

        // Step 1: Add all intervals ending before newInterval starts
        while (i < n && intervals.get(i).get(1) < newStart) {
            result.add(intervals.get(i));
            i++;
        }

        // Step 2: Merge overlapping intervals
        while (i < n && intervals.get(i).get(0) <= newEnd) {
            newStart = Math.min(newStart, intervals.get(i).get(0));
            newEnd = Math.max(newEnd, intervals.get(i).get(1));
            i++;
        }
        result.add(new ArrayList<>(Arrays.asList(newStart, newEnd)));

        // Step 3: Add remaining intervals
        while (i < n) {
            result.add(intervals.get(i));
            i++;
        }

        return result;
    }
}
```

---

