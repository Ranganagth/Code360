# Intuition

* A transaction always transfers **exactly `K` money** from one friend to another.
* So, every wallet balance can only change in **multiples of `K`**.
* To make all balances equal, all friends must be able to reach the **same final balance** using steps of size `K`.

Key observations:

1. Let the final equal balance be `X`.
2. The **total money** must remain the same →
   `X = (sum of all balances) / N`
3. If `sum % N != 0`, equal distribution is impossible.
4. For any friend `i`, `(arr[i] - X)` **must be divisible by `K`**.

   * Otherwise, we can never reach `X` using ±`K` transfers.
5. Each transaction moves `K` money from a surplus friend to a deficit friend.

   * So we only need to count how many **K-sized units** must be moved.

---

# Approach

1. Compute total sum of wallet balances.
2. If `sum % N != 0`, return `-1`.
3. Let `target = sum / N`.
4. For each friend:

   * Check if `(arr[i] - target) % K != 0` → return `-1`
   * If `arr[i] > target`, they have surplus
     → add `(arr[i] - target) / K` to transaction count
5. The total surplus units equals the **minimum number of transactions**.

Why this works:

* Each transaction consumes exactly **one surplus unit (`K`)**
* Deficits are automatically satisfied by surplus transfers

---

# Complexity 

- **Time Complexity**
	* **O(N)** per test case

- **Space Complexity**
	* **O(1)** extra space

---

# Code

```java
public class Solution {
    public static int minTransactions(int k, int[] arr) {

        int n = arr.length;
        long sum = 0;

        for (int val : arr) {
            sum += val;
        }

        // If total cannot be equally divided
        if (sum % n != 0) {
            return -1;
        }

        long target = sum / n;
        int transactions = 0;

        for (int val : arr) {
            long diff = val - target;

            // Must be adjustable in steps of K
            if (diff % k != 0) {
                return -1;
            }

            // Count only surplus contributions
            if (diff > 0) {
                transactions += diff / k;
            }
        }

        return transactions;
    }
};

```

---

# Example Walkthrough

#### Example 1

```
ARR = [4, 6, 6, 8], K = 2
```

* Sum = 24, N = 4 → target = 6
* Differences:

  * 4 → -2
  * 6 → 0
  * 6 → 0
  * 8 → +2 → 1 surplus unit
* Transactions = **1**

#### Example 2

```
ARR = [4, 2, 6], K = 3
```

* Sum = 12, N = 3 → target = 4
* Differences:

  * 4 → 0
  * 2 → -2 (not divisible by 3)
* Impossible → **-1**

---

### Final Insight

* Equalization is only possible if **every imbalance is a multiple of `K`**
* The answer is simply the **total number of surplus `K` units**
* Clean, greedy, and optimal in linear time
