# Intuition

SPECIAL_SUM(i) = FIRST_SUM(i) + LAST_SUM(i)

FIRST_SUM(i) = prefix[i]
LAST_SUM(i) = sum of elements from i → n-1

Instead of recomputing sums repeatedly, observe:

LAST_SUM(i) = totalSum - prefix[i-1] (for i > 0)
So,

SPECIAL_SUM(i) = prefix[i] + (totalSum - prefix[i-1])

Simplify:

For i > 0:
SPECIAL_SUM(i) = totalSum + arr[i]

For i = 0:
SPECIAL_SUM(0) = arr[0] + totalSum

Thus, the problem reduces to:

* For i = 0 → arr[0] + totalSum
* For i ≥ 1 → totalSum + arr[i]

So final answer:
minimum of (totalSum + arr[i]) for all i

---

# Approach

1. Compute total sum of array.
2. For each index i:

   * If i == 0:
     special = arr[0] + totalSum
   * Else:
     special = totalSum + arr[i]
3. Track minimum value across all i.
4. Return minimum.

No prefix/suffix arrays required.

---

# Complexity

* **Time complexity:**
  $$O(n)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
import java.util.*;

public class Solution {
	public static int specialSum(ArrayList<Integer> arr, int n) {

		int[] prefix = new int[n];

		prefix[0] = arr.get(0);
		for (int i = 1; i < n; i++) {
			prefix[i] = prefix[i - 1] + arr.get(i);
		}

		int total = prefix[n - 1];
		int ans = Integer.MAX_VALUE;

		for (int i = 0; i < n; i++) {

			int first = prefix[i];

			int last;
			if (i == n - 1) {
				last = total;
			} else {
				last = total - prefix[n - i - 2];
			}

			ans = Math.min(ans, first + last);
		}

		return ans;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

ARR = [1, 2, 3, 4]
totalSum = 10

i = 0 → 10 + 1 = 11
i = 1 → 10 + 2 = 12
i = 2 → 10 + 3 = 13
i = 3 → 10 + 4 = 14

Minimum = 11

But actual SPECIAL_SUM definition:
i = 0 → (1) + (1+2+3+4) = 1 + 10 = 11
i = 1 → (1+2) + (2+3+4) = 3 + 9 = 12
i = 2 → (1+2+3) + (3+4) = 6 + 7 = 13
i = 3 → (1+2+3+4) + (4) = 10 + 4 = 14

## Example 2:

ARR = [1, -2, -3, 4]
totalSum = 0

i = 0 → 0 + 1 = 1
i = 1 → 0 + (-2) = -2
i = 2 → 0 + (-3) = -3
i = 3 → 0 + 4 = 4

Minimum = -3
