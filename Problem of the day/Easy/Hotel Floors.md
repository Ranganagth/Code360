# Intuition

We need to track room bookings and calculate how many times a booking happens. Each `+` operation means a booking is made, which contributes 1 coin. Each `-` operation just frees a room and does not affect coin count. Since input guarantees that bookings and frees are valid, we don’t need to handle invalid cases.

So the task reduces to counting the number of `+` operations in the given list of queries.

---

# Approach

1. Initialize a counter `coins = 0`.
2. Iterate over all queries in the list.
3. If the query starts with `'+'`, increment `coins`.
4. Ignore `'-'` operations since they don’t contribute coins.
5. After processing all queries, return the total number of coins.

We don’t need to explicitly track room availability because the problem guarantees a valid sequence of bookings/frees.

---

# Complexity

* **Time complexity:** `O(n)` where `n` is the number of queries, since we scan each query once.
* **Space complexity:** `O(1)` as we only use a counter and no additional data structures.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int hotelBookings(List<String> queries) {
        int coins = 0;
        for (String query : queries) {
            if (query.charAt(0) == '+') {
                coins++;
            }
        }
        return coins;
    }
}
```

---

## **Example Walkthrough**

### Example 1:

Input: `["+1A", "+3E", "-1A", "+4F", "+1A", "-3E"]`

- `+1A` → coins = 1
- `+3E` → coins = 2
- `-1A` → no coins
- `+4F` → coins = 3
- `+1A` → coins = 4
- `-3E` → no coins

Output: **4**

---