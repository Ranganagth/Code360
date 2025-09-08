# Intuition

* On day `i`, you can buy at most `i` shares.
* But since you want to maximize the total shares with limited money `K`, you should **buy cheaper shares first**.
* This means we should not just follow the day order, but rather:

  * Sort stock prices with their allowed maximum shares.
  * Buy as many as possible starting from the cheapest stock price.

---

# Approach

1. Create an array of pairs `(price, maxSharesAllowed)` for each day.

   * On day `i` (1-based), max shares = `i`.
2. Sort these pairs by price (ascending).
3. Traverse the sorted list:

   * For each `(price, maxAllowed)`:

     * Calculate `canBuy = min(maxAllowed, k / price)` → maximum shares affordable for this price.
     * Deduct `canBuy * price` from capital `k`.
     * Add `canBuy` to the total shares.
   * Stop if money runs out (`k == 0`).
4. Return total shares.

---

# Complexity

* Sorting: `O(N log N)`
* Traversal: `O(N)`
* Overall: **O(N log N)** per test case.
* Space: `O(N)`.
  Efficient enough for `N ≤ 10^5`.

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {

    public static int max_shares(int k, int n, int[] shares) {
        // Step 1: Create pairs (price, maxSharesAllowed)
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; i++) {
            arr[i][0] = shares[i];   // price
            arr[i][1] = i + 1;       // max shares allowed
        }

        // Step 2: Sort by price
        Arrays.sort(arr, (a, b) -> Integer.compare(a[0], b[0]));

        int totalShares = 0;

        // Step 3: Buy shares greedily
        for (int i = 0; i < n; i++) {
            int price = arr[i][0];
            int maxAllowed = arr[i][1];

            if (k < price) break; // can't buy even 1 share

            int canBuy = Math.min(maxAllowed, k / price);
            totalShares += canBuy;
            k -= canBuy * price;

            if (k == 0) break;
        }

        return totalShares;
    }
}
```

---

# Example Walkthrough

Input:

```
K = 45, N = 3
PRICES = [10, 7, 19]
```

Step 1: Make pairs with limits:

```
Day1 → (10, 1)
Day2 → (7, 2)
Day3 → (19, 3)
```

Step 2: Sort by price:

```
[(7, 2), (10, 1), (19, 3)]
```

Step 3: Buy in order:

* Price 7: can buy min(2, 45/7=6) = 2 shares → cost 14, left 31.
* Price 10: can buy min(1, 31/10=3) = 1 share → cost 10, left 21.
* Price 19: can buy min(3, 21/19=1) = 1 share → cost 19, left 2.

Total shares = 2 + 1 + 1 = **4**. 

---
