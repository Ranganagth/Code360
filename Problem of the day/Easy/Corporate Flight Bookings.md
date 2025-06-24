# Intuition

To avoid repeatedly updating ranges in an array (which would cost O(N × M)), we can use a **difference array** technique:

- For a booking `[first, last, seats]`, we:
  - Add `seats` to `answer[first - 1]`
  - Subtract `seats` from `answer[last]` (if `last < n`)
- Finally, we take the prefix sum of the array to get the actual seat counts for each flight.

This reduces time complexity to **O(N + M)**.

---

# Approach

1. Initialize a result array `answer` of size `n` with all 0s.
2. For each booking:
   - Increment `answer[first - 1]` by `seats`
   - Decrement `answer[last]` by `seats` (if `last < n`)
3. Compute the prefix sum over `answer` to get the actual values.

---

# Complexity Analysis

- **Time:** O(N + M)
- **Space:** O(N)

---

# Code

```java
import java.util.*;

public class Solution {
    public static ArrayList<Integer> corpFlightBookings(ArrayList<ArrayList<Integer>> bookings, int n) {
        ArrayList<Integer> answer = new ArrayList<>(Collections.nCopies(n, 0));

        for (ArrayList<Integer> booking : bookings) {
            int first = booking.get(0) - 1;
            int last = booking.get(1) - 1;
            int seats = booking.get(2);

            answer.set(first, answer.get(first) + seats);

            if (last + 1 < n) {
                answer.set(last + 1, answer.get(last + 1) - seats);
            }
        }

        for (int i = 1; i < n; i++) {
            answer.set(i, answer.get(i) + answer.get(i - 1));
        }

        return answer;
    }
}
```

---

### Example

**Input:**
```
N = 4
Bookings = [ [1, 2, 3], [2, 3, 2], [1, 3, 1], [3, 4, 2] ]
```

**Output:**
```
[4, 6, 5, 2]
```
