# Intuition:
To solve the **sliding window maximum** problem efficiently, we use a **Deque (double-ended queue)** which helps us maintain the **indices of useful elements** in the current window.

---

# Approach:
Use a deque to keep track of the **indices** of potential maximum elements.  
At every step:
- Remove elements **outside** the window from the front.
- Remove elements **smaller than the current** from the back.
- Add the current element index.
- The **front of the deque** always holds the **index of the maximum element** for the current window.

---

# Complexity Analysis:
- Each element is pushed and popped **at most once** → **O(N)** per test case.

---

# Code:

```java
import java.util.*;

public class Solution {

    public static ArrayList<Integer> getMaximumOfSubarrays(ArrayList<Integer> nums, int k) {
        ArrayList<Integer> result = new ArrayList<>();
        Deque<Integer> deque = new LinkedList<>();
        int n = nums.size();

        for (int i = 0; i < n; i++) {
            // Remove elements out of the current window
            while (!deque.isEmpty() && deque.peekFirst() <= i - k)
                deque.pollFirst();

            // Remove elements smaller than current element from the back
            while (!deque.isEmpty() && nums.get(deque.peekLast()) < nums.get(i))
                deque.pollLast();

            // Add current element's index
            deque.offerLast(i);

            // Append max in window to result (start adding after window size reaches k)
            if (i >= k - 1)
                result.add(nums.get(deque.peekFirst()));
        }

        return result;
    }
}
```

---

### **Example:**
For input:  
`nums = [4, 2, 1, 4, 4], k = 2`  
The output is:  
`[4, 2, 4, 4]`
