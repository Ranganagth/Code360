# Intuition

Divisibility by 2 or 3 is independent per number.
Each element is either counted or ignored.
No ordering, no combinations, no side effects.
Scan once, accumulate conditionally.

---

# Approach

1. Initialize `sum = 0`.
2. Traverse the array.
3. For each number:
   * If `num % 2 == 0` OR `num % 3 == 0`, add it to `sum`.
4. Return `sum`.

---

# Complexity

* **Time:** `O(N)`
* **Space:** `O(1)`

---

# Code

```java
import java.util.ArrayList;

public class Solution {

    public static int findSum(int n, ArrayList<Integer> arr) {
        int sum = 0;

        for (int num : arr) {
            if (num % 2 == 0 || num % 3 == 0) {
                sum += num;
            }
        }

        return sum;
    }
};

```

---

# Example Walkthrough

**Input:**

```
N = 7
arr = [7, 5, 11, 3, 5, 2, 9]
```

**Step-by-step:**

* 7 → not divisible → ignore
* 5 → not divisible → ignore
* 11 → not divisible → ignore
* 3 → divisible by 3 → sum = 3
* 5 → not divisible → ignore
* 2 → divisible by 2 → sum = 5
* 9 → divisible by 3 → sum = 14

**Output:**

```
14
```
