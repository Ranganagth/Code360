# Intuition

We must convert an **integer `N`** into its **hexadecimal representation** (base 16).

Hexadecimal digits are:
`0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F`

For **positive numbers**, this is simple — repeatedly divide by 16 and collect remainders.

For **negative numbers**, we must handle the **two’s complement representation** of 32-bit integers:

* In Java, integers are 32-bit signed.
* So for negative numbers, we must interpret them as **unsigned 32-bit values**.
* Example:

  * `-1` → `FFFFFFFF`
  * `-50` → `FFFFFFCE`
  * `-20` → `FFFFFFEC`

To get this, we can **bit-mask** with `0xFFFFFFFF` to convert negative numbers to their unsigned 32-bit form.

---

# Approach

1. If `num == 0`, return `"0"`.
2. If `num` is negative, convert it using:

   ```
   num = num & 0xFFFFFFFF
   ```

   (This gives the unsigned 32-bit equivalent.)
3. Create a **hexadecimal character map**:

   ```
   "0123456789ABCDEF"
   ```
4. Use modulo (`num % 16`) and division (`num /= 16`) repeatedly to build the hex string.
5. Since we compute from least significant to most significant digit, reverse the final string.

---

# Complexity

* **Time:** O(log₁₆(N)) = O(8) (since 32 bits / 4 bits per hex digit)
* **Space:** O(1) (constant space for result string)

---

# Code

```java
public class Solution {
    public static String toHex(int num) {
        if (num == 0) return "0";

        // For negative numbers, convert to unsigned 32-bit
        long unsignedNum = num & 0xFFFFFFFFL;

        StringBuilder hex = new StringBuilder();
        char[] hexChars = "0123456789ABCDEF".toCharArray();

        while (unsignedNum > 0) {
            int digit = (int)(unsignedNum % 16);
            hex.append(hexChars[digit]);
            unsignedNum /= 16;
        }

        return hex.reverse().toString();
    }
}
```


---

## Example Walkthrough

### Example 1

```
Input: 14
```

Steps:

```
14 / 16 = 0 remainder 14 → E
Output: E
```

### Example 2

```
Input: -20
```

Binary (32-bit signed): `11111111 11111111 11111111 11101100`
→ Hexadecimal: `FFFFFFEC`

Output:

```
FFFFFFEC
```

---

**Sample Input**

```
3
14
100
-20
```

**Sample Output**

```
E
64
FFFFFFEC
```

---
