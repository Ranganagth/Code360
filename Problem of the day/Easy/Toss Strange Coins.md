# Intuition

Each coin is independent but biased. You need the probability that **exactly** `TARGET` coins show heads. This is a standard probability-DP: build the answer incrementally. After processing `i` coins, keep the probability of having exactly `j` heads. For each new coin, either it contributes a head or it doesn’t.

---

# Approach

Use 1-D DP of size `TARGET + 1`.
`dp[j]` = probability of getting exactly `j` heads after processing some coins.
Initialize: `dp[0] = 1` (zero coins → zero heads with probability 1).
Iterate each probability `p`: update `dp` **backward**:

`new_dp[j] = dp[j] * (1 - p) + dp[j-1] * p` for `j ≥ 1`
`new_dp[0] = dp[0] * (1 - p)`

Backward iteration prevents overwriting values needed in the same step.

**Answer** = `dp[target]`.

---

# Complexity

- **Time:** `O(N * TARGET)`
- **Space:** `O(TARGET)`

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {
    public static double tossStrangeCoins(ArrayList<Double> prob, int target) {

        int n = prob.size();
        double[] dp = new double[target + 1];

        dp[0] = 1.0;   // probability of zero heads initially

        for (int i = 0; i < n; i++) {
            double p = prob.get(i);

            // update from right to left
            for (int j = target; j >= 0; j--) {
                double stayTail = dp[j] * (1 - p);
                double becomeHead = (j > 0 ? dp[j - 1] * p : 0.0);
                dp[j] = stayTail + becomeHead;
            }
        }
        return dp[target];
    }
};

```

---

# Example walkthrough

**Input:** `prob = [0.5, 0.2, 0.3]`, `target = 0`

**Start:** dp = [1]

Coin 1 (0.5): dp[0] = 1 * 0.5 = 0.5
Coin 2 (0.2): dp[0] = 0.5 * 0.8 = 0.4
Coin 3 (0.3): dp[0] = 0.4 * 0.7 = 0.28

**Result:** `0.28`
