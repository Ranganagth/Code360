# Intuition

We are given a 2D array (or list of tuples), and we must **sort it based on the last element of each tuple** in **non-decreasing order**.
If two tuples have the same last element, we must keep them in the **original order** — this means we need a **stable sort**.

In Java, the built-in `Arrays.sort()` with a **custom comparator** is **stable for object arrays** (not primitive arrays).
Since we have `int[][] arr`, each element is an `int[]` (an object), so `Arrays.sort()` will maintain stability automatically.

---

# Approach

1. For each tuple `arr[i]`, the last element is `arr[i][arr[i].length - 1]`.
2. Sort `arr` using a comparator that compares tuples based on their last element.
3. Because Java's sort for object arrays is stable, tuples with equal last elements will remain in their original order.

## Algorithm

```text
1. Input: 2D integer array 'arr' of size n x l
2. Use Arrays.sort() with a comparator that compares arr[i][l - 1]
3. Done — print or return the sorted array
```

---

# Complexity

* **Time Complexity:** O(N * log N)
* **Space Complexity:** O(1) (in-place sort)

---

# Code

```java
import java.util.*;

public class Solution {
    public static void sortTuples(int[][] arr) {
        Arrays.sort(arr, (a, b) -> Integer.compare(a[a.length - 1], b[b.length - 1]));
    }
}
```

---

## Example Walkthrough

### Input

```
arr = [
  [1, 2],
  [1, 1],
  [3, 5],
  [2, 3]
]
```

### Process

* Last elements = [2, 1, 5, 3]
* Sorted order by last elements = [1, 2, 3, 5]

Result:

```
[1, 1]
[1, 2]
[2, 3]
[3, 5]
```

---

**Output:**

```
1 1
1 2
2 3
3 5
```

This matches the expected sample output.
