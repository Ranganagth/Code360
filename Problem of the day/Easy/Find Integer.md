# Intuition

Numbers from `1 → N` are rearranged as:

* all **odd numbers first** (ascending)
* then all **even numbers** (ascending)

Count of odd numbers:
$$oddCount = \lceil N/2 \rceil = (N + 1) / 2$$

If `K` lies within odd positions → directly compute the K-th odd number.
Otherwise → compute the `(K - oddCount)`-th even number.

No array construction is required because `N` can be as large as $(10^{12})$.

---

# Approach

1. Compute number of odd numbers:

   ```
   oddCount = (N + 1) / 2
   ```
2. If `K <= oddCount`:

   * Kth odd number = `2*K - 1`
3. Otherwise:

   * position among evens = `K - oddCount`
   * number = `2 * (K - oddCount)`
4. Return result.

---

# Complexity

* **Time:** (O(1))
* **Space:** (O(1))

---

# Code

```java
import java.util.*;
import java.io.*;

public class Solution {

	public static int findInteger(int n, int k) {

		long N = n;
		long K = k;

		long oddCount = (N + 1) / 2;

		if (K <= oddCount) {
			return (int)(2 * K - 1); // Kth odd
		} else {
			return (int)(2 * (K - oddCount)); // even numbers
		}
	}
};

```

---

# Example walkthrough with explanation

Example: `N = 7`, `K = 4`

Odd numbers count:

$$(7+1)/2 = 4$$

Odd sequence = `[1,3,5,7]`

Since `K = 4 ≤ 4`:

$$2*K -1 = 2*4 -1 = 7$$

Answer = **7**.
