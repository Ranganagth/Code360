# Intuition

Fractional binary conversion works by repeatedly multiplying by 2:

* If result ≥ 1 → append ‘1’, subtract 1
* Else → append ‘0’

Some decimal fractions never terminate in binary (like 0.72).
Limit length to 32 bits. If not finished → return "ERROR".

---

# Approach

1. Initialize result as `"0."`
2. Repeat:

   * Multiply num by 2
   * If ≥ 1:

     * append '1'
     * num = num - 1
   * Else:

     * append '0'
3. Stop when:

   * num becomes 0 → exact representation found
   * length exceeds 32 → return "ERROR"
4. Return result

---

# Complexity

* **Time complexity:**
  $$O(32) = O(1)$$

* **Space complexity:**
  $$O(1)$$

---

# Code

```Java id="k2d9x1"
import java.util.*;
import java.io.*; 

public class Solution {
	public static String toBinaryCalculator(double num) {

		if (num <= 0 || num >= 1) return "ERROR";

		StringBuilder sb = new StringBuilder();
		sb.append("0.");

		while (num > 0) {

			if (sb.length() > 32) {
				return "ERROR";
			}

			num = num * 2;

			if (num >= 1) {
				sb.append('1');
				num = num - 1;
			} else {
				sb.append('0');
			}
		}

		return sb.toString();
	}
};

```

---

# Example Walkthrough

## Example 1: Step by step explanation

Input: 0.625

Step 1: 0.625 × 2 = 1.25 → ‘1’, num = 0.25
Step 2: 0.25 × 2 = 0.5 → ‘0’, num = 0.5
Step 3: 0.5 × 2 = 1.0 → ‘1’, num = 0

Result: **0.101**

## Example 2:

Input: 0.72

Binary expansion does not terminate within 32 bits

Result: **ERROR**
