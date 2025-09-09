# Intuition

We are given a number `NUM` and an array of positions. For each position `p` in `ARR`, we need to flip (invert) the bit at that position in `NUM`.
Flipping a bit at a specific position can be done using **XOR with a mask**:

* Mask = `1 << (pos-1)` (since positions are **1-based from the rightmost bit**).
* `num = num ^ mask` flips the bit.

---

# Approach

1. Loop through the given array `arr`.
2. For each position `p = arr[i]`, compute the mask:
   `mask = 1 << (p - 1)`.
3. XOR the current `num` with the mask → flips the bit at that position.
4. After processing all positions, return the final `num`.

---

# Complexity

* **Time Complexity**: `O(N)` (one loop through the array of size `N`).
* **Space Complexity**: `O(1)` (only a few extra variables used).

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {
	
	public static int flipSomeBits(int num, int[] arr, int n) {
		for (int i = 0; i < n; i++) {
			int pos = arr[i];              // position to flip
			int mask = 1 << (pos - 1);     // 1-based indexing
			num = num ^ mask;              // flip the bit
		}
		return num;
	}
}
```

---

# Example Walkthrough

### Example 1:

```
NUM = 21, ARR = [4, 2, 1]

Binary of 21 = 10101

Flip 4th bit:
10101 → 11101 (29)

Flip 2nd bit:
11101 → 11111 (31)

Flip 1st bit:
11111 → 11110 (30)

Final Answer = 30
```

### Example 2:

```
NUM = 40, ARR = [4]

Binary of 40 = 101000

Flip 4th bit:
101000 → 100000 (32)

Final Answer = 32
```

---

