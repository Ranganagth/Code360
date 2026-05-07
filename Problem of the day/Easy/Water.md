# Intuition

Final state: all remaining buckets have equal value = (x).

Allowed:

* Remove entire bucket → lose full value
* Reduce bucket → lose difference

For a chosen (x):

* Keep buckets with value ≥ (x)
* Reduce them to (x)
* Remove all buckets < (x)

Goal:
Minimize total removed water

Equivalent:
Maximize total water **kept**:
$$
\text{kept} = x \times (\text{count of buckets ≥ x})
$$

Total removed:
$$
\text{totalSum} - \text{kept}
$$

---

# Approach

1. Sort array
2. Compute total sum
3. For each index (i):
   * Let $(x = arr[i])$
   * Buckets $≥ x = (n - i)$
   * kept = $(x \times (n - i))$
4. Track maximum kept
5. Answer = totalSum − maxKept

---

# Complexity

* **Time complexity:**  $$O(n \log n)$$

* **Space complexity:**  $$O(1)$$

---

# Code

```Java
import java.util.*;

public class Solution {
	public static int minWaterRemoved(ArrayList<Integer> arr, int n) {

		Collections.sort(arr);

		long total = 0;
		for (int val : arr) total += val;

		long maxKept = 0;

		for (int i = 0; i < n; i++) {
			long x = arr.get(i);
			long count = n - i;
			long kept = x * count;

			maxKept = Math.max(maxKept, kept);
		}

		return (int)(total - maxKept);
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

ARR = [1,1,2,2]

Sorted: [1,1,2,2]
Total = 6

Try x = 1:
kept = 1 × 4 = 4

Try x = 2:
kept = 2 × 2 = 4

Max kept = 4

Removed = 6 - 4 = 2

---

## Example 2:

ARR = [1,2,3]

Sorted: [1,2,3]
Total = 6

x = 1 → kept = 1×3 = 3
x = 2 → kept = 2×2 = 4
x = 3 → kept = 3×1 = 3

Max kept = 4

Removed = 6 - 4 = 2
