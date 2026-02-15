# Intuition

Binary addition follows the same principle as decimal addition: add digits from right to left while maintaining a carry. Since the strings may be large (up to 5000 length), conversion to integers is not possible; perform digit-by-digit addition.

---

# Approach

1. Use two pointers starting from the end of each string.
2. Maintain a `carry`.
3. At each step:

   * Add corresponding digits and carry.
   * Append `(sum % 2)` to the result.
   * Update `carry = sum / 2`.
4. Continue until both strings and carry are exhausted.
5. Reverse the built string to obtain the final result.

---

# Complexity

* **Time:** $(O(\max(n,m)))$
* **Space:** $(O(\max(n,m)))$

---

# Code

```java
public class Solution 
{
	public static String addBinaryString(String a, String b, int n, int m)
	{
		int i = n - 1;
		int j = m - 1;
		int carry = 0;

		StringBuilder res = new StringBuilder();

		while (i >= 0 || j >= 0 || carry != 0)
		{
			int sum = carry;

			if (i >= 0) sum += a.charAt(i--) - '0';
			if (j >= 0) sum += b.charAt(j--) - '0';

			res.append(sum % 2);
			carry = sum / 2;
		}

		return res.reverse().toString();
	}
};

```

---

# Example walkthrough with explanation

Input:
`a = "111"`, `b = "10"`

Process from right:

* 1 + 0 = 1 → result `1`, carry 0
* 1 + 1 = 2 → result `0`, carry 1
* 1 + 0 + carry(1) = 2 → result `0`, carry 1
* carry 1 remains → result `1`

Reverse → `"1001"`.
