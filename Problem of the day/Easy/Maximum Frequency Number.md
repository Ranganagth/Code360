## Problem
We need to:
* Find the element with **maximum frequency**.
* If multiple elements have the same frequency, return the one that **appears first** in the array.

---

# Intuition

We can use a frequency map (`HashMap<Integer, Integer>`) to count occurrences of each element. Then, while iterating over the array, we check:
* If the current element’s frequency is higher than the maximum so far → update answer.
* If frequency is the same but the element appeared **earlier** in the array, keep the earlier one.

Since we iterate in order, the **first occurrence preference** is naturally handled.

---

# Approach

1. Create a `HashMap` to store frequency of each number.
2. Traverse the array and populate the frequency map.
3. Traverse the array again (in input order):
   * Track the maximum frequency seen so far.
   * If an element’s frequency is higher than the current max, update the result.
   * If equal, skip (since the earlier one is already stored).
4. Return the stored element.

---

# Complexity

* **Time Complexity:** `O(N)` per test case (one pass to count, one pass to decide).
* **Space Complexity:** `O(N)` in worst case for the frequency map.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int maxFrequencyNumber(int n, int[] arr) {
        // Step 1: Count frequencies
        HashMap<Integer, Integer> freq = new HashMap<>();
        for (int num : arr) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        // Step 2: Find element with max frequency (respecting order)
        int maxFreq = 0;
        int result = arr[0]; // default to first element

        for (int num : arr) {
            int count = freq.get(num);
            if (count > maxFreq) {
                maxFreq = count;
                result = num;
            }
            // No need to handle equal count separately,
            // because the first element will naturally stay due to order of iteration
        }

        return result;
    }
}
```

---
