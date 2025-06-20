### Problem
Find the **length of the longest subarray** such that the **absolute difference** between the **maximum** and **minimum** elements is **≤ LIMIT**.

---

# Intuition
We want a **contiguous subarray** where:
```
max(subarray) - min(subarray) <= LIMIT
```

To efficiently track `max` and `min` in a sliding window, we can use:
- A **deque** for maintaining **min values**
- A **deque** for maintaining **max values**

---

# Approach
1. Use two **deques**:
   - `minDeque` — stores increasing values (front is min)
   - `maxDeque` — stores decreasing values (front is max)

2. Use **two pointers** `left` and `right` to maintain the sliding window.

3. For each `right`:
   - Insert current element into both deques in order.
   - While `max - min > LIMIT`, shrink window by moving `left`.

4. Track the **maximum window size** where the condition is satisfied.

---

# Complexity
- **Time Complexity:** `O(N)`  
  Every element is added and removed at most once from both deques.

- **Space Complexity:** `O(N)`  
  For the two deques in the worst case.

---

# Code

```java
import java.util.*;

public class Solution {
    public static int getLongestSubarrayLength(ArrayList<Integer> arr, int limit) {
        int n = arr.size();
        Deque<Integer> minDeque = new LinkedList<>();
        Deque<Integer> maxDeque = new LinkedList<>();

        int left = 0, right = 0, maxLen = 0;

        while (right < n) {
            int current = arr.get(right);

            // Maintain increasing order for minDeque
            while (!minDeque.isEmpty() && arr.get(minDeque.peekLast()) > current) {
                minDeque.pollLast();
            }
            minDeque.offerLast(right);

            // Maintain decreasing order for maxDeque
            while (!maxDeque.isEmpty() && arr.get(maxDeque.peekLast()) < current) {
                maxDeque.pollLast();
            }
            maxDeque.offerLast(right);

            // Shrink window if condition not satisfied
            while (arr.get(maxDeque.peekFirst()) - arr.get(minDeque.peekFirst()) > limit) {
                left++;
                if (minDeque.peekFirst() < left) minDeque.pollFirst();
                if (maxDeque.peekFirst() < left) maxDeque.pollFirst();
            }

            maxLen = Math.max(maxLen, right - left + 1);
            right++;
        }

        return maxLen;
    }
}
```

---

### Example Usage
For:
```java
arr = [3, 6, 5, 4, 1], limit = 2
```
Output is `3` → subarray `[6, 5, 4]`
