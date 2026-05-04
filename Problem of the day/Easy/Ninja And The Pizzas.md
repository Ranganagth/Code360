# Intuition

Cutting a rectangle adds **new boundary (perimeter)**.

For a rectangle $(l \times b)$:

* If cut parallel to length → gain = $(2 \times b)$
* If cut parallel to breadth → gain = $(2 \times l)$

To maximize cheese:

* For each slice, choose **max(l, b)**
* Contribution = $(2 \times \max(l, b))$

Pick top K such contributions.

---

# Approach

1. For each slice:

   * Compute `gain[i] = 2 * max(l[i], b[i])`
2. Sort gains in descending order
3. Pick top K values
4. Sum them

---

# Complexity

* **Time complexity:**
  $$O(N \log N)$$

* **Space complexity:**
  $$O(N)$$

---

# Code

```Java
import java.util.*;
import java.io.*; 

public class Solution {
	public static int maxExtraCheese(int n, int k, int[] l, int[] b) {

		int[] gain = new int[n];

		for (int i = 0; i < n; i++) {
			gain[i] = 2 * Math.max(l[i], b[i]);
		}

		Arrays.sort(gain);

		int result = 0;

		for (int i = n - 1; i >= n - k; i--) {
			result += gain[i];
		}

		return result;
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

L = [4, 2]
B = [6, 8]

Slice 1:
max(4,6) = 6 → gain = 12

Slice 2:
max(2,8) = 8 → gain = 16

Pick top 1:
16

Answer = 16

---

## Example 2:

L = [4]
B = [6]

max(4,6) = 6 → gain = 12

Answer = 12
