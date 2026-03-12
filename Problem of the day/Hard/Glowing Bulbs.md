# Intuition

A bulb glows if its number is divisible by **at least one prime switch label**.
Thus we must find the **K-th positive integer divisible by any number in `labels`**.

Let `count(x)` be the number of glowing bulbs ≤ `x`.

Using **Inclusion–Exclusion Principle**:

```
count(x) =
sum multiples of each prime
− sum multiples of pair LCMs
+ sum multiples of triple LCMs
...
```

Since labels are **distinct primes**, LCM of any subset is simply their **product**.

Then perform **binary search** to find the smallest `x` such that:

```
count(x) ≥ k
```

---

# Approach

1. Binary search range `[1, 1e18]`.
2. For each mid:

   * compute glowing bulbs ≤ mid using inclusion–exclusion.
3. If count ≥ k → move left.
4. Else → move right.
5. Return the smallest valid number.

Total subsets ≤ `2^10 = 1024`, which is feasible.

---

# Complexity

* **Time complexity:** $(O(2^N \log X))$
* **Space complexity:** $(O(1))$

Where $X ≈ 10^18$.

---
# Code

```java
import java.util.ArrayList;

public class Solution {

	public static long findKthGlowingBulb(ArrayList<Integer> labels, long k) {

		long left = 1;
		long right = (long)1e18;
		long ans = 0;

		while (left <= right) {

			long mid = left + (right - left) / 2;

			if (count(mid, labels) >= k) {
				ans = mid;
				right = mid - 1;
			} else {
				left = mid + 1;
			}
		}

		return ans;
	}

	private static long count(long x, ArrayList<Integer> labels) {

		int n = labels.size();
		long total = 0;

		int subsets = 1 << n;

		for (int mask = 1; mask < subsets; mask++) {

			long lcm = 1;
			int bits = 0;

			for (int i = 0; i < n; i++) {
				if ((mask & (1 << i)) != 0) {
					lcm *= labels.get(i);
					bits++;
				}
			}

			long add = x / lcm;

			if (bits % 2 == 1)
				total += add;
			else
				total -= add;
		}

		return total;
	}
};

```

---

# Example walkthrough

Example:

```
labels = [3,5,7]
k = 5
```

Binary search checks numbers while counting multiples.

Multiples sequence:

```
3,5,6,7,9,10,14,15...
```

5th glowing bulb:

```
9
```

Result:

```
9
```
