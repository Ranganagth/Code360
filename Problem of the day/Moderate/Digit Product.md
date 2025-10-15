# Intuition

We are asked to find the **smallest number `X`** such that
the **product of its digits = N**.

If it’s not possible, we must return `-1`.

### Example

Let’s take `N = 36`.

We want to express `36` as a product of digits between `1` and `9`.
Since digits can only be from `0`–`9`, we must **factorize `N` into digits ≤ 9**.

---

# Approach

1. **If `N` < 10 → return `N + 10`**

   * Because for single-digit `N`, the smallest `X` is a two-digit number (like `N=5 → X=25` since 2×5=10, but this step is handled differently below).

2. **Factorize `N` using digits 9 → 2:**

   * Try to divide `N` by the largest digit from 9 down to 2.
   * If divisible, store that digit in a list and divide `N` by it.
   * Continue until `N` becomes 1.

3. **If `N` > 1 after the loop**,
   → not possible to represent it as product of digits (return `-1`).

4. **Sort collected digits in ascending order** to form the smallest possible number.

5. **Combine digits** to form the final integer result.

---

# Complexity

* **Time:** O(log₉(N)) roughly → very fast for N ≤ 10⁶
* **Space:** O(1)

---

# Code

```java
import java.util.* ;
import java.io.*; 

public class Solution {

	public static int digitProduct(int n) {
		// Case for single-digit numbers
		if (n >= 0 && n <= 9) {
			return n + 10;
		}

		List<Integer> digits = new ArrayList<>();

		// Factorize n using digits from 9 to 2
		for (int i = 9; i >= 2; i--) {
			while (n % i == 0) {
				digits.add(i);
				n /= i;
			}
		}

		// If n > 1, it cannot be represented as product of digits
		if (n > 1) {
			return -1;
		}

		// Sort to get the smallest possible number
		Collections.sort(digits);

		// Combine digits to form final number
		int result = 0;
		for (int d : digits) {
			result = result * 10 + d;
		}

		return result;
	}

	public static void main(String[] args) {
		System.out.println(digitProduct(36)); // 49
		System.out.println(digitProduct(42)); // 67
		System.out.println(digitProduct(10)); // 25
		System.out.println(digitProduct(21)); // 37
		System.out.println(digitProduct(13)); // -1
	}
};

```

---

## Example Walkthrough

### Example 1: `N = 36`

```
Start: N = 36
Try 9 → 36 % 9 == 0 → add 9, N = 36 / 9 = 4
Try 8 → skip (not divisible)
Try 4 → 4 % 4 == 0 → add 4, N = 1
Done.
Digits = [9, 4]
Sort ascending → [4, 9]
Result = 49
```

Output = `49`

### Example 2: `N = 42`

```
Start: N = 42
Try 9 → skip
Try 8 → skip
Try 7 → 42 % 7 == 0 → add 7, N = 6
Try 6 → 6 % 6 == 0 → add 6, N = 1
Digits = [7, 6]
Sort → [6, 7]
Result = 67
```

Output = `67`


### Example 3: `N = 13`

No digit between 2–9 divides 13 → not possible.
Output = `-1`

### **Sample Outputs**

| Input | Output |
| ----- | ------ |
| 36    | 49     |
| 42    | 67     |
| 10    | 25     |
| 21    | 37     |
| 13    | -1     |

---

