# Intuition

Two numbers `FIRST` and `SECOND` must satisfy:

```
FIRST * SECOND = NUM + 1   OR   NUM + 2
```

To minimize their absolute difference, the pair should be **closest to the square root** of the number.
The closest divisor pair of a number is obtained by searching from `sqrt(number)` downward.

---

# Approach

1. Compute `a = NUM + 1` and `b = NUM + 2`.
2. For each value:

   * start from `sqrt(value)` and move downward.
   * the first divisor found gives the closest pair.
3. Compute the pair for both `a` and `b`.
4. Return the pair with the **minimum absolute difference**.

---

# Complexity

* **Time complexity:** $(O(\sqrt{NUM}))$
* **Space complexity:** $(O(1))$

---
# Code

```java
import java.util.*;
import java.io.*;
import java.util.ArrayList;

import java.util.*;
import java.io.*;
import java.util.ArrayList;

public class Solution 
{
	public static ArrayList<Integer> closestDivisors(int num)
	{
		ArrayList<Integer> ans = new ArrayList<>();

		int[] pair1 = getPair(num + 1);
		int[] pair2 = getPair(num + 2);

		if (Math.abs(pair1[0] - pair1[1]) <= Math.abs(pair2[0] - pair2[1])) {
			ans.add(pair1[0]);
			ans.add(pair1[1]);
		} else {
			ans.add(pair2[0]);
			ans.add(pair2[1]);
		}

		return ans;
	}

	private static int[] getPair(int x)
	{
		int root = (int)Math.sqrt(x);

		for (int i = root; i >= 1; i--) {
			if (x % i == 0) {
				return new int[]{i, x / i};
			}
		}

		return new int[]{1, x};
	}
};

```

---

# Example walkthrough with explanation

Example:

```
NUM = 10
```

Check:

```
NUM+1 = 11 → factors (1,11) → difference = 10
NUM+2 = 12 → factors (3,4)  → difference = 1
```

Minimum difference pair:

```
3 4
```
