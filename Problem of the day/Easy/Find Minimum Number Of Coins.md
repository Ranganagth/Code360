# Intuition:
To solve the **Minimum Coins** problem with Indian currency denominations, we can use a **Greedy Approach**, since the Indian currency system allows for this to yield optimal results.

---

# Approach (Greedy Strategy):
Always pick the **largest coin** that is **less than or equal to** the remaining amount `n`, subtract it from `n`, and repeat the process until `n` becomes 0.

### Coin Denominations (Descending Order):
`[1000, 500, 100, 50, 20, 10, 5, 2, 1]`

---
# Complexity:
- **O(n)** in worst case if `n = 1, 1, 1,...` but since the coin values are large, actual iterations are very few.
- Efficient for $n <= 10^5$ as per constraints.

---

# Code:

```java
import java.util.*;

public class Solution {
    public static List<Integer> MinimumCoins(int n) {
        List<Integer> result = new ArrayList<>();
        int[] denominations = {1000, 500, 100, 50, 20, 10, 5, 2, 1};

        for (int coin : denominations) {
            while (n >= coin) {
                n -= coin;
                result.add(coin);
            }
        }

        return result;
    }
}
```

---

### **Example Walkthrough:**
For `n = 13`:
- Pick 10 → remaining = 3
- Pick 2 → remaining = 1
- Pick 1 → remaining = 0  
**Result:** `[10, 2, 1]`

For `n = 50`:  
**Result:** `[50]`

---
