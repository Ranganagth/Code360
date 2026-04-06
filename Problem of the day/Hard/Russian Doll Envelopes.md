# Intuition

Two dimensions impose strict ordering. Direct DP on pairs leads to (O(n^2)), which violates constraints.

Transform the problem:

* Sort envelopes by height ascending.
* For equal heights, sort width in **descending order** to prevent invalid nesting.
* Now the problem reduces to finding **Longest Increasing Subsequence (LIS)** on widths.

Reason:
After sorting, height condition is already handled. Only width needs strict increase.

---

# Approach

1. Create pairs: (height[i], width[i])
2. Sort:

   * height ↑
   * width ↓ (if heights equal)
3. Extract widths.
4. Apply LIS (binary search optimization):

   * Maintain a list `dp`
   * For each width:

     * Use binary search to find position
     * Replace or append
5. Length of `dp` = answer

---

# Complexity

* **Time complexity:**
  $$O(n \log n)$$

* **Space complexity:**
  $$O(n)$$

---

# Code

```Java
import java.util.*;

public class Solution {
	public static int findMaxEnvelopes(ArrayList<Integer> height, ArrayList<Integer> width, int n) {

		int[][] env = new int[n][2];

		for (int i = 0; i < n; i++) {
			env[i][0] = height.get(i);
			env[i][1] = width.get(i);
		}

		Arrays.sort(env, (a, b) -> {
			if (a[0] == b[0]) return b[1] - a[1]; // width descending
			return a[0] - b[0]; // height ascending
		});

		ArrayList<Integer> dp = new ArrayList<>();

		for (int i = 0; i < n; i++) {
			int w = env[i][1];

			int idx = Collections.binarySearch(dp, w);
			if (idx < 0) idx = -(idx + 1);

			if (idx == dp.size()) {
				dp.add(w);
			} else {
				dp.set(idx, w);
			}
		}

		return dp.size();
	}
}
```

---

# Example Walkthrough

## Example 1: Step by step explanation

Input:
height = [5, 6, 6, 2]
width  = [4, 4, 7, 3]

Step 1: Pair
(5,4), (6,4), (6,7), (2,3)

Step 2: Sort
(2,3), (5,4), (6,7), (6,4)

Step 3: Extract widths
[3, 4, 7, 4]

Step 4: LIS process

* dp = [3]
* dp = [3, 4]
* dp = [3, 4, 7]
* replace 7 → dp = [3, 4, 4]

Length = 3


## Example 2:

Input:
height = [2, 1]
width  = [2, 1]

Pairs → (2,2), (1,1)
Sorted → (1,1), (2,2)

Widths → [1,2]

LIS:

* dp = [1]
* dp = [1,2]

Answer = 2
