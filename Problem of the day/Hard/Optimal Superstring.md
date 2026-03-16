# Intuition

To build the **shortest superstring**, strings should overlap as much as possible.

Example:

```
ABC
CDE
```

Overlap = `"C"`

Merged string:

```
ABCDE
```

Since `N ≤ 14`, this can be solved using **bitmask DP similar to TSP**.

Key idea:

* Precompute overlap between every pair of strings.
* Use DP where `mask` represents which strings are used and `last` represents the last added string.
* Transition by adding a new string and subtracting the overlap.

# Approach

1. Precompute overlap `overlap[i][j]`
   = maximum suffix of `s[i]` matching prefix of `s[j]`.

2. Define DP:

```
dp[mask][last] = shortest length of superstring using mask ending with last
```

3. Transition:

```
dp[newMask][j] =
min(
 dp[newMask][j],
 dp[mask][i] + len[j] - overlap[i][j]
)
```

4. Initialize with each string alone.

5. Answer = minimum value among `dp[(1<<N)-1][i]`.

# Complexity

Time complexity:

```
O(N^2 * 2^N)
```

Space complexity:

```
O(N * 2^N)
```

---

# Code

```java
public class Solution {

    public static int optimalSuperstring(String[] s, int size) {

        int n = size;

        int[][] overlap = new int[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {

                if (i == j) continue;

                int max = Math.min(s[i].length(), s[j].length());

                for (int k = max; k >= 0; k--) {

                    if (s[i].substring(s[i].length() - k).equals(s[j].substring(0, k))) {
                        overlap[i][j] = k;
                        break;
                    }
                }
            }
        }

        int FULL = 1 << n;
        int[][] dp = new int[FULL][n];

        for (int i = 0; i < FULL; i++)
            for (int j = 0; j < n; j++)
                dp[i][j] = Integer.MAX_VALUE / 2;

        for (int i = 0; i < n; i++)
            dp[1 << i][i] = s[i].length();

        for (int mask = 1; mask < FULL; mask++) {

            for (int last = 0; last < n; last++) {

                if ((mask & (1 << last)) == 0) continue;

                int prevMask = mask ^ (1 << last);

                if (prevMask == 0) continue;

                for (int prev = 0; prev < n; prev++) {

                    if ((prevMask & (1 << prev)) == 0) continue;

                    dp[mask][last] = Math.min(
                            dp[mask][last],
                            dp[prevMask][prev] + s[last].length() - overlap[prev][last]
                    );
                }
            }
        }

        int ans = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++)
            ans = Math.min(ans, dp[FULL - 1][i]);

        return ans;
    }
};

```

---

# Example walkthrough

### Input

```
ABC
CDE
EFG
```

Overlaps:

```
ABC → CDE = 1
CDE → EFG = 1
```

Merge sequence:

```
ABC + CDE → ABCDE
ABCDE + EFG → ABCDEFG
```

Length:

```
7
```
