# Intuition

A palindromic number reads the same forward and backward, such as `121`, `11`, `5`. Brute-forcing from `1` to `N` and checking each number would be too slow when `N` is close to `10^9`. So instead, we generate palindromic numbers **directly** and break the loop once they exceed `N`.

---

# Approach

1. All **single-digit numbers (1–9)** are palindromes.
2. We can **construct palindromes efficiently** by:

   * Creating half of the number and mirroring it to form the full number.
   * This can be done for **odd** and **even** length palindromes.
3. Stop when the generated palindrome exceeds `N`.

---

# Complexity

* **Time complexity**:

  * Efficient generation avoids iterating up to `N`.
  * We only generate valid palindromes ≤ `N`.
  * Number of palindromes ≤ `10^9` is manageable (\~100,000).
  * So, **almost O(√N)** in practice.

* **Space complexity**:

  * $O(k)$ where `k` is the number of palindromes generated ≤ `N`.

---

# Code

```java
import java.util.*;

public class Solution {
	public static int[] getPalindromes(int n) {
		Set<Integer> resultSet = new HashSet<>();

		// Single-digit palindromes
		for (int i = 1; i <= 9 && i <= n; i++) {
			resultSet.add(i);
		}

		// Even length palindromes
		for (int i = 1; ; i++) {
			String s = String.valueOf(i);
			String even = s + new StringBuilder(s).reverse();
			int evenPal = Integer.parseInt(even);
			if (evenPal > n) break;
			resultSet.add(evenPal);
		}

		// Odd length palindromes
		for (int i = 1; ; i++) {
			String s = String.valueOf(i);
			String odd = s + new StringBuilder(s).reverse().substring(1);
			int oddPal = Integer.parseInt(odd);
			if (oddPal > n) break;
			resultSet.add(oddPal);
		}

		// Sort result
		List<Integer> sortedList = new ArrayList<>(resultSet);
		Collections.sort(sortedList);
		return sortedList.stream().mapToInt(Integer::intValue).toArray();
	}
}

```

---
