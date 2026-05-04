# Intuition

Maintain **disjoint, sorted intervals** of tracked ranges.

Operations:

* **addRange** → merge overlapping intervals
* **queryRange** → check if a single interval fully covers [left, right)
* **removeRange** → cut/remove parts from existing intervals

Efficient structure: **TreeMap<start, end>**

* Keys sorted
* Enables floor/ceiling lookups

---

# Approach

### Data Structure

`TreeMap<Integer, Integer> map`

* key = start
* value = end

### addRange(left, right)

1. Find interval with start ≤ left
2. Merge all overlapping intervals
3. Remove them
4. Insert merged interval

### queryRange(left, right)

1. Find interval with start ≤ left
2. Check if its end ≥ right

### removeRange(left, right)

1. Find overlapping intervals
2. For each:

   * Left portion → keep
   * Right portion → keep
   * Middle → remove

---

# Complexity

* **Time complexity:**
  $$O(\log N + K)$$ per operation

* **Space complexity:**
  $$O(N)$$

---

# Code

```Java
import java.util.*;
import java.io.*;

public class RangeModule {

    TreeMap<Integer, Integer> map;

    public RangeModule() {
        map = new TreeMap<>();
    }

    public void addRange(int left, int right) {

        Integer start = map.floorKey(left);

        if (start != null && map.get(start) >= left) {
            left = start;
            right = Math.max(right, map.get(start));
            map.remove(start);
        }

        Integer next = map.ceilingKey(left);

        while (next != null && next <= right) {
            right = Math.max(right, map.get(next));
            map.remove(next);
            next = map.ceilingKey(left);
        }

        map.put(left, right);
    }
    
    public boolean queryRange(int left, int right) {
        Integer start = map.floorKey(left);
        if (start == null) return false;
        return map.get(start) >= right;
    }
    
    public void removeRange(int left, int right) {

        Integer start = map.floorKey(left);

        if (start != null && map.get(start) > left) {
            int end = map.get(start);

            if (end > right) {
                map.put(right, end);
            }

            map.put(start, left);
        }

        Integer next = map.ceilingKey(left);

        while (next != null && next < right) {
            int end = map.get(next);

            if (end > right) {
                map.remove(next);
                map.put(right, end);
                break;
            }

            map.remove(next);
            next = map.ceilingKey(left);
        }
    }
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Operations:

1. addRange(10,20)
   → map = { [10,20) }

2. removeRange(14,16)
   → split:
   [10,14), [16,20)

3. queryRange(10,14)
   → covered → true

4. queryRange(13,15)
   → gap → false

5. queryRange(16,17)
   → covered → true

---

## Example 2:

1. addRange(3,5)
   → [3,5)

2. removeRange(2,5)
   → removes all → empty

3. queryRange(3,4)
   → not covered → false
