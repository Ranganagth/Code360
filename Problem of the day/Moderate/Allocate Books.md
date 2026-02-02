# Intuition

You want to divide books (in order) among `m` students such that:

* Every student gets **at least one book**
* Books are assigned **contiguously**
* The **maximum pages assigned to any student is minimized**

This is a **minimize the maximum** problem → classic **Binary Search on Answer**.

### Key observations

* The **minimum possible answer** is:

  * `max(arr[i])` (a student must take at least one full book)
* The **maximum possible answer** is:

  * `sum(arr)` (one student takes all books)
* For any guessed maximum pages `X`, we can **check feasibility**:

  * Can we allocate books to ≤ `m` students such that no student gets more than `X` pages?

---

# Approach (Binary Search)

1. **Edge case**
   If `m > n`, allocation is impossible → return `-1`

2. **Binary Search Range**

   * `low = max(arr)`
   * `high = sum(arr)`

3. **Feasibility Check**

   * Traverse books in order
   * Accumulate pages for current student
   * If adding a book exceeds `mid`, assign books to next student
   * Count number of students required

4. **Binary Search Decision**

   * If students required ≤ `m` → try smaller maximum
   * Else → increase the limit

5. The final `low` is the **minimum possible maximum pages**

---

# Complexity

* **Time Complexity:**
$$  O(n \cdot \log(\text{sum of pages}))
$$

* **Space Complexity:**
$$  O(1)$$

---

# Code

```java
import java.util.ArrayList;

public class Solution {

    public static int findPages(ArrayList<Integer> arr, int n, int m) {

        // If students are more than books, allocation is impossible
        if (m > n) {
            return -1;
        }

        int low = 0;
        int high = 0;

        // Find max book and total sum
        for (int pages : arr) {
            low = Math.max(low, pages);
            high += pages;
        }

        int answer = -1;

        // Binary search on answer
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(arr, n, m, mid)) {
                answer = mid;
                high = mid - 1; // try smaller maximum
            } else {
                low = mid + 1; // need larger maximum
            }
        }

        return answer;
    }

    // Check if allocation is possible with maxPages limit
    private static boolean isPossible(ArrayList<Integer> arr, int n, int m, int maxPages) {
        int students = 1;
        int pagesSum = 0;

        for (int pages : arr) {
            if (pagesSum + pages <= maxPages) {
                pagesSum += pages;
            } else {
                students++;
                pagesSum = pages;

                if (students > m) {
                    return false;
                }
            }
        }

        return true;
    }
};

```

---

# Example Walkthrough

### Input

```
arr = [12, 34, 67, 90]
n = 4
m = 2
```

### Binary Search Range

```
low = 90
high = 203
```

### Feasibility Checks

| mid | Possible? | Reason                 |      |
| --- | --------- | ---------------------- | ---- |
| 146 | No        | Needs 3 students       |      |
| 113 | Yes       | Allocation: [12,34,67] | [90] |
| 101 | No        | Needs 3 students       |      |

### Final Answer

```
113
```

---

## Why This Works

* Uses **greedy allocation** for feasibility
* Uses **binary search** to minimize the worst-case student load
* Matches expected constraints and time limits

---
