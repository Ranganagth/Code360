# Intuition

Scan bits from least significant to most significant.

Track positions of `1`s:

* When a `1` is found, compute distance from previous `1`
* Keep maximum distance

---

# Approach

1. Initialize:

   * `prev = -1` (last seen position of 1)
   * `pos = 0` (current bit position)
   * `maxDist = 0`
2. While `num > 0`:

   * If `(num & 1) == 1`:

     * If `prev != -1`:

       * update `maxDist = max(maxDist, pos - prev)`
     * update `prev = pos`
   * Right shift `num`
   * Increment `pos`
3. Return `maxDist`

---

# Complexity

* **Time complexity:**
  $$O(\log N)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {
	public static int binaryGap(int num) {

		int prev = -1;
		int pos = 0;
		int maxDist = 0;

		while (num > 0) {

			if ((num & 1) == 1) {
				if (prev != -1) {
					maxDist = Math.max(maxDist, pos - prev);
				}
				prev = pos;
			}

			num >>= 1;
			pos++;
		}

		return maxDist;
	}
}
```

---

# Example Walkthrough

## Example 1: Step by step explanation

NUM = 7

Binary: 111

Positions:

* bit 0 → 1
* bit 1 → 1 → distance = 1
* bit 2 → 1 → distance = 1

Max = 1

---

## Example 2:

NUM = 5

Binary: 101

Positions:

* bit 0 → 1
* bit 2 → 1 → distance = 2

Max = 2
